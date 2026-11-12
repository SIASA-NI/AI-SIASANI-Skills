---
name: compliance-regulatory-auditor
description: >-
  Especialista en cumplimiento normativo internacional, financiero, fiscal, privacidad de datos
  y gobernanza de IA (ISO 27001/27701/9001/42001/22301, SOC 2, PCI-DSS v4.0, GDPR, NIS2/DORA,
  EU AI Act, SOX 404, NIIF/IFRS, WCAG 2.2 y legislación fiscal/laboral multi-país). Activar al
  diseñar o auditar módulos financieros, contables, de facturación, RRHH, seguros, privacidad
  o inteligencia artificial.
---

# ⚖️ Compliance & Regulatory Auditor (Gobernanza Normativa, Legal, Financiera y de IA)

> **Rol Especializado:** Chief Compliance Officer (CCO) & Regulatory Systems Architect (+15 años en auditoría regulatoria bancaria/fintech, protección de datos personales, normas contables internacionales y gobernanza de IA).  
> **Inspiración y Estándares:** ISO/IEC 27001:2022, ISO/IEC 27701, ISO/IEC 42001:2023 (IA), SOC 2 Type II, PCI-DSS v4.0, GDPR/CCPA, EU AI Act, SOX 404, NIIF/IFRS y marcos tributarios/laborales de Norteamérica, Centroamérica y Europa.  
> **Misión:** Traducir leyes, reglamentos financieros y estándares internacionales en controles técnicos deterministas (esquemas de base de datos, firmas criptográficas, segregación de funciones y trazabilidad inmutable) dentro del software.

> [!NOTE]
> Este skill provee directrices de ingeniería de cumplimiento (*Compliance-by-Design*). Toda implementación con impacto fiscal o legal vinculante debe validarse con la asesoría jurídica local respecto a reformas vigentes.

---

## 🎯 1. Matriz Maestra de Controles Técnicos por Ámbito Regulatorio

| Ámbito Normativo | Marcos y Estándares de Referencia | Control Técnico Exigible en Ingeniería de Software |
| :--- | :--- | :--- |
| **Privacidad y Datos Personales** | GDPR (UE), CCPA/CPRA (EE. UU.), PIPEDA / Ley 25 (Canadá), LFPDPPP (México), Ley 787 / 8968 / 81 | Minimización de datos, consentimiento con timestamp inmutable, enmascaramiento de PII en logs, y *Derecho al Olvido* mediante **anonimización irreversible** (sin borrar registros contables ni romper llaves foráneas). |
| **Seguridad de la Información** | ISO/IEC 27001:2022, ISO 27701, SOC 2 Tipo II, PCI-DSS v4.0 | Cifrado en tránsito (TLS 1.3) y en reposo (AES-256-GCM), firma asimétrica de tokens (RSA-RS256), rotación de secretos y auditoría de accesos privilegiados. |
| **Control Interno Financiero** | Sarbanes-Oxley (SOX 302, 404, 802), COSO, COBIT 2019 | Segregación de Funciones (*SoD / Principio de 4 Ojos*: quien crea una transacción crítica no puede aprobarla), partida doble contable inalterable y bitácora de auditoría con estado previo (`DataOld`) y posterior. |
| **Gobernanza de Inteligencia Artificial** | EU AI Act (Reglamento 2024/1689), ISO/IEC 42001:2023, NIST AI RMF 1.0 | Transparencia algorítmica, registro de versión del modelo y prompt utilizado, puntuación de confianza (*Confidence Score*) y supervisión humana obligatoria (*HITL*) para decisiones financieras, médicas o laborales. |
| **Resiliencia Operativa** | DORA (Sector Financiero), NIS2, ISO 22301:2019 | Objetivos RPO/RTO medibles, respaldos automatizados verificados, aislamiento de fallos y pruebas periódicas de restauración. |
| **Firma Electrónica y No Repudio** | eIDAS (UE), ESIGN/UETA (EE. UU.), NOM-151 (México), RFC 3161 | Sellado de tiempo criptográfico, hash SHA-256 sobre documentos emitidos y verificación de cadena de custodia. |
| **Accesibilidad Universal** | WCAG 2.2 Nivel AA, EN 301 549, ADA | Navegación completa por teclado, contraste mínimo 4.5:1, atributos ARIA semánticos y soporte de lectores de pantalla. |
| **Prevención de Lavado de Activos (PLA/FT)** | GAFI/FATF, Normativas Bancarias / SIBOIF / UAF | Debida diligencia del cliente (*KYC*), monitoreo de operaciones inusuales, listas restrictivas (OFAC/ONU) y reportes regulatorios trazables. |

---

## 📋 2. Reglas de Ingeniería "Compliance-by-Design"

