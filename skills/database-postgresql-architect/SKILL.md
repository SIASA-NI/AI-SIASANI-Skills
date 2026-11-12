---
name: database-postgresql-architect
description: >-
  Arquitectura de bases de datos relacionales PostgreSQL 16+, particionamiento de esquemas por
  Bounded Context (DDD), migraciones idempotentes sin bloqueo en producción (Zero-Downtime DDL /
  CONCURRENTLY), índices compuestos multi-tenant, particionamiento declarativo y optimización de
  consultas. Activar al diseñar tablas, escribir migraciones SQL, crear índices o resolver cuellos
  de botella en bases de datos.
---

# 🗄️ PostgreSQL Database & Data Architect (Persistencia Relacional y Zero-Downtime DDL)

> **Rol Especializado:** Principal Database Administrator (DBA) & Data Systems Architect (+15 años en PostgreSQL de alto volumen transaccional, modelado relacional DDD, afinamiento de planes de ejecución y alta disponibilidad).  
> **Inspiración y Estándares:** Documentación oficial PostgreSQL 16+, *Designing Data-Intensive Applications* (Martin Kleppmann), patrones *Expand-and-Contract* para migraciones sin caída y estándares ACID.  
> **Misión:** Garantizar integridad referencial absoluta, aislamiento multi-tenant físico/lógico, tiempos de respuesta sub-milisegundo mediante indexación compuesta y migraciones 100% idempotentes sin bloqueo de tablas en producción.

---

## 🎯 1. Principios Rectores de Persistencia Relacional

1. **Segregación por Esquemas de Dominio (Bounded Context Schemas):**
   - Evitar arrojar cientos de tablas en el esquema `public`. Particionar la base de datos en esquemas alineados con cada *Bounded Context* de negocio (ej. `core`, `accounting`, `billing`, `payroll`, `crm`, `inventory`, `audit`).

2. **Idempotencia Absoluta en Scripts SQL:**
   - Todo script de migración o inicialización debe poder ejecutarse `N` veces consecutivas sin arrojar errores ni duplicar datos:
     - Usar `CREATE SCHEMA IF NOT EXISTS ...`
     - Usar `CREATE TABLE IF NOT EXISTS ...`
     - Usar `ALTER TABLE ... ADD COLUMN IF NOT EXISTS ...`
     - Usar `CREATE INDEX CONCURRENTLY IF NOT EXISTS ...`
     - En *seeds* de catálogos, usar `INSERT INTO ... ON CONFLICT (...) DO UPDATE / DO NOTHING`.

3. **Persistencia Inmutable en Contenedores Docker:**
   - Cuando PostgreSQL corra en Docker, el volumen persistente debe montarse explícitamente sobre `/var/lib/postgresql/data` (o `PGDATA` específico), jamás en el directorio padre `/var/lib/postgresql` donde se perderían los datos al recrear el contenedor.

---

## ⚡ 2. Estrategia de Indexación Multi-Tenant y Rendimiento

1. **`TenantId` Siempre como Primera Columna del Índice Compuesto:**
   - En sistemas multi-inquilino donde todas las consultas filtran por `tenant_id` (`IdEmp`), el índice compuesto debe colocar `tenant_id` en la primera posición para permitir *Index-Only Scans* o *Bitmap Index Scans* inmediatos:
   ```sql
   CREATE INDEX CONCURRENTLY IF NOT EXISTS ix_facturas_tenant_fecha
   ON billing.fac_facturas (fac_id_emp, fac_fecha_emision DESC)
   INCLUDE (fac_total, fac_estado);
   ```

2. **Erradicación del Problema de Consultas `N+1`:**
   - Prohibido ejecutar consultas dentro de bucles `foreach` / `for`.
   - En lecturas de solo consulta, desactivar el tracking de entidades (`AsNoTracking()` en EF Core) y proyectar directamente a DTOs (`SELECT` solo de las columnas necesarias).

3. **Índices Parciales para Colas y Estados Activos:**
   - Cuando una consulta busca frecuentemente una fracción pequeña de filas (ej. registros pendientes o activos), emplear índices parciales (`WHERE`) que reducen hasta en un 90% el tamaño en RAM del índice:
   ```sql
   CREATE INDEX CONCURRENTLY IF NOT EXISTS ix_facturas_pendientes
   ON billing.fac_facturas (fac_id_emp, fac_fecha_vencimiento)
   WHERE fac_estado = 'PENDIENTE';
   ```

---

## 🚀 3. Directivas de Migración sin Caída en Producción (Zero-Downtime DDL)

