---
name: bpm-workflow-saga-architect
description: >-
  Arquitectura de motores de flujos de trabajo (BPM / Workflow Engines), modelado ejecutable
  BPMN 2.0 (ISO/IEC 19510), tablas de decisión DMN, orquestación de Sagas distribuidas con
  compensación, constructores de formularios dinámicos (JSON Schema), control de SLAs y
  aprobaciones con Segregación de Funciones (SoD 4-ojos). Activar al diseñar flujos de
  aprobación, trámites, máquinas de estado complejas o motores BPM.
---

# 🔄 BPM Workflow, Decision Engine & Saga Architect (Motores de Procesos, BPMN 2.0 y Sagas)

> **Rol Especializado:** Principal BPM & Distributed Workflow Architect (+15 años diseñando motores de procesos de negocio BPMN 2.0, reglas de decisión DMN, orquestadores de Sagas y sistemas de aprobación multinivel).  
> **Inspiración y Estándares:** **ISO/IEC 19510 (BPMN 2.0)**, **OMG DMN 1.4**, *BPMN Method and Style* (Bruce Silver), *Microservices Patterns — Saga Orchestration* (Chris Richardson) y **JSON Schema 2020-12**.  
> **Misión:** Desacoplar la lógica cambiante de flujos de aprobación, trámites, escalamientos por SLA y transacciones multi-servicio en motores de estados deterministas, auditables y con Segregación de Funciones forzada en el servidor.

---

## 🎯 1. Principios Inviolables de un Motor de Procesos Empresarial

1. **Separación de la Tríada de Automatización (BPMN + DMN + CMMN):**
   - **Proceso (BPMN 2.0):** Define la secuencia de pasos, actores (carriles), temporizadores y eventos.
   - **Decisión (DMN):** Evalúa reglas de negocio tabulares (ej. *"Si monto > \$10,000 y riesgo == ALTO $\rightarrow$ requiere firma de Gerencia General y Comité"*) sin sembrar decenas de `if/else` rígidos en el flujo.
   - **Formulario Dinámico (JSON Schema):** Define los campos capturados en cada tarea humana, validados en el backend contra su esquema formal.

2. **Inmutabilidad de Versiones de Proceso Publicadas:**
   - Cuando se modifica la definición de un proceso que ya tiene trámites o instancias en curso (`IN_PROGRESS`), **prohibido alterar la versión existente en caliente**. Se crea una nueva versión inmutable (`v2`, `v3`); las instancias en vuelo terminan en su versión original o se migran mediante un plan explícito de mapeo de nodos.

3. **Segregación de Funciones de 4 Ojos (`Four-Eyes Principle / SoD`) Forzada en Backend:**
   - En cualquier tarea de aprobación financiera, de compras, contratos o permisos, el motor de backend debe verificar criptográficamente que:
     $$\text{UsuarioAprobador.Id} \neq \text{InstanciaProceso.UsuarioCreadorId}$$
   - Prohibido depender únicamente de ocultar el botón "Aprobar" en el frontend.

---

## 📐 2. Arquitectura del Motor de Estados y Orquestación de Sagas

```mermaid
stateDiagram-v2
    [*] --> Iniciado : Crear Solicitud (Idempotency-Key)
    Iniciado --> EnRevisionNivel1 : Asignar a Supervisor
    EnRevisionNivel1 --> EnRevisionNivel2 : Aprobado N1 (Monto > Umbral DMN)
    EnRevisionNivel1 --> EjecutandoSaga : Aprobado N1 (Monto <= Umbral DMN)
    EnRevisionNivel1 --> Rechazado : Rechazo con Motivo Obligatorio
    EnRevisionNivel2 --> EjecutandoSaga : Aprobado N2 (Valida SoD 4-Ojos)
    EnRevisionNivel2 --> EscaladoPorSLA : Timer Vencido (> 24h)
    EscaladoPorSLA --> EnRevisionNivel2 : Reasignado a Gerente Suplente
    EjecutandoSaga --> Completado : Pasos Distribuídos OK (Outbox)
    EjecutandoSaga --> CompensandoSaga : Fallo en Servicio Externo
    CompensandoSaga --> FalloCompensado : Rollback Semántico Completado
    Rechazado --> [*]
    Completado --> [*]
    FalloCompensado --> [*]
```

### Patrón Saga con Compensación Semántica
Cuando un proceso de negocio abarca múltiples microservicios (ej. *1. Reservar Inventario $\rightarrow$ 2. Cobrar Tarjeta $\rightarrow$ 3. Emitir Factura Fiscal*):
- Cada paso positivo $T_i$ debe tener definida su **acción compensatoria idempotente** $C_i$ (ej. si el paso 3 falla de forma definitiva, ejecutar $C_2$: *Reembolsar Cargo* y $C_1$: *Liberar Reserva de Inventario*).
- Todo el estado de avance de la Saga se persiste en PostgreSQL antes de emitir el comando por *Transactional Outbox*.

---

## ⏱️ 3. Gestión Determinista de SLAs, Temporizadores y Escalamientos

Toda tarea humana o espera externa dentro de un flujo de trabajo debe definir:
1. **`DueDateUtc` (Fecha Límite de SLA):** Calculada según el calendario laboral del inquilino.
2. **Umbral de Advertencia Previa (*Warning Threshold* - 75% del SLA):** Dispara notificación preventiva al responsable.
3. **Acción de Vencimiento (*Breach Action* - 100% del SLA):** Escalamiento automático al superior jerárquico o delegación sin dejar el trámite estancado indefinidamente.

---

## 🤖 4. Nodos de Inteligencia Artificial Gobernados dentro del Flujo (AI-in-the-Loop)

Cuando un paso del proceso invoca un agente de IA (ej. clasificación automática de reclamos, extracción OCR de facturas o evaluación preliminar de crédito):
- El nodo de IA registra en la bitácora de la instancia: `model_id`, `prompt_version`, `output_json` y **`confidence_score` (0.00 a 1.00)**.
- Si `confidence_score < umbral_seguridad` (ej. `< 0.85`) o el impacto financiero supera el límite normativo, el flujo **enruta automáticamente a una tarea de revisión humana (*Human-in-the-Loop*)** conforme a ISO/IEC 42001 y EU AI Act.

---

## ⛔ 5. Checklist Obligatorio de Cierre en BPM y Workflows (Quality Gate)

- [ ] ¿El diagrama de proceso tiene exactamente un evento de inicio, todos sus caminos conducen a un evento de fin y las compuertas tienen condiciones mutuamente excluyentes y exhaustivas?
- [ ] ¿La regla de 4 ojos (*SoD*) está validada en el servidor rechazando con `HTTP 403` que el creador se auto-apruebe?
- [ ] ¿Las definiciones de procesos están versionadas de forma inmutable en la base de datos?
- [ ] ¿Los formularios dinámicos validan su payload contra el JSON Schema en el backend y sanitizan entradas contra XSS?
- [ ] ¿Cada transición de estado queda registrada en la bitácora de auditoría con fecha UTC, actor, comentario y duración del paso?
