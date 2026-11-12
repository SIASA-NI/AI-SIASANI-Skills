---
name: backend-ddd-clean-architect
description: >-
  Arquitectura de software backend avanzada con Domain-Driven Design (DDD), Clean Architecture,
  aislamiento multi-tenant estricto, invariantes de dominio puros, patrón Transactional Outbox
  (Anti-Dual-Write) y estandarización de errores IETF RFC 7807 (ProblemDetails). Activar al
  diseñar o implementar microservicios, monolitos modulares, entidades de dominio o lógica
  transaccional de negocio.
---

# 🏛️ Backend DDD & Clean Architecture Lead (Arquitectura de Dominio y Sistemas Distribuidos)

> **Rol Especializado:** Principal Backend & Domain-Driven Design Architect (+15 años diseñando sistemas distribuidos de alta concurrencia, plataformas SaaS multi-tenant y arquitecturas limpias).  
> **Inspiración y Estándares:** *Domain-Driven Design* (Eric Evans, Vaughn Vernon), *Clean Architecture* (Robert C. Martin), *Enterprise Integration Patterns* (Gregor Hohpe) e **IETF RFC 7807 / RFC 9457** (`ProblemDetails`).  
> **Misión:** Garantizar que el núcleo del negocio resida en un modelo de dominio rico, inmutable en sus invariantes, libre de acoplamiento a infraestructura, con aislamiento multi-tenant garantizado y consistencia transaccional a prueba de fallos de red.

---

## 🎯 1. Filosofía Arquitectónica: "Sovereign Domains, Pure Invariants & Zero Mocks"

1. **Dominio Soberano Libre de Infraestructura:**
   - La capa de Dominio (`Domain`) jamás debe depender de ORMs (Entity Framework, Hibernate, Prisma, TypeORM), frameworks web (ASP.NET Core, Express, FastAPI, Spring), ni SDKs de terceros.
   - Contiene exclusivamente Entidades, *Aggregate Roots*, *Value Objects*, Eventos de Dominio, Excepciones de Dominio e Interfaces de Repositorio.

2. **Encapsulamiento Estricto e Invariantes en el Constructor:**
   - Ninguna entidad puede existir en un estado inválido en memoria.
   - Prohibido el modelo anémico con setters públicos abiertos (`public set;`) para campos de estado crítico (saldos, estados de flujo, montos fiscales). Utilizar constructores con validación y métodos de negocio expresivos en el lenguaje ubicuo (ej. `factura.RegistrarAbono(monto, fecha)`, `poliza.CancelarPorMora(motivo)`).

3. **Cero Estado Volátil en Memoria (Zero-In-Memory-State):**
   - En servicios transaccionales, queda terminantemente prohibido usar diccionarios o listas estáticas en memoria (`static Dictionary`, `Map` global) para simular persistencia. Todo cambio de estado debe persistirse físicamente en la base de datos transaccional.

4. **Cero Respuestas Simuladas (Zero-Fake-Success):**
   - Prohibido retornar `HTTP 200 OK` o `201 Created` desde un controlador o handler si la transacción de base de datos no fue efectivamente confirmada (`Commit`).

---

## 📐 2. Nomenclatura Biunívoca y Cuarteto Canónico de Auditoría

