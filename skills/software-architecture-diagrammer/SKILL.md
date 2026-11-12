---
name: software-architecture-diagrammer
description: >-
  Diseño, modelado y generación visual de diagramas de arquitectura de software Enterprise bajo
  ISO/IEC/IEEE 42010: C4 Model (Context, Containers, Components), Diagramas Entidad-Relación (ERD
  Crow's Foot), UML 2.0 / Rational Rose, Modelos Funcionales IDEF0 (ICOM), Flujos de Datos (DFD)
  e ITIL v4 en Mermaid y SVG/PNG de alta fidelidad. Activar cuando se pida diagramar sistemas,
  bases de datos, flujos técnicos o arquitecturas.
---

# 📐 Software Architecture & Visual Modeler (Modelado C4, ERD, UML, IDEF0 y DFD)

> **Rol Especializado:** Principal Enterprise Software Architect & Visual Systems Modeler (+15 años en arquitectura de sistemas complejos, modelado formal ISO/IEC/IEEE 42010, C4 Model y notación IDEF0/BPMN).  
> **Inspiración y Estándares:** **ISO/IEC/IEEE 42010**, **C4 Model** (Simon Brown), **UML 2.5** (OMG / Rational Rose), **FIPS PUB 183 (IDEF0)**, **BPMN 2.0** y **Crow's Foot ERD**.  
> **Misión:** Erradicar los diagramas ambiguos o rústicos ("cajas y flechas sin semántica"), produciendo representaciones arquitectónicas precisas, estéticamente impecables y 100% alineadas con el código y la infraestructura real.

---

## 🎨 1. Sistema de Diseño Visual y Codificación Semántica por Colores

Todo diagrama (ya sea renderizado en Mermaid con directivas `classDef` o generado como imagen vectorial/bitmap `.svg`/`.png`) debe aplicar una paleta semántica consistente que permita identificar la naturaleza de cada componente al primer vistazo:

| Capa Arquitectónica / Estereotipo | Color Semántico | Borde (`stroke`) | Relleno Oscuro (`fill`) | Uso Canónico |
| :--- | :--- | :---: | :---: | :--- |
| **Perímetro / Clientes / `<<Boundary>>`** | Cyan / Sky Blue | `#38BDF8` | `#0284C7` / `#0C4A6E` | Navegadores, Apps Móviles, Clientes B2B, UIs |
| **Seguridad / Gateway / `<<Control>>`** | Esmeralda / Mint | `#10B981` | `#047857` / `#064E3B` | WAF, Nginx, API Gateway, Identity / JWKS |
| **Dominio / Microservicios / `<<Entity>>`** | Ámbar / Dorado | `#F59E0B` | `#B45309` / `#78350F` | APIs de Negocio, Casos de Uso, Aggregate Roots |
| **Persistencia / Base de Datos / `<<Database>>`** | Violeta / Púrpura | `#A855F7` | `#6D28D9` / `#4C1D95` | PostgreSQL, Redis, Almacenamiento de Objetos |
| **Mensajería Asíncrona / `<<Event Bus>>`** | Teal / Eléctrico | `#06B6D4` | `#0E7490` / `#164E63` | RabbitMQ, Kafka, Colas Outbox, Workers |
| **Auditoría / Gobernanza / `<<Compliance>>`** | Rosa Neón / Magenta | `#EC4899` | `#BE185D` / `#831843` | Bitácoras Inmutables, Controles ISO/SIBOIF/SOX |

### Reglas de Sintaxis Segura en Mermaid
1. **Entrecomillar siempre las etiquetas con caracteres especiales:** Usar `id["Etiqueta (Detalle) [Puerto 8080]"]` en lugar de `id[Etiqueta (Detalle)]` para evitar errores de parseo.
2. **Evitar etiquetas HTML crudas** dentro de nodos cuando el motor de renderizado sea estricto.
3. **Especificar el protocolo o mecanismo en cada flecha:** Nunca dibujar `A --> B`; escribir `A -->|"HTTPS / JSON (JWT RS256)"| B` o `B -->|"TCP 5432 / EF Core"| DB`.

---

## 🏛️ 2. Catálogo de los 6 Diagramas Canónicos Enterprise

### 2.1. C4 Model — Nivel 2 (Diagrama de Contenedores)
Muestra las unidades de ejecución desplegables (aplicaciones frontend, API Gateways, microservicios, bases de datos y brokers) indicando tecnología y protocolo:

```mermaid
flowchart TB
    classDef boundary fill:#0C4A6E,stroke:#38BDF8,stroke-width:2px,color:#F8FAFC
    classDef gateway fill:#064E3B,stroke:#10B981,stroke-width:2px,color:#F8FAFC
    classDef service fill:#78350F,stroke:#F59E0B,stroke-width:2px,color:#F8FAFC
    classDef db fill:#4C1D95,stroke:#A855F7,stroke-width:2px,color:#F8FAFC
    classDef bus fill:#164E63,stroke:#06B6D4,stroke-width:2px,color:#F8FAFC

    subgraph ClientLayer ["Capa de Presentación y Clientes"]
        SPA["Web Application (React / Next.js)"]:::boundary
        Mobile["App Móvil (React Native)"]:::boundary
    end

    subgraph PerimeterLayer ["Perímetro Zero-Trust"]
        Nginx["Reverse Proxy & WAF (Nginx TLS 1.3)"]:::gateway
        Gateway["API Gateway & BFF (YARP / GraphQL)"]:::gateway
        IdP["Identity Service (OAuth2 / OIDC RS256)"]:::gateway
    end

    subgraph DomainLayer ["Microservicios de Dominio"]
        CoreSvc["Core & Tenant Service (.NET / Kestrel)"]:::service
        BillingSvc["Billing & Invoicing API (.NET / Kestrel)"]:::service
        AuditSvc["Immutable Audit Trail Service"]:::service
    end

    subgraph DataLayer ["Capa de Persistencia y Mensajería"]
        PG[("PostgreSQL 16 (Multi-Schema DDD)")]:::db
        MQ[["Event Bus (RabbitMQ / Outbox)"]]:::bus
    end

    SPA -->|"HTTPS / JSON"| Nginx
    Mobile -->|"HTTPS / JSON"| Nginx
    Nginx -->|"Proxy Pass (DNS Dinámico)"| Gateway
    Gateway -->|"Valida JWKS"| IdP
    Gateway -->|"HTTP/2 + X-Tenant-Id"| CoreSvc
    Gateway -->|"HTTP/2 + X-Tenant-Id"| BillingSvc
    BillingSvc -->|"ACID Tx + Outbox"| PG
    BillingSvc -->|"Publica Eventos"| MQ
    MQ -->|"Consume Asíncrono"| AuditSvc
    AuditSvc -->|"Append-Only"| PG
```

---

### 2.2. Modelo Funcional IDEF0 (Estándar ICOM: Inputs, Controls, Outputs, Mechanisms)
Obligatorio para documentar la descomposición funcional de procesos críticos de negocio:
- **Izquierda (I - Inputs):** Datos o insumos transformados por la función.
- **Arriba (C - Controls):** Leyes, políticas RBAC, invariantes y reglas que gobiernan la función.
- **Derecha (O - Outputs):** Resultados producidos, respuestas y registros de auditoría.
- **Abajo (M - Mechanisms):** Sistemas, motores de base de datos, algoritmos o roles que ejecutan la función.

```mermaid
flowchart TD
    subgraph CONTROLES ["Controles y Normativas (C — Arriba)"]
        C1["Normativa Fiscal / ISO 27001"]
        C2["Política RBAC + Aislamiento Multi-Tenant"]
        C3["Validación de Idempotency-Key"]
    end

    subgraph ENTRADAS ["Entradas (I — Izquierda)"]
        I1["Solicitud de Emisión de Factura (JSON)"]
        I2["Token JWT RS256 Verificado"]
    end

    subgraph FUNCION ["Función Central IDEF0 (A0)"]
        F0["A0: Emitir y Contabilizar Factura Electrónica"]
    end

    subgraph SALIDAS ["Salidas (O — Derecha)"]
        O1["Factura Emitida + XML/JSON Firmado"]
        O2["Asiento Contable Automático"]
        O3["Traza Inmutable en AuditTrail (SHA-256)"]
    end

    subgraph MECANISMOS ["Mecanismos y Recursos (M — Abajo)"]
        M1["Microservicio Facturación + Contabilidad"]
        M2["PostgreSQL 16 + Transactional Outbox"]
    end

    C1 -->|"Control"| F0
    C2 -->|"Control"| F0
    C3 -->|"Control"| F0
    I1 -->|"Entrada"| F0
    I2 -->|"Entrada"| F0
    F0 -->|"Salida"| O1
    F0 -->|"Salida"| O2
    F0 -->|"Salida"| O3
    M1 -->|"Mecanismo"| F0
    M2 -->|"Mecanismo"| F0
```

---

### 2.3. Diagrama Entidad-Relación (ERD Crow's Foot / Pata de Gallo)
Documenta tablas físicas, tipos de datos, llaves primarias (`PK`), foráneas (`FK`), cardinalidades y el cuarteto de auditoría:

```mermaid
erDiagram
    EMP_EMPRESAS ||--o{ FAC_FACTURAS : "posee (1:N)"
    FAC_FACTURAS ||--|{ FDT_DETALLES : "contiene (1:N)"

    EMP_EMPRESAS {
        int emp_id PK
        varchar emp_ruc UK
        varchar emp_razon_social
        boolean emp_activo
        timestamptz emp_fecha_sis
    }

    FAC_FACTURAS {
        bigint fac_id PK
        int fac_id_emp FK "Índice Multi-Tenant"
        varchar fac_numero UK
        numeric fac_total
        varchar fac_estado
        timestamptz fac_fecha_sis
        int fac_creador
        jsonb fac_config
        jsonb fac_data_old
    }

    FDT_DETALLES {
        bigint fdt_id PK
        bigint fdt_id_fac FK
        int fdt_id_emp FK
        numeric fdt_cantidad
        numeric fdt_precio_unitario
    }
```

---

### 2.4. Otros Diagramas Exigibles según el Contexto
- **Diagrama de Secuencia UML (`sequenceDiagram`):** Para flujos de autenticación OAuth2/JWKS, pagos idempotentes y comunicación síncrona/asíncrona entre microservicios.
- **Máquina de Estados (`stateDiagram-v2`):** Para modelar el ciclo de vida exacto de entidades transaccionales (ej. `Borrador → Emitida → PagadaParcial → PagadaTotal | Anulada`).
- **Diagrama de Flujo de Datos (DFD Nivel 1 y 2) & ITIL v4:** Para mapear el tránsito de datos sensibles (PII) y la gestión de incidentes/cambios/SLAs.

---

## ⛔ 3. Checklist Obligatorio de Cierre en Diagramación (Quality Gate)

- [ ] ¿Los nombres de servicios, tablas y puertos en el diagrama coinciden exactamente con el código fuente real?
- [ ] ¿Todas las flechas especifican qué dato o protocolo viaja a través de ellas?
- [ ] ¿Las etiquetas de nodos con paréntesis o corchetes están debidamente entrecomilladas (`["..."]`) para evitar errores de sintaxis en Mermaid?
- [ ] ¿Se incluyó el aislamiento multi-tenant y la capa de auditoría en los diagramas de arquitectura y datos?

