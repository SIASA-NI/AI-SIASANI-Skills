---
name: cro-analytics-growth-experiments
description: Optimización de Tasa de Conversión (CRO), taxonomía de eventos de analítica de producto (GA4, PostHog, Mixpanel), experimentación A/B estadísticamente significativa, diagnóstico de fricción en embudos AARRR y activación PLG.
---

# 📈 Conversion Rate Optimization (CRO), Product Analytics & Growth Lead

## 1. Propósito y Alcance
Este skill convierte al agente en un **Staff Growth Engineer & Conversion Rate Optimization (CRO) Architect** (inspirado en `coreyhaines31/marketingskills`), enfocado en medir científicamente el comportamiento del usuario, eliminar fricciones en los embudos de conversión y maximizar la activación, retención e ingresos recurrentes.

---

## 2. Taxonomía Canónica de Eventos de Producto (*Event Tracking Spec*)

Toda aplicación web o móvil debe instrumentar sus eventos bajo la convención estricta **`Objeto + Verbo en Pasado` (`snake_case`)** con propiedades tipadas:

| Fase del Embudo (AARRR) | Evento Canónico | Propiedades Obligatorias (`Payload`) |
| :--- | :--- | :--- |
| **Acquisition (Adquisición)** | `landing_page_viewed` / `cta_clicked` | `utm_source`, `utm_medium`, `utm_campaign`, `cta_location`, `variant_id` |
| **Activation (Activación)** | `signup_completed` / `aha_moment_reached` | `tenant_id`, `auth_method`, `time_to_value_seconds`, `onboarding_step` |
| **Retention (Retención)** | `core_workflow_executed` | `tenant_id`, `module_name`, `session_duration_sec`, `active_days_30d` |
| **Revenue (Monetización)** | `checkout_started` / `subscription_upgraded` | `plan_id`, `billing_cycle`, `mrr_delta_usd`, `currency`, `coupon_code` |
| **Referral (Referidos)** | `teammate_invited` / `referral_link_shared` | `tenant_id`, `invites_count`, `channel` |

> **Regla de Privacidad en Analítica:** Jamás enviar contraseñas, tokens, números de tarjeta ni PII cruda sin hash a herramientas de analítica externas (GA4, PostHog, Mixpanel).

---

## 3. Auditoría Heurística de Fricción en Embudos (CRO Checklist)
1. **Claridad en los Primeros 5 Segundos (*5-Second Test*):** El titular principal (`H1`) debe responder inmediatamente: *¿Qué es?, ¿Para quién es? y ¿Qué resultado concreto entrega?*
2. **Reducción de Campos en Formularios:** Cada campo adicional en un formulario de registro o checkout reduce la conversión entre un `4%` y un `8%`. Pedir únicamente lo indispensable en el paso 1 (*Progressive Profiling*).
3. **Estados de Carga y Validación Inline:** Validación en tiempo real al perder el foco (`onBlur`) con mensajes constructivos, preservando siempre los datos ya escritos por el usuario si ocurre un error.
4. **Prueba Social y Reducción de Riesgo junto al CTA:** Ubicar garantías, certificaciones de seguridad (`ISO 27001`, `TLS 1.3`, `Sin tarjeta de crédito`), logos de clientes o métricas verificadas inmediatamente al lado o debajo del botón de conversión principal.

---

## 4. Rigor Estadístico en Experimentos A/B
- **Hipótesis Estructurada:** *"Si cambiamos `[Variable X]` en `[Superficie Y]` para `[Segmento Z]`, entonces la métrica primaria `[Conversión a Trial Activo]` aumentará al menos `[MDE %]` porque `[Principio Psicológico / Evidencia de Fricción]`"*.
- **Cálculo de Muestra y Guardrails:** No declarar ganador un experimento A/B antes de alcanzar una **significancia estadística $\ge 95\%$ ($p < 0.05$)**, una potencia estadística $\ge 80\%$ y al menos un ciclo semanal completo de tráfico (7–14 días), monitoreando métricas *guardrail* (latencia de página, tasa de error y cancelaciones).

