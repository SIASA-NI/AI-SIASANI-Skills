---
name: customer-success-support-automation
description: Arquitectura de Service Desk ITIL v4, automatización de soporte omnicanal Tier-1 con IA (RAG sobre manuales verificados), gestión de SLAs (MTTR, FRT), matrices de escalamiento, salud del cliente (Customer Health Score) y prevención de Churn.
---

# 🎧 Customer Success, Service Desk ITIL v4 & AI Support Automation

## 1. Propósito y Alcance
Este skill transforma al agente en un **Arquitecto de Service Desk (ITIL v4), Soporte Omnicanal y Customer Success B2B/B2C**, diseñando sistemas donde los usuarios reciben respuestas precisas en segundos, los incidentes críticos se escalan con trazabilidad total y el riesgo de abandono (*Churn*) se detecta proactivamente.

---

## 2. Arquitectura de Service Desk y Clasificación ITIL v4

Todo ticket o conversación entrante debe clasificarse y enrutarse automáticamente en una de las 4 prácticas ITIL v4:
1. **Incident Management (Incidente):** Interrupción no planificada o degradación de un servicio productivo. Prioridad determinada por la matriz **`Impacto x Urgencia`** (`P1 Crítico`, `P2 Alto`, `P3 Medio`, `P4 Bajo`).
2. **Service Request (Solicitud de Servicio):** Petición estándar de usuario (ej. alta de permiso, exportación de reporte, duda funcional sobre un módulo).
3. **Problem Management (Problema):** Causa raíz subyacente de uno o más incidentes recurrentes (vinculado a un *Known Error Database — KEDB*).
4. **Change Enablement (Cambio):** Solicitud controlada de modificación o despliegue con evaluación de riesgo y plan de rollback.

---

## 3. Relojes de SLA yMatriz de Escalamiento Automático

| Prioridad | Definición Operativa | SLA Primera Respuesta (FRT) | SLA Resolución (MTTR) | Escalamiento Automático |
| :-: | :--- | :-: | :-: | :--- |
| **P1 (Crítico)** | Caída total o bloqueo de facturación/pagos | `≤ 5 min` | `≤ 1 hora` | Alerta inmediata a SRE On-Call + Canal P1 |
| **P2 (Alto)** | Módulo clave degradado sin workaround simple | `≤ 15 min` | `≤ 4 horas` | Escalamiento a Ingeniería Nivel 3 |
| **P3 (Medio)** | Incidencia parcial con workaround disponible | `≤ 1 hora` | `≤ 24 horas` | Cola de Soporte Técnico Nivel 2 |
| **P4 (Bajo)** | Consulta general, cosmética o solicitud de mejora | `≤ 4 horas` | `≤ 72 horas` | Resolución asistida por Agente IA Tier-1 |

- **Pausa de Reloj Auditada (*SLA Clock Pause*):** El temporizador de SLA solo se pausa cuando el estado cambia a `WaitingForCustomer` y se reanuda instantáneamente en cuanto el cliente responde.

---

## 4. Agente de Soporte IA Tier-1 Basado en Evidencia (*Zero-Hallucination Helpdesk*)
1. **Anclaje Exclusivo en Documentación Oficial (RAG):** El asistente de soporte responde consultando los manuales de usuario verificados, procesos `SRV-*` y el estado de salud real de la plataforma (`/status`).
2. **Prohibición de Promesas Falsas:** El bot jamás inventa funcionalidades inexistentes, ni promete reembolsos o cambios de base de datos no autorizados.
3. **Handoff Humano con Contexto Completo:** Cuando la confianza semántica es `< 0.78`, el usuario expresa frustración o se detecta un incidente `P1/P2`, el sistema transfiere la sesión a un especialista humano adjuntando un resumen ejecutivo de 3 líneas, el `TenantId`, logs recientes de error del usuario y los pasos ya intentados.

---

## 5. Salud del Cliente (*Customer Health Score*) y Prevención de Churn
Calcular diariamente un **Health Score (0–100)** por cuenta/inquilino ponderando:
- **Frecuencia de Uso del Core (40%):** DAU/MAU y tendencia de sesiones en los últimos 14 días.
- **Adopción de Módulos Clave (25%):** Profundidad de uso de las funciones de mayor retención.
- **Salud de Soporte (20%):** Tickets abiertos `P1/P2` y calificación `CSAT`.
- **Salud de Facturación (15%):** Pagos al día vs. reintentos de cobro fallidos (*Dunning*).
> Cuando el *Health Score* cae por debajo de `50` o disminuye `> 20 puntos` en una semana, disparar automáticamente un flujo de retención preventiva para el equipo de Customer Success.

