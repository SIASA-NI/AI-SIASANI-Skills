---
name: tdd-red-green-refactor
description: Disciplina estricta de Desarrollo Guiado por Pruebas (TDD Red-Green-Refactor), diseño orientado a contratos verificables, pruebas basadas en propiedades (Property-Based Testing) y prohibición de código productivo sin prueba fallida previa.
---

# 🧪 Test-Driven Development (TDD: Red-Green-Refactor) Lead

## 1. Propósito y Mandato Inviolable
Este skill establece la disciplina de **Desarrollo Guiado por Pruebas (TDD)** de nivel Staff Engineer (inspirado en Kent Beck y `obra/superpowers/test-driven-development`).

> **REGLA DE ORO DEL TDD:** *No se escribe una sola línea de código de producción a menos que sea para hacer pasar un test automatizado que actualmente falla.*

---

## 2. El Ciclo Canónico `RED ➔ GREEN ➔ REFACTOR`

### 🔴 Etapa 1: RED (Escribir un Test que Falle por la Razón Correcta)
1. Definir el comportamiento esperado de una unidad de negocio o endpoint antes de escribir su implementación.
2. Escribir un test pequeño, expresivo y determinista siguiendo el patrón **Arrange – Act – Assert (AAA)** o **Given – When – Then**.
3. **Ejecutar el test y observar el fallo real:**
   - Certificar que el test falla porque la funcionalidad aún no existe o lanza `NotImplementedException`, no por un error de sintaxis del propio test.
   - Si el test pasa de inmediato sin haber tocado el código productivo, el test es inválido (falso positivo) y debe reescribirse.

### 🟢 Etapa 2: GREEN (Hacer Pasar el Test con la Implementación Más Simple)
1. Escribir únicamente el código de producción mínimo y limpio necesario para satisfacer la aserción del test.
2. Prohibido adelantar optimizaciones prematuras o funcionalidades no cubiertas por el test actual (*YAGNI — You Aren't Gonna Need It*).
3. Ejecutar nuevamente el runner (`dotnet test`, `vitest`, `pytest`, `cargo test`) y verificar con evidencia de consola que el test cambia a **PASS (Green)**.

### 🔵 Etapa 3: REFACTOR (Purificar el Diseño Manteniendo los Tests en Verde)
1. Con la red de seguridad en verde, eliminar duplicación, extraer objetos de valor (*Value Objects*), mejorar nombres semánticos y reducir la complejidad ciclomática (`< 10`).
2. Ejecutar los tests tras cada micro-refactorización para garantizar que el comportamiento permanece 100% intacto.

---

## 3. Pirámide de Pruebas de Alta Fidelidad y Cero Mocks Mentirosos
1. **Pruebas Unitarias de Dominio (70%):** Rápidas (`< 10 ms`), sin I/O ni base de datos, probando cálculos financieros, máquinas de estados, reglas de negocio e invariantes con valores límite (*Boundary Value Analysis*).
2. **Pruebas Basadas en Propiedades (*Property-Based Testing — 10%*):** Uso de `FsCheck`, `Hypothesis` o `fast-check` para generar cientos de entradas aleatorias (strings Unicode extremos, montos negativos, fechas bisiestas) y verificar invariantes matemáticos (ej. `Suma(Partidas) === TotalFactura`).
3. **Pruebas de Integración Reales (15%):** Contra contenedores reales efímeros (*Testcontainers* con PostgreSQL/Redis) verificando migraciones, constraints `UNIQUE/FK`, filtros multi-tenant (`EmpId`) y serialización JSON.
4. **Pruebas E2E Críticas (5%):** Flujos completos de usuario en navegador real (`Playwright`).

---

## 4. Anti-Patrones de Testing Estrictamente Prohibidos
- ❌ **Testing Mock Behavior:** Configurar un mock para que devuelva `true` y luego hacer `Assert.True(mock.Method())`. Eso prueba la librería de mocking, no tu sistema.
- ❌ **Tests Frágiles Acoplados a Detalles Internos:** Verificar llamadas a métodos privados en lugar de verificar el resultado observable o el cambio de estado final.
- ❌ **Assertions Vacías o Permisivas:** Usar `expect(result).toBeDefined()` en lugar de verificar los campos, tipos y valores exactos del objeto retornado.
- ❌ **Tests Dependientes del Orden o del Reloj del Sistema:** Todo test debe inyectar un `TimeProvider` / `IClock` determinista y limpiar su propio estado transaccional.

