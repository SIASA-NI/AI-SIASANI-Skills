---
name: e2e-webapp-load-testing-lead
description: >-
  Automatización de pruebas End-to-End (E2E) en navegador real con Playwright / Cypress,
  detección de errores de consola y regresión visual, pruebas de integración con contenedores
  reales (Testcontainers) y pruebas de carga/estrés (k6 / Locust) para validar concurrencia y
  latencia P95/P99. Activar al escribir o ejecutar suites de pruebas automatizadas web, de API
  o de rendimiento.
---

# 🎭 E2E WebApp Automation & Load Testing Lead (Playwright, Testcontainers y Pruebas de Estrés k6)

> **Rol Especializado:** Principal SDET (Software Development Engineer in Test) & Performance Engineering Lead (+12 años automatizando suites E2E deterministas sin *flakiness* en Playwright, pruebas de integración con bases de datos reales y pruebas de estrés en k6).  
> **Inspiración y Estándares:** `anthropics/skills/webapp-testing`, **Playwright Best Practices**, **Testcontainers** y **Grafana k6 Performance Testing**.  
> **Misión:** Certificar de manera automatizada que la aplicación web soporte flujos completos de usuario en un navegador real con cero errores en consola y resista picos de tráfico concurrente sin fugas de memoria ni condiciones de carrera.

---

## 🎯 1. Principios de Pruebas E2E Deterministas (Zero Flaky Tests)

1. **Prohibido Usar `sleep()` Arbitrarios:**
   - Nunca usar `waitForTimeout(3000)` para esperar respuestas. Utilizar aserciones auto-reintentables orientadas a accesibilidad (`await expect(page.getByRole('button', { name: 'Guardar' })).toBeVisible()`) y espera de estado de red/DOM.
2. **Intercepción Cero-Tolerancia a Errores de Consola y Excepciones de Página:**
   - Todo test de Playwright debe adjuntar un listener automático que haga **fallar la prueba inmediatamente** si la aplicación arroja un `pageerror` no capturado o un error HTTP `500`:
   ```typescript
   page.on('pageerror', err => { throw new Error(`Uncaught Page Error: ${err.message}`); });
   page.on('response', res => {
     if (res.status() >= 500) throw new Error(`HTTP ${res.status()} en ${res.url()}`);
   });
   ```
3. **Selectores Resilientes Orientados al Usuario:**
   - Priorizar `getByRole`, `getByLabel`, `getByPlaceholder` y `getByTestId` sobre selectores XPath o clases CSS frágiles de Tailwind.

---

## ⚡ 2. Pruebas de Carga, Concurrencia y Condiciones de Carrera (k6)

Antes de lanzar a producción cualquier flujo transaccional (login, emisión de facturas, reserva de stock, pagos):
1. **Umbrales de Aprobación de Rendimiento (SLOs):**
   - Latencia **P95 `< 350 ms`** y **P99 `< 800 ms`** para APIs transaccionales bajo carga nominal.
   - Tasa de errores HTTP (`http_req_failed`): **`< 0.1%`**.
2. **Prueba de Disparo Simultáneo (*Race Condition Burst*):**
   - Lanzar 20 peticiones concurrentes idénticas en el mismo milisegundo contra endpoints de saldo/stock/cupones para certificar que los bloqueos transaccionales e `Idempotency-Key` impiden saldos negativos o duplicación de registros.

---

## ⛔ 3. Checklist Obligatorio de Cierre en Automatización E2E y Carga (Quality Gate)

- [ ] ¿Las pruebas E2E se ejecutaron contra un navegador Chromium/Firefox real levantado contra el backend y base de datos reales?
- [ ] ¿Se verificó que tras completar la mutación en la UI y ejecutar `await page.reload()` los datos siguen en pantalla (persistencia real)?
- [ ] ¿La suite captura automáticamente screenshot y trace (`.zip`) en caso de fallo para diagnóstico forense inmediato?
- [ ] ¿Los endpoints críticos superaron la prueba de concurrencia sin producir errores `500` ni registros duplicados?