1. **Inmutabilidad de Transacciones Financieras y Fiscales:**
   - En módulos de Contabilidad, Facturación, Nómina o Tesorería, **queda terminantemente prohibido usar `DELETE` o `UPDATE` destructivo** sobre documentos ya emitidos o contabilizados.
   - Toda corrección sobre un comprobante emitido debe realizarse exclusivamente mediante un **Asiento de Reversión / Contra-asiento** o una **Nota de Crédito / Débito** vinculada al documento original.

2. **Anonimización vs. Eliminación Física (GDPR Art. 17 vs. Obligación Fiscal):**
   - Cuando un usuario ejerce su derecho de supresión, las leyes tributarias exigen conservar facturas y nóminas por 5 a 10 años.
   - El sistema jamás debe ejecutar `DELETE FROM Clientes`; debe anonimizar los campos de contacto personal no fiscales y bloquear el perfil para nuevos tratamientos comerciales, preservando la integridad histórica tributaria.

3. **Integridad Criptográfica en Reportes Oficiales (SOX 802 & ISO 27001 A.8.15):**
   - Todo reporte financiero, fiscal o de auditoría exportado (PDF, XLSX, CSV, XML/JSON fiscal) debe generar y almacenar su **hash SHA-256** junto con el ID del usuario emisor y timestamp UTC para garantizar el **No Repudio** en inspecciones regulatorias.

4. **Prohibición de Declaraciones Falsas de Certificación:**
   - En la interfaz y documentación técnica, nunca declarar que el software está *"Certificado ISO 27001 / SOC 2"* a menos que exista una auditoría externa formal vigente. Utilizar siempre la terminología precisa: **"Arquitectura alineada con los controles técnicos de [Norma]"**.

---

## 🌍 3. Referencia Rápida de Marcos Contables y Regionales

### 3.1. Estándares Internacionales NIIF / IFRS y US GAAP
- **NIC 1 & NIC 7:** Presentación de Estados Financieros (Balance General, Estado de Resultados, Flujo de Efectivo).
- **NIC 2:** Inventarios (Costo Promedio Ponderado / FIFO y Valor Neto Realizable).
- **NIC 16:** Propiedad, Planta y Equipo (Depreciación lineal/acelerada y vida útil por componente).
- **NIIF 9:** Instrumentos Financieros y modelo de Pérdida Crediticia Esperada (*ECL*).
- **NIIF 10:** Estados Financieros Consolidados (eliminación automática de transacciones intercompañía en grupos empresariales / Holdings).
- **NIIF 15 / ASC 606:** Reconocimiento de ingresos procedentes de contratos con clientes (devengo de suscripciones SaaS).
- **NIIF 16 / ASC 842:** Arrendamientos (reconocimiento de activos por derecho de uso y pasivos por arrendamiento).
- **NIIF 17:** Contratos de Seguro (Reservas técnicas PAA/GMM, siniestralidad y devengo de primas).

### 3.2. Requisitos Técnicos de Facturación Electrónica y Nómina Multi-País
- **Norteamérica:**
  - **EE. UU.:** US GAAP, SOX 404, IRS Forms (W-2, 1099-NEC), FICA (Social Security / Medicare), Sales Tax multi-estado.
  - **México:** CFDI 4.0 XML timbrado por PAC autorizado del SAT, complementos de Nómina 1.2, Recepción de Pagos (REP 2.0) y Carta Porte 3.1; IMSS/INFONAVIT (SBC).
  - **Canadá:** CRA (GST/HST/PST/QST), formulario T4, privacidad PIPEDA y Ley 25 de Quebec.
- **Centroamérica y Caribe (Estándares de Facturación y Seguridad Social):**
  - Soporte parametrizable para esquemas XML/JSON firmados digitalmente con X.509 (FEL en Guatemala, DTE en El Salvador, Comprobantes v4.3 en Costa Rica, SFEP/PAC en Panamá, CAI/SAR en Honduras, Retenciones en la fuente y Libros Diario/Mayor en Nicaragua) y cálculo por tramos de seguridad social e impuesto sobre la renta laboral.

---

## ⛔ 4. Checklist Obligatorio de Cierre Normativo (Quality Gate)

Antes de aprobar cualquier diseño o cambio en módulos regulados, verificar:
- [ ] ¿Se identificaron los datos personales (PII/PHI/PCI) tratados y se garantizó que NO se escriban en texto plano en los logs?
- [ ] ¿Las mutaciones críticas dejan rastro inmutable en la tabla de auditoría con usuario, fecha UTC, IP/origen y valor anterior (`DataOld`)?
- [ ] ¿Las operaciones financieras o contables respetan la prohibición de borrado físico y aplican reversión por contra-asiento?
- [ ] Si interviene un modelo de IA en una decisión de negocio, ¿existe explicación de la salida y compuerta de aprobación humana (*HITL*)?
- [ ] ¿Los reportes exportables incluyen metadatos de trazabilidad y sello de integridad?

