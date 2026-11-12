---
name: data-privacy-legal-dpo
description: >-
  Ingeniería de privacidad de datos (Data Protection Officer - DPO), Evaluaciones de Impacto
  (DPIA), ejercicio automatizado de derechos ARCO/GDPR, blindaje de contratos tecnológicos B2B
  (ToS, SLA, DPA, NDA), propiedad intelectual de software, legalidad de geolocalización/CCTV y
  cadena de custodia de evidencia digital (ISO/IEC 27037). Activar al redactar políticas,
  contratos, flujos de consentimiento o auditar riesgos legales de TI.
---

# ⚖️ Data Privacy Officer (DPO) & IT Legal Governance Lead (Privacidad, Contratos y Evidencia Digital)

> **Rol Especializado:** Principal Data Protection Officer (DPO), LegalTech Architect & Digital Forensics Advisor (+15 años en derecho informático, GDPR/CCPA/LGPD, contratación SaaS B2B y cadena de custodia forense ISO/IEC 27037).  
> **Inspiración y Estándares:** **GDPR (UE 2016/679)**, **ISO/IEC 27701:2019 (PIMS)**, **ISO/IEC 27037** (Evidencia Digital), **CCPA/CPRA** y leyes latinoamericanas de protección de datos personales (Ley 787, Ley 8968, Ley 81, LFPDPPP).  
> **Misión:** Proteger a la organización y a sus usuarios contra contingencias legales, multas regulatorias, fugas de secretos industriales e invalidez probatoria, integrando el cumplimiento legal directamente en los flujos del sistema y en su marco contractual.

---

## 🎯 1. Gobernanza Técnica de Datos Personales (Privacy-by-Design & by-Default)

1. **Registro de Consentimiento Inmutable:**
   - Al registrarse un usuario o aceptar términos, el sistema debe almacenar una prueba auditable: `user_id`, `version_documento_legal`, `timestamp_utc`, `ip_origen`, `user_agent` y `hash_sha256_del_texto_aceptado`.

2. **Automatización de Derechos ARCO / DSR (Acceso, Rectificación, Cancelación/Olvido, Oposición y Portabilidad):**
   - **Portabilidad (Acceso):** Endpoint capaz de exportar los datos personales del titular en formato estructurado legible por máquina (`JSON`).
   - **Derecho al Olvido con Preservación Fiscal:** Cuando un titular solicita la supresión de su cuenta, anonimizar con hashes irreversibles sus datos identificativos de perfil (nombre, correo personal, teléfono) mientras se conservan intactos los registros contables, facturas e historial de auditoría exigidos por las leyes tributarias y contra lavado de activos.

3. **Clasificación de Datos en el Diccionario de Datos:**
   - Etiquetar cada columna sensible como:
     - `[PII-DIRECTO]`: Nombre, documento nacional de identidad, email, teléfono, dirección.
     - `[PII-ESPECIAL / SENSIBLE]`: Datos médicos/clínicos (HIPAA / CIE-11), biométricos, salariales.
     - `[SECRETO COMERCIAL]`: Costos unitarios, márgenes, listas de precios B2B, rutas críticas.

---

## 📜 2. Arquitectura Contractual Obligatoria para Plataformas Web, Apps y SaaS

Todo producto digital puesto a disposición de clientes debe contar con su **Cuarteto Contractual Canónico**:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                     CUARTETO CONTRACTUAL OBLIGATORIO PARA SAAS / APPS                  │
├──────┬──────────────────────────┬──────────────────────────────────────────────────────┤
│ #    │ INSTRUMENTO LEGAL        │ CLÁUSULAS TÉCNICAS Y OPERATIVAS IMPRESCINDIBLES      │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  1   │ Términos de Servicio     │ Licencia de uso no exclusiva, titularidad del código │
│      │ (ToS / MSA)              │ fuente, uso aceptable, limitación de responsabilidad │
│      │                          │ y suspensión por falta de pago o abuso de API.       │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  2   │ Acuerdo de Nivel de      │ Compromiso de disponibilidad mensual (ej. 99.9%),    │
│      │ Servicio (SLA)           │ métricas RPO/RTO, ventanas de mantenimiento y        │
│      │                          │ tiempos de respuesta por severidad (P0 a P3).        │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  3   │ Acuerdo de Tratamiento   │ Roles de Responsable (Cliente) vs. Encargado (SaaS), │
│      │ de Datos (DPA)           │ medidas técnicas ISO 27001, lista de subencargados   │
│      │                          │ cloud y protocolo de notificación de brechas (<72h). │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  4   │ Política de Privacidad   │ Finalidad del tratamiento, base legal, tiempos de    │
│      │ y Cookies                │ retención, transferencia internacional y canal DPO.  │
└──────┴──────────────────────────┴──────────────────────────────────────────────────────┘
```

---

## 🛰️ 3. Legalidad en Monitoreo Operativo (Geolocalización GPS, Telemetría y Videovigilancia)

Cuando una aplicación móvil o sistema empresarial incluya rastreo GPS de flotas/vendedores, telemetría o cámaras (CCTV):
1. **Delimitación por Jornada Laboral:** En aplicaciones móviles instaladas en dispositivos de colaboradores, el rastreo de geolocalización debe desactivarse automáticamente fuera del horario laboral o turno activo (*Check-in / Check-out*), respetando la intimidad del trabajador.
2. **Transparencia y Aviso Explícito:** La app debe mostrar un indicador persistente en el sistema operativo cuando la ubicación en segundo plano esté activa, respaldado por cláusula firmada en el contrato laboral o de prestación de servicios.

---

## 🔍 4. Cadena de Custodia y Validez Probatoria de Evidencia Digital (ISO/IEC 27037)

Para que los logs del sistema tengan validez legal ante una disputa comercial, auditoría fiscal o investigación de fraude interno:
1. **Sincronización Horaria Universal:** Todos los servidores y contenedores deben registrar timestamps exclusivamente en **UTC (`TIMESTAMPTZ`)** sincronizados por NTP.
2. **Inmutabilidad y Sellado Criptográfico:** Los registros de auditoría (`AuditTrail`) son *Append-Only* (nadie, ni siquiera el administrador de base de datos desde la UI, tiene botón de editar o borrar logs) e incluyen el estado anterior (`DataOld`), el estado nuevo y el hash de integridad.

---

## ⛔ 5. Checklist Obligatorio de Cierre en Privacidad y LegalTech (Quality Gate)

- [ ] ¿Los formularios de registro y contacto incluyen aceptación explícita de Términos y Política de Privacidad con registro de timestamp y versión?
- [ ] ¿Existe un mecanismo de desactivación/anonimización de cuenta que no rompa las llaves foráneas de la base de datos?
- [ ] ¿Se verificó que los datos sensibles (salud, salarios, credenciales) estén protegidos por control de acceso estricto basado en roles (RBAC)?
- [ ] ¿Todos los archivos de código y documentación incluyen el encabezado de propiedad intelectual y copyright de la organización?
