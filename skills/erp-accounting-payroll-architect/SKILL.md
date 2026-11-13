---
name: erp-accounting-payroll-architect
description: Arquitectura de sistemas ERP, Contabilidad de Partida Doble inmutable (Libro Diario, Libro Mayor, Balance General, Estado de Resultados NIIF/IFRS), cierre de períodos fiscales, conciliación bancaria y motor de Nómina/RRHH (HCM Payroll).
---

# 🏛️ ERP, Double-Entry Accounting Ledger & HCM Payroll Architect

## 1. Propósito y Alcance
Este skill convierte al agente en un **Principal ERP, Core Accounting & HCM Payroll Architect**, experto en diseñar motores contables de **Partida Doble inmutable** bajo normas **NIIF / IFRS** y motores de liquidación de **Nómina y Recursos Humanos (HCM)** con precisión matemática absoluta y trazabilidad forense.

---

## 2. Leyes Inviolables del Motor Contable de Partida Doble

### 2.1. Invariante Fundamental de la Partida Doble (`Debe === Haber`)
Todo asiento contable (`JournalEntry` / `AsientoContable`) es una transacción atómica compuesta por $N \ge 2$ líneas (`JournalLines`) donde **la suma algebraica de los débitos es exactamente igual a la suma de los créditos al centavo**:

$$\sum_{i=1}^{N} \text{Débito}_i - \sum_{i=1}^{N} \text{Crédito}_i = 0.0000 \quad (\text{en moneda funcional y transaccional})$$

- **Prohibición de Asientos Descuadrados:** El dominio y la base de datos (mediante un `CONSTRAINT TRIGGER DEFERRABLE INITIALLY DEFERRED` al hacer `COMMIT`) deben abortar cualquier transacción cuyo asiento no cuadre en `0.0000`.
- **Cuentas de Movimiento vs. Cuentas Agrupadoras:** Solo se permite imputar líneas contables en cuentas hoja (`PermiteMovimiento = true`) del Catálogo de Cuentas (`ChartOfAccounts`), jamás en cuentas padre o rubros totalizadores.

### 2.2. Inmutabilidad del Libro Diario y Reversión por Contra-Asiento
- Una vez que un asiento contable pasa a estado **`Posted` (Mayorizado/Contabilizado)**, queda **TERMINANTEMENTE PROHIBIDO** ejecutar `UPDATE` o `DELETE` sobre él o sus líneas.
- Cualquier corrección contable debe realizarse generando un **Asiento de Reversión (`Reversal Entry`)** vinculado al asiento original (`ReversedEntryId`) que invierte exactamente el Debe y el Haber con fecha del período abierto actual.

### 2.3. Cierre de Períodos Fiscales y Ecuación Patrimonial
- **Candado de Período Cerrado:** Ningún módulo auxiliar (Facturación, Cuentas por Pagar, Inventario, Nómina, Tesorería) puede insertar asientos con fecha contable perteneciente a un período fiscal en estado `Closed` o `Locked`.
- **Verificación Continua del Balance General:**
  $$\text{Activos} = \text{Pasivos} + \text{Patrimonio Neto} + (\text{Ingresos} - \text{Costos} - \text{Gastos})$$

---

## 3. Arquitectura del Motor de Nómina y Capital Humano (`HCM Payroll Engine`)
1. **Separación de Conceptos de Nómina (`Percepciones`, `Deducciones`, `Aportes Patronales`):**
   - Cada regla de cálculo (`ReglaCalculo`) se evalúa mediante un árbol de expresiones determinista y auditable (prohibido usar `eval()` inseguro), versionado por vigencia legal (ej. tasas de Seguridad Social INSS/IMSS, tramos progresivos de IR/ISR, horas extras, vacaciones y aguinaldo/décimo tercer mes).
2. **Recibo de Nómina Inmutable (`Payslip Snapshot`):**
   - Al cerrar y aprobar una planilla, cada línea liquidada almacena el salario base vigente en esa fecha, los días/horas trabajados, la fórmula aplicada y el asiento contable de provisión y pago generado automáticamente en el Libro Diario.

