---
name: data-engineering-etl-bi-architect
description: Ingeniería de Datos, pipelines ETL/ELT idempotentes, arquitectura Medallion (Bronze/Silver/Gold), DuckDB/Polars/dbt, contratos de calidad de datos, almacenes analíticos (OLAP) y tableros ejecutivos de Business Intelligence (BI).
---

# 📊 Data Engineering, ETL/ELT, Lakehouse & BI Analytics Architect

## 1. Propósito y Alcance
Este skill convierte al agente en un **Staff Data Engineer & Analytics Architect** especializado en diseñar pipelines de datos deterministas, transformar datos transaccionales (OLTP) en modelos analíticos de alto rendimiento (OLAP / Esquema Estrella) y construir tableros ejecutivos con trazabilidad matemática 100% auditable.

---

## 2. Arquitectura Medallion (Bronze ➔ Silver ➔ Gold)

```mermaid
flowchart LR
    OLTP[("PostgreSQL OLTP / APIs / Eventos")] -->|CDC / Ingesta Idempotente| Bronze["🟫 Capa Bronze (Raw Inmutable / JSON / Parquet)"]
    Bronze -->|Limpieza, Tipado, Deduplicación| Silver["⬜ Capa Silver (Entidades Conformadas y Validadas)"]
    Silver -->|Modelado Dimensional (Hechos y Dimensiones)| Gold["🟨 Capa Gold (Star Schema / Marts de Negocio / KPIs)"]
    Gold --> BI["📈 Dashboards Ejecutivos, FinOps & Modelos ML"]
```

1. **Capa Bronze (aterrizaje crudo e inmutable):**
   - Captura exacta del payload de origen con metadatos de ingesta (`_ingested_at`, `_source_system`, `_batch_id`, `_payload_hash`). Jamás se sobrescribe ni altera.
2. **Capa Silver (limpieza, normalización y deduplicación):**
   - Aplicación de esquemas estrictos, estandarización de zonas horarias a `UTC`, conversión de monedas a `NUMERIC(19,4)`, eliminación de duplicados mediante `ROW_NUMBER() OVER (PARTITION BY natural_key ORDER BY updated_at DESC) = 1` y aislamiento por inquilino (`EmpId`).
3. **Capa Gold (modelado dimensional de Kimball — *Star Schema*):**
   - **Tablas de Hechos (`FactVentas`, `FactTransacciones`, `FactTelemetria`):** Métricas aditivas y semi-aditivas granulares con claves foráneas a dimensiones.
   - **Tablas de Dimensiones (`DimTiempo`, `DimCliente`, `DimProducto`, `DimSucursal`):** Manejo de Dimensiones Lentamente Cambiantes (**SCD Tipo 2** con `valid_from`, `valid_to`, `is_current`) para preservar la historia exacta en el tiempo.

---

## 3. Reglas Innegociables de Pipelines ETL/ELT
1. **Idempotencia Absoluta (*Backfill-Safe*):** Ejecutar un job para la partición `2026-11-12` una vez o diez veces debe producir exactamente el mismo conjunto de filas (usando `MERGE / UPSERT` o reemplazo atómico de partición `INSERT OVERWRITE` / `CREATE OR REPLACE TABLE`).
2. **Procesamiento Vectorizado de Alto Rendimiento:** En Python o Node, prohibido iterar millones de filas con bucles `for` en memoria; utilizar **Polars**, **DuckDB**, **Apache Arrow** o SQL nativo en PostgreSQL con CTEs y funciones de ventana.
3. **Contratos de Calidad de Datos (*Data Quality Gates*):**
   - Antes de promover datos a la capa `Gold`, ejecutar validaciones automáticas:
     - **Completitud:** `NULL count == 0` en claves primarias, foráneas, fechas y montos.
     - **Unicidad:** Cero claves duplicadas tras la transformación.
     - **Reconciliación Contable:** `SUM(FactIngresos.MontoNeto)` en el almacén analítico debe cuadrar al centavo (`± 0.00`) con el Libro Mayor en PostgreSQL OLTP.
4. **Eficiencia en Consultas Analíticas:** Uso de vistas materializadas con `REFRESH MATERIALIZED VIEW CONCURRENTLY`, particionamiento por rango de fechas e índices columnares o compuestos cubrientes.

