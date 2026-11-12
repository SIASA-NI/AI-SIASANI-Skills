---
name: autonomous-workflow-cron-swarm
description: >-
  Orquestación de automatizaciones autónomas 24/7, tareas programadas (Cron con zona horaria
  IANA), pipelines dirigidos por eventos (Webhooks / n8n / Workers), rutinas autónomas de
  mantenimiento/respaldo/auditoría y coordinación de enjambres multi-agente (Swarm) trabajando
  en paralelo. Activar cuando el usuario pida automatizar tareas recurrentes, programar trabajos
  autónomos o coordinar múltiples agentes en paralelo.
---

# ⚙️ Autonomous Workflow, Cron & Multi-Agent Swarm Orchestrator (Automatización 24/7 y Enjambres de Agentes)

> **Rol Especializado:** Principal Autonomous Automation & Multi-Agent Swarm Architect (+12 años en sistemas distribuidos dirigidos por eventos, motores Cron de alta confiabilidad, automatización de procesos sin supervisión y coordinación de agentes en paralelo).  
> **Inspiración y Estándares:** `antigravity/builtin/skills/automation`, `antigravity/builtin/skills/native_swarm`, `obra/superpowers` (*Subagent-Driven Development*) y patrones de ejecución idempotente con bloqueo distribuido (*Distributed Cron Locks*).  
> **Misión:** Diseñar automatizaciones programadas y flujos multi-agente que operen de manera 100% autónoma, segura e idempotente, encargándose de mantenimiento, respaldos, auditorías, reportes y desarrollo en paralelo sin intervención manual.

---

## 🎯 1. Principios de Automatización Autónoma Confiable (Unattended Execution)

1. **Prompts y Scripts 100% Autocontenidos (`Zero Interactive Dependency`):**
   - Una tarea programada que corre a las 03:00 AM no tiene a un humano frente a la pantalla para responder preguntas.
   - **Regla Inviolable:** Toda automatización programada debe tener claramente definidas sus rutas absolutas, credenciales de solo-lectura o alcance acotado, criterios de éxito y canal de entrega del reporte, **sin bloquearse jamás esperando un prompt interactivo (`stdin`)**.

2. **Zona Horaria Explícita (`CRON_TZ=<IANA_Timezone>`):**
   - Nunca asumir que el servidor está en la misma hora local que el usuario. Declarar siempre la zona horaria IANA explícita (ej. `CRON_TZ=America/Managua 0 8 * * 1-5` o `America/Mexico_City`, `America/New_York`, `Europe/Madrid`).

3. **Idempotencia y Exclusión Mutua (`Single-Instance Lock`):**
   - Utilizar bloqueos (`flock -n /tmp/job.lock` en Linux o *Advisory Locks* `pg_try_advisory_lock` en PostgreSQL) para garantizar que si una ejecución programada tarda más de lo previsto, la siguiente corrida no se solape ni corrompa el estado.

---

## ⏰ 2. Catálogo de Automatizaciones Esenciales para Operar un Producto en Piloto Automático

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│              LAS 5 AUTOMATIZACIONES NUCLEARES DE UN SISTEMA AUTÓNOMO 24/7              │
├──────┬──────────────────────────┬──────────────────┬───────────────────────────────────┤
│ #    │ AUTOMATIZACIÓN           │ FRECUENCIA CRON  │ ACCIÓN AUTÓNOMA                   │
├──────┼──────────────────────────┼──────────────────┼───────────────────────────────────┤
│  1   │ Centinela de Salud y     │ Cada 5 minutos   │ Verifica HTTP 200 en portales/APIs│
│      │ Certificados SSL         │ (`*/5 * * * *`)  │ y validez > 15 días del cert TLS. │
├──────┼──────────────────────────┼──────────────────┼───────────────────────────────────┤
│  2   │ Respaldo Físico y        │ Diario 02:00 AM  │ Ejecuta `pg_dump`, verifica tamaño│
│      │ Control de Disco (`df`)  │ (`0 2 * * *`)    │ > 0, registra en catálogo y alerta│
│      │                          │                  │ si el espacio libre baja de 20 GB.│
├──────┼──────────────────────────┼──────────────────┼───────────────────────────────────┤
│  3   │ Conciliación Contable /  │ Diario 03:00 AM  │ Verifica cuadre de partida doble, │
│      │ Reintento de Outbox      │ (`0 3 * * *`)    │ webhooks pendientes y secuencias. │
├──────┼──────────────────────────┼──────────────────┼───────────────────────────────────┤
│  4   │ Reporte Ejecutivo        │ Lunes a Viernes  │ Envía resumen de ventas, nuevos   │
│      │ Matutino (Digest)        │ 07:30 AM         │ registros, tickets SLA y estado de│
│      │                          │ (`30 7 * * 1-5`) │ infraestructura al administrador. │
├──────┼──────────────────────────┼──────────────────┼───────────────────────────────────┤
│  5   │ Auditoría Semanal de     │ Domingos 04:00 AM│ Escanea dependencias vulnerables, │
│      │ Seguridad y Drift        │ (`0 4 * * 0`)    │ paridad documental e integridad.  │
└──────┴──────────────────────────┴──────────────────┴───────────────────────────────────┘
```

---

## 🐝 3. Orquestación de Enjambres Multi-Agente (`Subagent & Swarm Architecture`)

Cuando una tarea de ingeniería o investigación es grande y divisible en partes independientes, aplicar el patrón de **Coordinador + Especialistas en Paralelo**:

1. **Particionamiento sin Colisión de Archivos (*Disjoint File Ownership*):**
   - Si 3 agentes editan código al mismo tiempo, asignar a cada uno un módulo o capa disjunta (o un workspace aislado `workspace: "share"` / `"branch"`) para evitar que un agente sobreescriba el trabajo del otro.
2. **Brief Público Autocontenido:**
   - Un subagente nuevo arranca con contexto limpio; su instrucción (*brief*) debe especificar exactamente: **qué archivos le pertenecen, qué reglas debe cumplir y qué evidencia exacta debe devolver al terminar**.
3. **Par de Ojos Independiente (*Author vs. Verifier*):**
   - Asignar siempre un agente verificador (`qa-evidence-based-verification`) distinto del agente autor para probar la implementación de extremo a extremo antes de dar por cerrado el trabajo.

---

## ⛔ 4. Checklist Obligatorio de Cierre en Automatización (Quality Gate)

- [ ] ¿La expresión Cron incluye su zona horaria IANA explícita (`CRON_TZ=...`) y fue validada contra el horario local esperado?
- [ ] ¿El script o tarea programada cuenta con bloqueo contra ejecuciones simultáneas (`flock` / advisory lock) y timeout máximo?
- [ ] ¿Se realizó una corrida de prueba en vivo confirmando que la automatización termina con código `0` sin pedir confirmación manual?
- [ ] ¿Existe registro en bitácora (log con timestamp) tanto de las ejecuciones exitosas como de los fallos?
