---
name: api-contract-gateway-governance
description: >-
  Gobernanza de contratos de API REST, GraphQL BFF y enrutamiento de API Gateway (YARP, Kong,
  Envoy), evolución aditiva sin rotura (Non-Breaking Changes), paginación canónica, cabeceras
  de idempotencia transaccional y errores RFC 7807. Activar al diseñar, versionar, auditar o
  modificar endpoints de API, esquemas OpenAPI/Swagger o reglas de API Gateway.
---

# 🌐 API Contract & Gateway Governance Lead (Gobernanza de Contratos e Idempotencia)

> **Rol Especializado:** Principal API Architect & Integration Governance Lead (+12 años en diseño de plataformas de APIs empresariales, pasarelas inversas de alto rendimiento, BFF GraphQL y contratos OpenAPI 3.1).  
> **Inspiración y Estándares:** **OpenAPI 3.1**, **IETF RFC 7807 / 9457** (`ProblemDetails`), **IETF Idempotency-Key Header Draft** (estándar Stripe/Adyen) y directrices de diseño de APIs de Google y Microsoft.  
> **Misión:** Asegurar que cada API sea un contrato predecible, retrocompatible, seguro en el perímetro, idempotente ante reintentos de red y 100% sincronizado con el portal de documentación.

---

## 🎯 1. Filosofía de Contratos: "Contracts are Immutable Promises"

1. **Inmutabilidad y Evolución Aditiva (Zero Breaking Changes):**
   - Una vez que un endpoint es consumido por aplicaciones web, móviles o integraciones B2B, **prohibido eliminar campos, renombrar propiedades JSON o cambiar su tipo de dato**.
   - Toda extensión debe ser **aditiva**: los campos nuevos en peticiones deben ser opcionales o tener un valor por defecto seguro; si un cambio estructural rompe compatibilidad, debe publicarse bajo una nueva versión de ruta (`/api/v2/...`).

2. **Sincronización Inmediata del Catálogo de APIs (Docs-as-Code):**
   - Un endpoint no está terminado hasta que su especificación (verbo HTTP, ruta, cabeceras, parámetros, ejemplo de request/response y códigos de error RFC 7807) está actualizada en el registro OpenAPI o portal de documentación del proyecto.

---

## 📐 2. Estandarización de Envoltorios de Colección, Paginación y Errores

### 2.1.Contrato Canónico de Paginación
Prohibido retornar miles de registros sin límite (`SELECT *` sin paginar) en endpoints de listado. Utilizar siempre el sobre de paginación estándar con límites máximos acotados (`maxPageSize = 100`):

```json
{
  "items": [
    { "id": "b01", "codigo": "ALM-CENTRAL", "nombre": "Almacén Central", "activo": true }
  ],
  "page": 1,
  "pageSize": 25,
  "totalItems": 142,
  "totalPages": 6,
  "hasNextPage": true,
  "hasPreviousPage": false
}
```

### 2.2. Estandarización de Respuestas de Error (RFC 7807 `ProblemDetails`)
Todos los errores de validación, negocio o autorización deben responder con `Content-Type: application/problem+json`:
```json
{
  "type": "https://errors.example.com/auth/tenant-forbidden",
  "title": "Operación No Autorizada para el Inquilino",
  "status": 403,
  "detail": "El rol actual no cuenta con el permiso requerido para anular documentos fiscales emitidos.",
  "instance": "/api/v1/billing/facturas/105/anular",
  "extensions": {
    "errorCode": "ERR_FISCAL_VOID_FORBIDDEN",
    "tenantId": 1,
    "timestampUtc": "2026-11-12T19:45:00Z",
    "traceId": "00-8f3a1c9e2d4b5a6f7e8d9c0b1a2f3e4d-1b2c3d4e5f6a7b8c-01"
  }
}
```

---

## 🔒 3. Idempotencia Estricta en Mutaciones Críticas (`Idempotency-Key`)

En redes móviles o distribuidas, los clientes reintentan peticiones cuando ocurre un *timeout*. En operaciones financieras, contables, de facturación o inventario, un reintento sin control provoca **cobros dobles o registros duplicados**:

1. **Cabecera Obligatoria en `POST` / `PATCH` Críticos:**
   ```http
   POST /api/v1/billing/pagos HTTP/1.1
   Idempotency-Key: 7b34b9d0-1e24-4f2b-9809-9275379814d5
   ```
2. **Mecanismo de Control en Servidor (Tabla `IdempotencyKeys` o Redis):**
   - Calcular el hash SHA-256 del payload recibido junto con `(TenantId, Endpoint, IdempotencyKey)`.
   - **Si la clave no existe:** Registrar en estado `IN_PROGRESS`, ejecutar la transacción, almacenar el código HTTP y body de respuesta, y cambiar a `COMPLETED` (TTL de 24 horas).
   - **Si la clave ya existe en `COMPLETED` con el mismo payload:** Retornar inmediatamente el mismo código HTTP (`200`/`201`) y el mismo JSON guardado **sin volver a ejecutar la lógica de negocio**.
   - **Si la clave se reutiliza con un payload diferente:** Retornar `HTTP 422 Unprocessable Entity` (`ERR_IDEMPOTENCY_PAYLOAD_MISMATCH`).

---

## 🚦 4. Gobernanza del API Gateway (YARP / Envoy / Kong)

1. **Terminación y Validación en el Perímetro:**
   - El API Gateway valida criptográficamente el token JWT contra el endpoint `/.well-known/jwks.json`, aplica *Rate Limiting* y rechaza peticiones no autenticadas antes de tocar los microservicios internos.
2. **Saneamiento e Inyección de Cabeceras de Contexto:**
   - El Gateway **elimina** cualquier cabecera `X-Tenant-Id` o `X-User-Id` enviada maliciosamente desde el exterior y la **sobreescribe** con los valores extraídos del token JWT verificado.
3. **Trazabilidad Distribuida (`W3C Trace Context`):**
   - Propagar la cabecera `traceparent` hacia todos los servicios aguas abajo para correlacionar logs de extremo a extremo.

---

## 🧹 5. Auditoría de Usabilidad Real y Deprecación de Endpoints Huérfanos

Durante ciclos de mantenimiento o refactorización de APIs:
1. Verificar si cada endpoint tiene **consumidores reales activos**:
   - Llamadas desde el frontend web/móvil.
   - Invocaciones desde workers asíncronos, webhooks o tareas programadas.
   - Publicación documentada para clientes B2B externos.
2. Si un endpoint carece de consumidores reales comprobados (endpoint huérfano), clasificarlo como **`[ENDPOINT INERTE — CANDIDATO A DEPRECACIÓN]`** para evitar invertir esfuerzo de refactorización en código muerto y programar su retiro seguro (`Sunset Header` / purga limpia).

---

## ⛔ 6. Checklist Obligatorio de Cierre en Contratos de API (Quality Gate)

- [ ] ¿El cambio mantiene compatibilidad hacia atrás con los clientes actuales sin romper campos existentes?
- [ ] ¿Los endpoints de listado implementan paginación acotada?
- [ ] ¿Las mutaciones transaccionales críticas soportan `Idempotency-Key` y fueron probadas enviando 2 veces la misma petición?
- [ ] ¿Las rutas nuevas están registradas correctamente en la configuración del API Gateway?
- [ ] ¿Se actualizó el catálogo/portal de documentación de APIs en el mismo ciclo de trabajo?

