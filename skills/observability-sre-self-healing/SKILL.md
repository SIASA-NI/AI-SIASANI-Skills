---
name: observability-sre-self-healing
description: >-
  Ingeniería de Confiabilidad de Sitios (SRE), observabilidad distribuida con OpenTelemetry
  (Métricas, Logs estructurados y Trazas), tableros Prometheus/Grafana, alertas inteligentes sin
  fatiga, Circuit Breakers y mecanismos de auto-recuperación (Self-Healing Infrastructure) para
  que el sistema opere 24/7 de forma autónoma. Activar al configurar monitoreo, healthchecks,
  alertas o resiliencia operativa.
---

# 🩺 Observability, SRE & Self-Healing Systems Architect (Operación Autónoma 24/7 y Auto-Recuperación)

> **Rol Especializado:** Principal Site Reliability Engineer (SRE) & Autonomous Operations Architect (+15 años en observabilidad OpenTelemetry, alta disponibilidad 99.99%, ingeniería de resiliencia y sistemas auto-reparables).  
> **Inspiración y Estándares:** *Google SRE Book*, **OpenTelemetry (OTel) Specification**, **RED / USE Metrics Methods** y patrones de resiliencia (*Circuit Breaker, Bulkhead, Timeout, Retry with Jitter*).  
> **Misión:** Dotar a la plataforma de visibilidad total en tiempo real y reflejos autónomos de auto-curación (*Self-Healing*), de modo que el sistema detecte anomalías, aísle fallos y se recupere automáticamente las 24 horas del día sin intervención humana.

---

## 🎯 1. Los 3 Pilares de Observabilidad + Las 4 Señales Doradas (Golden Signals)

Todo servicio en producción debe emitir telemetría correlacionada por `trace_id` (`W3C Trace Context`) midiendo las **4 Señales Doradas de SRE**:
1. **Latencia (Latency):** Tiempo de respuesta diferenciando peticiones exitosas (`2xx`) de peticiones fallidas (`5xx`), medido en percentiles **P50, P95 y P99** (nunca solo promedios que ocultan valores extremos).
2. **Tráfico (Traffic):** Peticiones HTTP por segundo (RPS), mensajes procesados en colas por minuto y sesiones activas.
3. **Errores (Errors):** Tasa de respuestas `5xx`, excepciones no controladas y fallos de validación de negocio.
4. **Saturación (Saturation):** Uso de CPU, memoria RAM, conexiones activas del pool de PostgreSQL (`pg_stat_activity`) y porcentaje de disco libre.

---

## 🔄 2. Arquitectura de Auto-Recuperación Autónoma (`Self-Healing Loop`)

Para que el sistema funcione sola y continuamente en producción, implementar estos **4 mecanismos de auto-curación**:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                 BUCLE DE AUTO-RECUPERACIÓN AUTÓNOMA (SELF-HEALING SRE)                 │
├──────┬──────────────────────────┬──────────────────────────────────────────────────────┤
│ #    │ VECTOR DE FALLO          │ MECANISMO AUTÓNOMO DE AUTO-CURACIÓN                  │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  1   │ Proceso Colgado / Deadlock│ `healthcheck` activo en Docker/K8s (`/health/live`)  │
│      │ o Fuga de Memoria        │ + `restart: unless-stopped` y límite `mem_limit`.    │
│      │                          │ Si el servicio no responde en 3 intentos, se reinicia│
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  2   │ Caída Temporal de API    │ **Circuit Breaker + Retry con Exponential Backoff &  │
│      │ Externa o Base de Datos  │ Jitter:** Abre el circuito para no agotar hilos y    │
│      │                          │ reintenta automáticamente desde la tabla Outbox.     │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  3   │ Saturación de Disco por  │ Rotación automática de logs Docker (`max-size: 20m`, │
│      │ Logs o Archivos Temp     │ `max-file: 5`) y cron job de purga de `/tmp` y       │
│      │                          │ compresión/rotación de respaldos antiguos.           │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  4   │ Expiración de Certificados│ Renovación automática programada con `certbot renew` │
│      │ SSL/TLS (Let's Encrypt)  │ + hook `--deploy-hook "docker exec nginx -s reload"`.│
└──────┴──────────────────────────┴──────────────────────────────────────────────────────┘
```

### Configuración Canónica de Healthcheck y Rotación de Logs en Docker Compose
```yaml
services:
  api-service:
    restart: unless-stopped
    healthcheck:
      test: ["CMD-SHELL", "curl -fsS http://127.0.0.1:8080/health || exit 1"]
      interval: 15s
      timeout: 5s
      retries: 3
      start_period: 20s
    logging:
      driver: "json-file"
      options:
        max-size: "20m"
        max-file: "5"
```

---

## 🚨 3. Alertas Inteligentes de Alta Señal (Zero Alert Fatigue)

Una alerta solo debe despertar o notificar por WhatsApp/Telegram/Slack/Email al administrador si es **accionable e inmediata**:
- **Alerta Crítica P0 (Notificación Inmediata):** Caída de un servicio tras fallar su auto-reinicio, tasa de errores `5xx > 2%` durante 3 minutos, fallo en la ejecución del backup diario, o espacio libre en disco `< 15%`.

---

## ⛔ 4. Checklist Obligatorio de Cierre en Observabilidad y SRE (Quality Gate)

- [ ] ¿Todos los microservicios exponen un endpoint `/health` real que verifica conectividad con la base de datos?
- [ ] ¿Los contenedores tienen configurados `restart: unless-stopped`, `healthcheck` y límites de rotación de logs (`max-size`)?
- [ ] ¿Los logs se emiten en formato JSON estructurado con `traceId`, `tenantId`, `level` y `timestampUtc`?
- [ ] ¿Se verificó que si se reinicia el contenedor de base de datos o broker, los servicios reconectan automáticamente sin quedar muertos?

