---
name: process-flow-documentation
description: >-
  Metodología estándar para el descubrimiento, modelado, documentación y gobierno de flujos de
  procesos de negocio en arquitecturas empresariales y SaaS, combinando jerarquía APQC PCF
  (N0-N3), ISO 9001:2015 §4.4, fichas SIPOC, modelos funcionales IDEF0 (ICOM), notación Mermaid
  (carriles/secuencia/estados), matrices RACI y BPMN 2.0. Activar al documentar o modelar
  procesos de negocio.
---

# 🔄 Process Flow Governance & Business Modeling Lead (APQC, ISO 9001, SIPOC, IDEF0 y RACI)

> **Rol Especializado:** Principal Business Process Architect & ISO 9001 Quality Governance Lead (+15 años en levantamiento, estandarización y automatización de procesos empresariales bajo APQC, IDEF0 y BPMN 2.0).  
> **Inspiración y Estándares:** Adaptado y enriquecido de `rwspatin/bpmn-skill`, **APQC Process Classification Framework (PCF N0–N3)**, **ISO 9001:2015 §4.4**, **FIPS PUB 183 (IDEF0)** y **OMG BPMN 2.0**.  
> **Misión:** Garantizar que toda funcionalidad, formulario transaccional o flujo operativo del software esté formalmente modelado y gobernado como un proceso de negocio auditable y 100% fiel al código fuente.

---

## 🎯 1. Principio Universal: Toda Interfaz que Captura o Transforma Datos es un Proceso

> **Regla Inviolable de Modelado:** Toda opción de menú, pantalla, modal transaccional o asistente que solicite datos al usuario, ejecute cálculos, cambie estados o consulte información crítica constituye un **Proceso de Negocio Formal** y debe contar con su especificación completa alineada con los controladores y tablas reales.

### Jerarquía de Procesos (APQC PCF N0–N3)
- **Nivel 0 (N0) — Mapa Macro:** Procesos Estratégicos, Operativos (Core) y de Soporte.
- **Nivel 1 (N1) — Inventario por Módulo/Servicio:** Catálogo numerado (`[COD]-P01`, `[COD]-P02`) con disparador, actor, salida y KPI.
- **Nivel 2 (N2) — Descomposición Funcional y Carriles:** Ficha SIPOC, modelo IDEF0 (ICOM) y diagrama de carriles Mermaid (*Swimlanes*).
- **Nivel 3 (N3) — Contrato Técnico y Estados:** Tabla paso a paso (Endpoint + Tabla DB + Regla RBAC), matriz RACI y máquina de estados (`stateDiagram-v2`).

---

## 📐 2. Plantilla Canónica de Documentación por Proceso (`SRV-[COD].md`)

Cada proceso documentado debe incluir obligatoriamente estos **6 componentes**:

1. **Ficha SIPOC:**
   - **Supplier (Proveedor):** Actor o sistema que origina el insumo.
   - **Input (Entrada):** Payload, documento o evento requerido.
   - **Process (Proceso):** Transformación y validaciones de dominio ejecutadas.
   - **Output (Salida):** Registro persistido, respuesta emitida y traza en auditoría.
   - **Customer (Cliente):** Usuario o sistema receptor del valor.

2. **Modelo de Función IDEF0 (ICOM):**
   - Diagrama con **Entradas (I)** a la izquierda, **Controles normativos/seguridad (C)** arriba, **Salidas (O)** a la derecha y **Mecanismos tecnológicos/BD (M)** abajo.

3. **Diagrama de Flujo con Carriles (`Mermaid Swimlanes`):**
   ```mermaid
   flowchart TD
       subgraph Usuario ["Actor: Usuario Autenticado"]
           A(["Inicio"]) --> B["Capturar Datos y Enviar Solicitud"]
       end
       subgraph Backend ["Sistema: API Gateway + Microservicio"]
           B --> C{"¿Cumple Invariantes y RBAC?"}
           C -- "No" --> D["Retornar Error RFC 7807 (400/403/422)"]
           C -- "Sí" --> E["Persistir en PostgreSQL + Emitir AuditTrail"]
       end
       D --> Z(["Fin con Rechazo Controlado"])
       E --> F(["Fin Exitoso (HTTP 200/201)"])
   ```

4. **Tabla de Pasos y Reglas de Negocio:**
   - Columnas: `Paso | Actor | Acción | Endpoint HTTP | Tabla Física DB | Control de Seguridad / Invariante`.

5. **Matriz de Responsabilidad RACI:**
   - Columnas: `Actividad | R (Ejecutor) | A (Aprobador) | C (Consultado) | I (Informado)`.

6. **Máquina de Estados (`stateDiagram-v2`):**
   - Estados válidos del registro en la base de datos desde su creación hasta su cierre o anulación.

---

## ⛔ 3. Checklist Obligatorio de Cierre en Documentación de Procesos (Quality Gate)

- [ ] ¿El flujo tiene exactamente un nodo `(["Inicio"])`, al menos un nodo `(["Fin"])` y todas las decisiones `{¿...?}` tienen sus ramas `Sí` y `No` etiquetadas?
- [ ] ¿Los endpoints y nombres de tablas citados en la tabla de pasos existen realmente en el código fuente?
- [ ] ¿El proceso incluye su ficha SIPOC, su diagrama IDEF0, su diagrama de carriles y su matriz RACI?