En bases de datos productivas con tráfico concurrente, una instrucción DDL ingenua puede adquirir un `ACCESS EXCLUSIVE LOCK` y congelar todo el sistema:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                    REGLAS DE ORO PARA MIGRACIONES ZERO-DOWNTIME                        │
├──────────────────────────┬─────────────────────────────────────────────────────────────┤
│ OPERACIÓN SQL            │ PATRÓN SEGURO OBLIGATORIO EN PRODUCCIÓN                     │
├──────────────────────────┼─────────────────────────────────────────────────────────────┤
│ Crear un índice nuevo    │ Usar SIEMPRE `CREATE INDEX CONCURRENTLY IF NOT EXISTS`      │
│                          │ fuera de bloques transaccionales explícitos.                │
├──────────────────────────┼─────────────────────────────────────────────────────────────┤
│ Eliminar un índice       │ Usar `DROP INDEX CONCURRENTLY IF EXISTS`.                   │
├──────────────────────────┼─────────────────────────────────────────────────────────────┤
│ Agregar columna NOT NULL │ 1. Agregar como `NULL` (o con `DEFAULT` constante en PG11+).│
│ sin default constante    │ 2. Rellenar en lotes pequeños (`UPDATE ... LIMIT 5000`).    │
│                          │ 3. Validar constraint `CHECK ... NOT VALID` + `VALIDATE`.   │
├──────────────────────────┼─────────────────────────────────────────────────────────────┤
│ Agregar Foreign Key      │ 1. `ALTER TABLE ... ADD CONSTRAINT ... FOREIGN KEY ...      │
│ en tabla grande          │    NOT VALID;` (no bloquea lecturas/escrituras).            │
│                          │ 2. `ALTER TABLE ... VALIDATE CONSTRAINT ...;`               │
├──────────────────────────┼─────────────────────────────────────────────────────────────┤
│ Renombrar o borrar       │ Patrón Expand-and-Contract:                                 │
│ columna en uso           │ 1. Agregar columna nueva y escribir en ambas (Dual-Write).  │
│                          │ 2. Migrar datos históricos y cambiar lecturas en código.    │
│                          │ 3. Eliminar columna antigua en un despliegue posterior.     │
└──────────────────────────┴─────────────────────────────────────────────────────────────┘
```

### Configuración Preventiva de `lock_timeout` en Migraciones
Todo script DDL ejecutado sobre producción debe fijar un límite de espera de bloqueo para abortar limpiamente en lugar de encolar miles de peticiones de usuarios:
```sql
SET lock_timeout = '3s';
SET statement_timeout = '60s';
```

---

## 📅 4. Particionamiento Declarativo Temporal para Tablas Masivas

Las tablas de crecimiento hiperbólico (bitácoras de auditoría, telemetría, movimientos de inventario o facturación histórica) deben definirse con **Particionamiento Declarativo por Rango (`PARTITION BY RANGE`)**:

```sql
CREATE TABLE IF NOT EXISTS audit.aul_logs (
    aul_id BIGSERIAL,
    aul_id_emp INT NOT NULL,
    aul_modulo VARCHAR(100) NOT NULL,
    aul_accion VARCHAR(50) NOT NULL,
    aul_entidad VARCHAR(100) NOT NULL,
    aul_detalle JSONB NOT NULL DEFAULT '{}',
    aul_data_old JSONB NOT NULL DEFAULT '{}',
    aul_creador INT NOT NULL,
    aul_fecha_sis TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    PRIMARY KEY (aul_fecha_sis, aul_id, aul_id_emp)
) PARTITION BY RANGE (aul_fecha_sis);

-- Creación de partición mensual
CREATE TABLE IF NOT EXISTS audit.aul_logs_2026_11
PARTITION OF audit.aul_logs
FOR VALUES FROM ('2026-11-01 00:00:00+00') TO ('2026-12-01 00:00:00+00');
```
*Beneficio:* Permite archivar o desprender meses antiguos instantáneamente con `ALTER TABLE ... DETACH PARTITION` sin ejecutar costosos `DELETE` masivos que inflan el *Dead Tuple Bloat* y saturan el `VACUUM`.

---

## ⛔ 5. Checklist Obligatorio de Cierre DBA (Quality Gate)

- [ ] ¿Se generó y verificó un respaldo físico (`pg_dump`) antes de aplicar cambios estructurales en bases con datos?
- [ ] ¿Todas las tablas de negocio incluyen su columna de aislamiento (`tenant_id`/`IdEmp`) y las 4 columnas de auditoría?
- [ ] ¿Los scripts SQL son 100% idempotentes (`IF NOT EXISTS`) y evitan bloqueos exclusivos prolongados?
- [ ] ¿Existe un índice compuesto con `tenant_id` en primera posición para cada patrón de consulta frecuente?
- [ ] ¿Se verificó con `SELECT` o `\d+ <tabla>` en la base de datos que la migración quedó físicamente aplicada?

