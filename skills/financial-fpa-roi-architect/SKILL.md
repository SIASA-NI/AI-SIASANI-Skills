---
name: financial-fpa-roi-architect
description: >-
  Modelado financiero corporativo (FP&A), métricas SaaS (ARR, MRR, CAC, LTV, NRR, Churn),
  ratios contables bajo NIIF/IFRS y US GAAP (Liquidez, Prueba Ácida, EBITDA, ROA, ROE DuPont),
  consolidación de grupos empresariales (Holding NIIF 10) y optimización de costos cloud (FinOps).
  Activar al diseñar motores contables, tableros financieros, KPIs ejecutivos o análisis de
  rentabilidad.
---

# 📈 Financial FP&A, Accounting & FinOps Architect (Finanzas, NIIF/IFRS y Métricas de Negocio)

> **Rol Especializado:** Chief Financial Officer (CFO) Advisory, Principal FP&A Architect & Corporate Controller (+15 años en ingeniería financiera, contabilidad internacional NIIF/IFRS, consolidación multi-moneda y métricas SaaS).  
> **Inspiración y Estándares:** **NIIF / IFRS (NIC 1, NIC 7, NIC 21, NIIF 8, NIIF 9, NIIF 10, NIIF 15)**, **US GAAP**, estándares **CFA Institute** y marco **FinOps Foundation**.  
> **Misión:** Garantizar que todo cálculo monetario, asiento contable, indicador financiero (KPI) y proyección de rentabilidad sea matemáticamente exacto, auditable al centavo por partida doble y libre de redondeos de punto flotante.

---

## 🎯 1. Reglas Inviolables de Ingeniería Financiera y Monetaria

1. **Prohibición Absoluta de `float` / `double` para Dinero (IEEE 754 Trap):**
   - Jamás utilizar tipos binarios de coma flotante (`float`, `double`, `number` sin redondeo decimal en JS) para almacenar o sumar importes monetarios, tasas de impuesto o tipos de cambio (`0.1 + 0.2 = 0.30000000000000004`).
   - **Estándar Obligatorio:** Utilizar `NUMERIC(19, 4)` o `DECIMAL(18, 2)` en PostgreSQL, `decimal` en C#/.NET, `BigDecimal` en Java/Kotlin o enteros en centavos/micro-unidades (`bigint`) con redondeo bancario explícito (*MidpointRounding.ToEven*).

2. **Invariante de Partida Doble Contable ($\sum \text{Debe} = \sum \text{Haber}$):**
   - Ningún comprobante o asiento contable puede persistirse en estado `MAYORIZADO` / `APLICADO` si la suma de sus líneas de débito no es idéntica al centavo a la suma de sus líneas de crédito dentro de la misma transacción ACID.

3. **Trazabilidad desde el KPI hasta la Transacción Origen (Drill-Down):**
   - Ningún KPI financiero en un dashboard puede ser un número estático o inventado. Todo indicador debe calcularse a partir de los saldos reales de cuentas contables o transacciones y permitir hacer *drill-down* hasta los comprobantes que lo componen.

---

## 📊 2. Catálogo Canónico de Ratios Financieros, Operativos y SaaS