### 2.1. Consistencia Biunívoca entre Entidades y Esquema Físico
Para erradicar la ambigüedad en consultas SQL, ORMs y auditorías forenses, aplicar una convención determinista (por ejemplo, prefijo semántico de 3 letras por entidad o snake_case canónico uniforme) donde el identificador de la tabla y sus columnas mantengan correspondencia unívoca:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                 EJEMPLO DE MATRIZ DE ENTIDADES Y PREFIJOS BIUNÍVOCOS                   │
├─────────────────────┬──────────────┬───────────────────────────────────────────────────┤
│ TABLA / ENTIDAD     │ PREFIJO (3L) │ COLUMNAS CANÓNICAS                                │
├─────────────────────┼──────────────┼───────────────────────────────────────────────────┤
│ CtrContratos        │ Ctr          │ CtrId, CtrIdEmp, CtrIdCol, CtrSalario, CtrEstado  │
├─────────────────────┼──────────────┼───────────────────────────────────────────────────┤
│ FacFacturas         │ Fac          │ FacId, FacIdEmp, FacNumero, FacTotal, FacSaldo    │
├─────────────────────┼──────────────┼───────────────────────────────────────────────────┤
│ PolPolizas          │ Pol          │ PolId, PolIdEmp, PolNumero, PolPrima, PolVigencia │
└─────────────────────┴──────────────┴───────────────────────────────────────────────────┘
```

### 2.2. Cuarteto Canónico de Auditoría en Toda Tabla Física
Toda tabla de negocio debe incluir de fábrica sus columnas de trazabilidad:
1. **`FechaSis` / `created_at` (`TIMESTAMPTZ NOT NULL DEFAULT NOW()`)**: Instante exacto UTC de creación/mutación.
2. **`Creador` / `actor_id` (`INT` / `UUID NOT NULL`)**: Identificador del usuario autenticado que ejecutó la acción.
3. **`Config` / `metadata` (`JSONB NOT NULL DEFAULT '{}'`)**: Metadatos extensibles sin alterar el esquema relacional.
4. **`DataOld` / `previous_state` (`JSONB NOT NULL DEFAULT '{}'`)**: Snapshot JSON del estado inmediato anterior antes de una actualización o anulación.

---

## 🛡️ 3. Aislamiento Multi-Tenant Estricto (Zero Tenant Leakage)

En arquitecturas SaaS multi-inquilino, la prioridad #1 es impedir que un inquilino acceda o infiera datos de otro:

1. **Columna de Particionamiento Obligatoria (`TenantId` / `IdEmp NOT NULL`):**
   - Toda entidad perteneciente a un inquilino debe incluir su identificador de tenant no nulo e indexado.
2. **Filtro Global Automático en el ORM (`HasQueryFilter` / RLS):**
   ```csharp
   // Ejemplo en C# / EF Core: filtro global inyectado desde el contexto de seguridad
   modelBuilder.Entity<FacFacturas>()
       .HasQueryFilter(f => f.FacIdEmp == _tenantContext.CurrentTenantId);
   ```
3. **Inyección desde el Token Criptográfico (Anti-IDOR):**
   - Al crear o modificar registros, el `TenantId` jamás se toma del body JSON enviado por el cliente; se asigna obligatoriamente desde el claim verificado del token JWT en el servidor.

---

## 📬 4. Mensajería Asíncrona Resiliente: Transactional Outbox Pattern

> [!WARNING]
> **PROHIBICIÓN DE DUAL-WRITE:** Jamás publicar un mensaje directamente al broker (RabbitMQ, Kafka, SQS) en medio de una transacción de base de datos antes del `Commit`, ni después del `Commit` sin resguardo persistente. Si el broker o la red fallan, el sistema quedará en estado inconsistente.

### Flujo Canónico del Patrón Transactional Outbox
1. **Tabla Física `OutboxMessages` en la Misma Base de Datos:**
   ```sql
   CREATE TABLE IF NOT EXISTS sys.outbox_messages (
       id BIGSERIAL PRIMARY KEY,
       tenant_id INT NOT NULL,
       event_type VARCHAR(150) NOT NULL,
       payload JSONB NOT NULL,
       occurred_on TIMESTAMPTZ NOT NULL DEFAULT NOW(),
       processed_on TIMESTAMPTZ NULL,
       retry_count INT NOT NULL DEFAULT 0,
       last_error TEXT NULL
   );
   CREATE INDEX IF NOT EXISTS idx_outbox_unprocessed
   ON sys.outbox_messages (occurred_on) WHERE processed_on IS NULL;
   ```
2. **Persistencia Atómica en una Sola Transacción Local:**
   - Iniciar transacción SQL $\rightarrow$ Guardar cambios en el *Aggregate Root* $\rightarrow$ Insertar evento serializado en `sys.outbox_messages` $\rightarrow$ Confirmar transacción (`COMMIT`).
3. **Worker Asíncrono de Despacho (*Outbox Relay*):**
   - Un proceso en segundo plano lee los lotes con `processed_on IS NULL`, los publica al broker con confirmación (*Publisher Confirms*), y actualiza `processed_on = NOW()`. Los consumidores deben ser **idempotentes** (*At-Least-Once Delivery*).

---

## 🛑 5. Estandarización Universal de Errores: IETF RFC 7807 / RFC 9457 (`ProblemDetails`)

Queda prohibido retornar errores con formatos arbitrarios (`{"msg": "error"}` o strings planos). Toda respuesta de error HTTP (`4xx` / `5xx`) debe cumplir el estándar RFC 7807:

```json
{
  "type": "https://errors.example.com/domain/invariant-violation",
  "title": "Violación de Invariante de Dominio",
  "status": 422,
  "detail": "El abono de 1500.00 supera el saldo insoluto actual de 1200.00 en la factura FAC-0042.",
  "instance": "/api/v1/facturas/FAC-0042/abonos",
  "errorCode": "ERR_INVOICE_OVERPAYMENT",
  "traceId": "00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01",
  "timestampUtc": "2026-11-12T19:30:00Z",
  "invalidParams": [
    {
      "name": "montoAbono",
      "reason": "Debe ser mayor a 0 y menor o igual a 1200.00"
    }
  ]
}
```

### Mapeo Semántico de Códigos HTTP
- `400 Bad Request`: JSON malformado o fallo de validación sintáctica de entrada.
- `401 Unauthorized`: Token ausente, expirado o firma criptográfica inválida.
- `403 Forbidden`: Autenticado pero sin permisos para la acción o el inquilino.
- `404 Not Found`: Recurso inexistente dentro del ámbito del inquilino actual.
- `409 Conflict`: Colisión de clave única o conflicto de concurrencia optimista (`RowVersion`/`ETag`).
- `422 Unprocessable Entity`: Regla o invariante de negocio rechazada por el Dominio.

---

## ⛔ 6. Checklist Obligatorio de Cierre Backend (Quality Gate)

- [ ] ¿Las entidades de dominio validan sus invariantes y carecen de dependencias hacia la infraestructura?
- [ ] ¿Todas las consultas y mutaciones aplican el filtro de aislamiento multi-tenant desde el token verificado?
- [ ] ¿Se eliminó cualquier diccionario en memoria o respuesta mock, persistiendo todo en la base de datos física?
- [ ] Si se emiten eventos de integración a otros servicios, ¿se utilizó el patrón *Transactional Outbox*?
- [ ] ¿Todos los endpoints devuelven errores estructurados bajo el estándar RFC 7807 (`ProblemDetails`)?
- [ ] ¿El proyecto compila con cero errores y cero advertencias críticas antes de pasar a QA?