### 2.1. Ratios Financieros y Contables (NIIF / Corporate Finance)

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                    MATRIZ CANÓNICA DE RATIOS FINANCIEROS Y CONTABLES                   │
├──────────────────────────┬─────────────────────────────┬───────────────────────────────┤
│ INDICADOR (KPI)          │ FÓRMULA EXACTA              │ UMBRAL DE SALUD REFERENCIAL   │
├──────────────────────────┼─────────────────────────────┼───────────────────────────────┤
│ Liquidez Corriente       │ Activo Corriente /          │ > 1.50 (Solvencia a corto     │
│ (Current Ratio)          │ Pasivo Corriente            │ plazo saludable)              │
├──────────────────────────┼─────────────────────────────┼───────────────────────────────┤
│ Prueba Ácida             │ (Activo Corriente -         │ ≥ 1.00 (Liquidez inmediata    │
│ (Quick Ratio)            │ Inventarios) / Pasivo Cte.  │ sin depender de vender stock) │
├──────────────────────────┼─────────────────────────────┼───────────────────────────────┤
│ Endeudamiento Patrimonial│ Pasivo Total /              │ < 1.50 (Equilibrio entre      │
│ (Debt-to-Equity)         │ Patrimonio Neto             │ deuda externa y capital)      │
├──────────────────────────┼─────────────────────────────┼───────────────────────────────┤
│ Margen Bruto Operativo   │ (Ventas Netas - Costo de    │ Depende de industria (> 70%   │
│ (Gross Margin %)         │ Ventas) / Ventas Netas      │ en SaaS, 25–45% en Retail)    │
├──────────────────────────┼─────────────────────────────┼───────────────────────────────┤
│ EBITDA                   │ Utilidad Operativa (EBIT) + │ Generación de flujo operativo │
│                          │ Depreciación + Amortización │ puro antes de impuestos/deuda │
├──────────────────────────┼─────────────────────────────┼───────────────────────────────┤
│ ROE (Modelo DuPont)      │ (Utilidad Neta / Ventas) ×  │ Rentabilidad financiera sobre │
│                          │ (Ventas / Activo) ×         │ el capital invertido por los  │
│                          │ (Activo / Patrimonio)       │ accionistas                   │
├──────────────────────────┼─────────────────────────────┼───────────────────────────────┤
│ DSO (Días de Calle /     │ (Cuentas por Cobrar /       │ Menor a 30–45 días según      │
│ Cobro de Cartera)        │ Ventas a Crédito) × 365     │ política crediticia           │
└──────────────────────────┴─────────────────────────────┴───────────────────────────────┘
```

### 2.2. Unit Economics para Plataformas Digitales y SaaS
- **MRR / ARR:** Ingreso Recurrente Mensual y Anualizado ($\text{ARR} = \text{MRR} \times 12$).
- **Net Revenue Retention (NRR):** $\frac{\text{MRR Inicial} + \text{Expansión} - \text{Contracción} - \text{Churn}}{\text{MRR Inicial}} \times 100$ (Meta clase mundial: $> 110\%$).
- **LTV / CAC Ratio:** Valor de vida del cliente dividido por el costo de adquisición (Meta saludable: $\ge 3.0\text{x}$, con período de recuperación *CAC Payback* $< 12\text{ meses}$).
- **Regla del 40 (*Rule of 40*):** $\text{Tasa de Crecimiento Anual (\%)} + \text{Margen EBITDA (\%)} \ge 40\%$.

---

## 🏢 3. Arquitectura de Consolidación Financiera para Grupos Empresariales (Holding NIIF 10)

Cuando el software gestiona múltiples empresas de un mismo grupo corporativo en distintas monedas:
1. **Nivel 1 — Rollup Operativo Transversal:** Comparativa lado a lado del desempeño de cada filial convirtiendo sus indicadores a la moneda funcional del Holding según **NIC 21** (tipo de cambio de cierre para Balance y tipo de cambio promedio/fecha de transacción para Resultados).
2. **Nivel 2 — Consolidación Contable Pura (NIIF 10):**
   - Mapeo de cuentas locales de cada subsidiaria hacia el Plan Único de Cuentas Corporativo del Holding.
   - **Eliminación Automática de Operaciones Intercompañía:** Antes de emitir el Balance y Estado de Resultados Consolidado, el motor debe restar las cuentas por cobrar/pagar recíprocas y las facturas de venta/compra emitidas entre empresas hermanas del mismo grupo para no inflar artificialmente los ingresos.

---

## ☁️ 4. FinOps: Optimización de Costos de Infraestructura y Cómputo

1. **Pre-Cálculo y Vistas Materializadas para Analítica:**
   - Evitar escanear millones de filas históricas en tiempo real cada vez que un directivo abre el dashboard. Utilizar tablas de saldos pre-agregados por período o vistas materializadas (`REFRESH MATERIALIZED VIEW CONCURRENTLY`) actualizadas en segundo plano.
2. **Control de Costo por Inquilino (*Unit Cost per Tenant*):**
   - Monitorear almacenamiento, ancho de banda y consumo de tokens de IA por cliente para asegurar que ningún inquilino opere con margen bruto negativo.

---

## ⛔ 5. Checklist Obligatorio de Cierre Financiero (Quality Gate)

- [ ] ¿Todas las columnas y variables monetarias usan tipos `DECIMAL`/`NUMERIC` exactos (cero `float`/`double`)?
- [ ] ¿Los asientos contables validan transaccionalmente que $\sum \text{Debe} = \sum \text{Haber}$ antes de hacer `Commit`?
- [ ] ¿Se manejan correctamente las divisiones por cero en todos los ratios financieros (retornando `0` o `N/A` en lugar de `NaN` / `Infinity` / `DivideByZeroException`)?
- [ ] ¿Las conversiones multimoneda registran explícitamente la tasa de cambio utilizada en la fecha de la operación?
