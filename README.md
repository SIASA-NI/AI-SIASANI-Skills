# 🏛️ Compendio Maestro 360° de Agent Skills — De la Idea e Investigación hasta la Aplicación, Publicación Cloud, Publicidad, Redes Sociales y Operación Autónoma 24/7

> **Estándar Abierto `SKILL.md` (`agentskills.io`)** · **45 Skills de Nivel Principal / Staff Engineer** · Compatible con **Google Antigravity**, **Claude Code**, **Cursor**, **GitHub Copilot Agent**, **Windsurf** y **OpenAI Codex**.

Este repositorio es un **compendio integral y autocontenido de 45 Agent Skills** diseñado para cubrir **el 100% del ciclo de vida de un producto tecnológico y de negocio**: desde la chispa de la idea inicial, la investigación profunda y el diseño de arquitectura, pasando por el desarrollo Web, Móvil, Escritorio, Pagos e Inteligencia Artificial, hasta las pruebas empíricas de QA, ciberseguridad Zero-Trust, documentación viva, videos demostrativos automatizados, despliegue en la nube, campañas publicitarias, **automatización de redes sociales** y **operación autónoma 24/7 con auto-recuperación**.

Sintetiza estándares de ingeniería probados en producción junto con los patrones de los repositorios más influyentes del ecosistema global (**`anthropics/skills`**, **`obra/superpowers`**, **`vercel-labs/skills`**, **`NVIDIA/NeMo-Guardrails`**, **`rwspatin/bpmn-skill`**, **DDD**, **OWASP ASVS v4**, **ISO/IEC 27001/42001/9001/22301**, **NIIF/IFRS** y **C4 Model**).

---

## 🧭 Mapa del Ciclo de Vida Completo 360° (Las 9 Fases y sus 45 Skills)

```mermaid
flowchart TB
    classDef truth fill:#831843,stroke:#EC4899,stroke-width:2px,color:#F8FAFC
    classDef discovery fill:#0C4A6E,stroke:#38BDF8,stroke-width:2px,color:#F8FAFC
    classDef arch fill:#78350F,stroke:#F59E0B,stroke-width:2px,color:#F8FAFC
    classDef build fill:#064E3B,stroke:#10B981,stroke-width:2px,color:#F8FAFC
    classDef domain fill:#1E3A8A,stroke:#60A5FA,stroke-width:2px,color:#F8FAFC
    classDef qa fill:#14532D,stroke:#4ADE80,stroke-width:2px,color:#F8FAFC
    classDef docs fill:#3B0764,stroke:#C084FC,stroke-width:2px,color:#F8FAFC
    classDef ops fill:#312E81,stroke:#818CF8,stroke-width:2px,color:#F8FAFC
    classDef growth fill:#7C2D12,stroke:#FB923C,stroke-width:2px,color:#F8FAFC

    subgraph F0 ["Fase 0: Mantenimiento de la Verdad y Meta-Skills (4)"]
        S01["01. anti-sycophancy-direct-truth"]:::truth
        S02["02. integrity-governance-guard"]:::truth
        S03["03. skill-creator-installer"]:::truth
        S04["04. skills-curator-evaluator"]:::truth
    end

    subgraph F1 ["Fase 1: Ideación, Investigación, Finanzas y Legal (5)"]
        S05["05. deep-search-research-lead"]:::discovery
        S06["06. product-discovery-saas-strategy"]:::discovery
        S07["07. financial-fpa-roi-architect"]:::discovery
        S08["08. compliance-regulatory-auditor"]:::discovery
        S09["09. data-privacy-legal-dpo"]:::discovery
    end

    subgraph F2 ["Fase 2: Arquitectura de Software, Datos y BPM (5)"]
        S10["10. backend-ddd-clean-architect"]:::arch
        S11["11. database-postgresql-architect"]:::arch
        S12["12. api-contract-gateway-governance"]:::arch
        S13["13. bpm-workflow-saga-architect"]:::arch
        S14["14. software-architecture-diagrammer"]:::arch
    end

    subgraph F3 ["Fase 3: Creación Web, App Móvil, Desktop, IA y Voz (10)"]
        S15["15. ui-ux-design-system-architect"]:::build
        S16["16. landing-page-design"]:::build
        S17["17. webmaster-seo-landing-conversion"]:::build
        S18["18. mobile-cross-platform-architect"]:::build
        S19["19. cross-platform-desktop-packager"]:::build
        S20["20. ai-llm-rag-agentic-architect"]:::build
        S21["21. realtime-ai-streaming-lead"]:::build
        S22["22. speech-to-speech-audio-architect"]:::build
        S23["23. windows-audio-native-architect"]:::build
        S24["24. linux-audio-native-architect"]:::build
    end

    subgraph F4 ["Fase 4: Pagos Fintech, Logística, Flotas/IoT y Auditoría (6)"]
        S25["25. fintech-payments-billing-architect"]:::domain
        S26["26. logistics-distribution-systems"]:::domain
        S27["27. fleet-telematics-iot-audit"]:::domain
        S28["28. cctv-physical-security-audit"]:::domain
        S29["29. it-governance-forensic-audit"]:::domain
        S30["30. executive-board-reporting"]:::domain
    end

    subgraph F5 ["Fase 5: QA Empírico, E2E, Ciberseguridad y Auto-Curación SRE (4)"]
        S31["31. qa-evidence-based-verification"]:::qa
        S32["32. e2e-webapp-load-testing-lead"]:::qa
        S33["33. cybersecurity-zero-trust-defensive"]:::qa
        S34["34. observability-sre-self-healing"]:::qa
    end

    subgraph F6 ["Fase 6: Documentación Viva, Procesos y Videos Demo (3)"]
        S35["35. technical-documentation-process-governance"]:::docs
        S36["36. process-flow-documentation"]:::docs
        S37["37. automated-demo-video-training-producer"]:::docs
    end

    subgraph F7 ["Fase 7: Despliegue Cloud Inmutable y Mantenimiento (2)"]
        S38["38. devops-immutable-cloud-infrastructure"]:::ops
        S39["39. code-simplifier-refactoring-maintenance"]:::ops
    end

    subgraph F8 ["Fase 8 & 9: Publicidad, Redes Sociales, Cron/Swarm y Talento (6)"]
        S40["40. advertising-marketing-lead"]:::growth
        S41["41. social-media-omnichannel-automation"]:::growth
        S42["42. autonomous-workflow-cron-swarm"]:::growth
        S43["43. job-hunter-scout"]:::growth
        S44["44. resume-ats-optimizer"]:::growth
        S45["45. linkedin-profile-optimizer"]:::growth
    end

    F0 --> F1 --> F2 --> F3 --> F4 --> F5 --> F6 --> F7 --> F8
```

---

## 📂 Árbol Completo de Archivos del Repositorio (45 Skills)

Cada skill cuenta con su propio archivo canónico `SKILL.md` listo para ser descubierto por cualquier agente compatible:

```text
AI-SIASANI-Skills/
├── LICENSE
├── README.md
└── skills/
    ├── advertising-marketing-lead/SKILL.md
    ├── ai-llm-rag-agentic-architect/SKILL.md
    ├── anti-sycophancy-direct-truth/SKILL.md
    ├── api-contract-gateway-governance/SKILL.md
    ├── automated-demo-video-training-producer/SKILL.md
    ├── autonomous-workflow-cron-swarm/SKILL.md
    ├── backend-ddd-clean-architect/SKILL.md
    ├── bpm-workflow-saga-architect/SKILL.md
    ├── cctv-physical-security-audit/SKILL.md
    ├── code-simplifier-refactoring-maintenance/SKILL.md
    ├── compliance-regulatory-auditor/SKILL.md
    ├── cross-platform-desktop-packager/SKILL.md
    ├── cybersecurity-zero-trust-defensive/SKILL.md
    ├── data-privacy-legal-dpo/SKILL.md
    ├── database-postgresql-architect/SKILL.md
    ├── deep-search-research-lead/SKILL.md
    ├── devops-immutable-cloud-infrastructure/SKILL.md
    ├── e2e-webapp-load-testing-lead/SKILL.md
    ├── executive-board-reporting/SKILL.md
    ├── financial-fpa-roi-architect/SKILL.md
    ├── fintech-payments-billing-architect/SKILL.md
    ├── fleet-telematics-iot-audit/SKILL.md
    ├── integrity-governance-guard/SKILL.md
    ├── it-governance-forensic-audit/SKILL.md
    ├── job-hunter-scout/SKILL.md
    ├── landing-page-design/SKILL.md
    ├── linkedin-profile-optimizer/SKILL.md
    ├── linux-audio-native-architect/SKILL.md
    ├── logistics-distribution-systems/SKILL.md
    ├── mobile-cross-platform-architect/SKILL.md
    ├── observability-sre-self-healing/SKILL.md
    ├── process-flow-documentation/SKILL.md
    ├── product-discovery-saas-strategy/SKILL.md
    ├── qa-evidence-based-verification/SKILL.md
    ├── realtime-ai-streaming-lead/SKILL.md
    ├── resume-ats-optimizer/SKILL.md
    ├── skill-creator-installer/SKILL.md
    ├── skills-curator-evaluator/SKILL.md
    ├── social-media-omnichannel-automation/SKILL.md
    ├── software-architecture-diagrammer/SKILL.md
    ├── speech-to-speech-audio-architect/SKILL.md
    ├── technical-documentation-process-governance/SKILL.md
    ├── ui-ux-design-system-architect/SKILL.md
    ├── webmaster-seo-landing-conversion/SKILL.md
    └── windows-audio-native-architect/SKILL.md
```

---

## 📖 Catálogo Maestro Explicado por Fase del Ciclo de Vida

### Fase 0: Mantenimiento de la Verdad, Integridad Epistémica y Ciclo de Vida de Skills

| # | Skill | Archivo | Propósito y Estándares Clave |
| :-: | :--- | :--- | :--- |
| **01** | **Anti-Sycophancy & Direct Technical Truth** | [SKILL.md](./skills/anti-sycophancy-direct-truth/SKILL.md) | **Elimina la adulación y complacencia.** Fuerza análisis *Red-Team / Pre-Mortem* (¿dónde fallará en producción?), separa hechos verificados de hipótesis e impide ceder ante anti-patrones por presión del usuario. |
| **02** | **Integrity & Governance Guard** | [SKILL.md](./skills/integrity-governance-guard/SKILL.md) | **Veedor de veracidad empírica (*Fail-Closed HITL*).** Inspirado en *Constitutional AI* y *NeMo Guardrails*. Prohíbe inventar pruebas o usar parches efímeros, exige backups físicos pre-mutación y solicita confirmación humana ante órdenes ambiguas o destructivas. |
| **03** | **Skill Creator & Installer** | [SKILL.md](./skills/skill-creator-installer/SKILL.md) | **Meta-skill para diseñar e instalar nuevas skills.** Basado en `anthropics/skills/skill-creator`. Define los 3 niveles de *Progressive Disclosure*, la anatomía de 5 bloques, descripciones *anti-undertriggering* y auditoría de seguridad para importar skills de GitHub. |
| **04** | **Skills Curator & Lifecycle Evaluator** | [SKILL.md](./skills/skills-curator-evaluator/SKILL.md) | **Curaduría continua y sincronización *Anti-Drift*.** Audita compuertas deterministas (*Tool-Gates*), frugalidad de tokens y detecta desfases de versión (SHA-256) entre múltiples repositorios. |

---

### Fase 1: Ideación, Investigación Profunda, Estrategia de Producto, Finanzas y Legal

| # | Skill | Archivo | Propósito y Estándares Clave |
| :-: | :--- | :--- | :--- |
| **05** | **Deep Search & Research Lead** | [SKILL.md](./skills/deep-search-research-lead/SKILL.md) | **Investigación profunda multi-hop y búsqueda forense.** Descomposición en 3–5 sub-preguntas ortogonales, rastreo quirúrgico de código sin ruido, *dorking* de RFCs/CVEs/normativas y benchmarking competitivo global. |
| **06** | **Product Discovery & SaaS Strategy Lead** | [SKILL.md](./skills/product-discovery-saas-strategy/SKILL.md) | **De la idea al PRD ejecutable.** Validación de problema (*JTBD*), evaluación *Greenfield vs. Brownfield*, priorización matemática **RICE**, arquitectura de *Feature Flags* y *Self-Service Onboarding* (`Time-to-Value < 5 min`). |
| **07** | **Financial FP&A, Accounting & FinOps Architect** | [SKILL.md](./skills/financial-fpa-roi-architect/SKILL.md) | **Ingeniería financiera, NIIF/IFRS y métricas de negocio.** Prohibición de `float`/`double` para dinero, invariante de partida doble ($\sum \text{Debe} = \sum \text{Haber}$), ratios financieros (Liquidez, EBITDA, ROE DuPont), métricas SaaS (ARR, NRR, LTV/CAC), consolidación Holding (NIIF 10) y FinOps cloud. |
| **08** | **Compliance & Regulatory Auditor** | [SKILL.md](./skills/compliance-regulatory-auditor/SKILL.md) | **Cumplimiento normativo *Compliance-by-Design*.** Controles técnicos para ISO 27001/27701/42001, SOC 2, PCI-DSS v4.0, GDPR, EU AI Act, SOX 404 (4 ojos), NIIF/IFRS y marcos tributarios/laborales de Norteamérica, Centroamérica y Europa. |
| **09** | **Data Privacy Officer (DPO) & IT Legal Lead** | [SKILL.md](./skills/data-privacy-legal-dpo/SKILL.md) | **Privacidad, contratos legales SaaS y evidencia digital.** Consentimiento inmutable, derechos ARCO/portabilidad/olvido con preservación fiscal, cuarteto contractual (ToS, SLA, DPA, Privacidad), legalidad GPS/CCTV laboral y cadena de custodia ISO/IEC 27037. |

---

### Fase 2: Arquitectura de Software, Bases de Datos, Contratos de API y Motores BPM

| # | Skill | Archivo | Propósito y Estándares Clave |
| :-: | :--- | :--- | :--- |
| **10** | **Backend DDD & Clean Architecture Lead** | [SKILL.md](./skills/backend-ddd-clean-architect/SKILL.md) | **Dominio soberano y sistemas distribuidos resilientes.** *Domain-Driven Design* (invariantes puros, cero estado en memoria), aislamiento multi-tenant (`HasQueryFilter`), cuarteto de auditoría, patrón **Transactional Outbox** (*Anti-Dual-Write*) y errores **IETF RFC 7807 (`ProblemDetails`)**. |
| **11** | **PostgreSQL Database & Data Architect** | [SKILL.md](./skills/database-postgresql-architect/SKILL.md) | **Persistencia relacional de alto rendimiento y Zero-Downtime DDL.** Esquemas por *Bounded Context*, scripts SQL idempotentes, índices compuestos multi-tenant, migraciones sin bloqueo (`CREATE INDEX CONCURRENTLY`, *Expand-and-Contract*) y particionamiento declarativo temporal. |
| **12** | **API Contract & Gateway Governance Lead** | [SKILL.md](./skills/api-contract-gateway-governance/SKILL.md) | **Gobernanza de contratos REST/GraphQL e idempotencia.** Evolución aditiva sin rotura, paginación canónica, cabecera `Idempotency-Key` contra pagos/registros duplicados, saneamiento en API Gateway y detección de endpoints huérfanos. |
| **13** | **BPM Workflow, Decision Engine & Saga Architect** | [SKILL.md](./skills/bpm-workflow-saga-architect/SKILL.md) | **Motores de procesos BPMN 2.0, DMN y Sagas distribuidas.** Versionado inmutable de procesos, Segregación de Funciones de 4 ojos (*SoD*) forzada en backend, Sagas con compensación semántica, temporizadores SLA y nodos de IA con umbral *HITL*. |
| **14** | **Software Architecture & Visual Modeler** | [SKILL.md](./skills/software-architecture-diagrammer/SKILL.md) | **Modelado visual formal ISO/IEC/IEEE 42010.** Paleta semántica por capas y plantillas Mermaid para **C4 Model** (Contenedores), **IDEF0** (ICOM), **ERD Crow's Foot**, **UML Secuencia/Estados** y **DFD**. |

---

### Fase 3: Creación de Web, Landing Pages, Apps Móviles, Escritorio Nativo, IA y Voz

| # | Skill | Archivo | Propósito y Estándares Clave |
| :-: | :--- | :--- | :--- |
| **15** | **UI/UX & Enterprise Design System Architect** | [SKILL.md](./skills/ui-ux-design-system-architect/SKILL.md) | **Sistemas de diseño ejecutivos, accesibilidad e i18n.** Tokens semánticos CSS (Dark/Light), los **5 estados obligatorios de toda vista** (*Loading, Empty, Ideal, Mutating, Error*), accesibilidad WCAG 2.2 AA, prohibición de `alert()`/`confirm()` y cero textos quemados. |
| **16** | **High-Impact Landing Page Design Lead** | [SKILL.md](./skills/landing-page-design/SKILL.md) | **Diseño visual de páginas de producto de alto impacto.** Estructuras *Bento Grid* asimétricas, sistema tipográfico `Outfit` + `Inter`, contraste accesible y ergonomía multi-zoom (`100%` a `200%`) sin desbordamientos. |
| **17** | **Webmaster, Technical SEO & Conversion Architect** | [SKILL.md](./skills/webmaster-seo-landing-conversion/SKILL.md) | **Arquitectura web pública, SEO técnico y conversión (CRO).** Estructura de 7 secciones de persuasión, Core Web Vitals (`LCP < 1.8s`, `CLS = 0`), metadatos OpenGraph, JSON-LD `Schema.org`, `sitemap.xml` y formularios asíncronos + WhatsApp directo. |
| **18** | **Mobile Cross-Platform & Offline-First Architect** | [SKILL.md](./skills/mobile-cross-platform-architect/SKILL.md) | **Desarrollo de apps móviles iOS y Android (React Native / Flutter).** Sincronización *Offline-First* con base local SQLite y cola de mutaciones idempotente, almacenamiento en Keychain/Keystore (OWASP MASVS), biometría, notificaciones Push y reglas de aprobación en App Store y Google Play. |
| **19** | **Cross-Platform Desktop Packaging Architect** | [SKILL.md](./skills/cross-platform-desktop-packager/SKILL.md) | **Empaquetado nativo de escritorio (Tauri 2.0 / Electron / Rust).** Generación determinista de instaladores para Windows (`.exe` NSIS / `.msi` WiX), Linux (`.deb` / `.AppImage`) y macOS (`.dmg`) con bajo consumo de RAM y verificación SHA-256. |
| **20** | **AI, Hybrid RAG, MCP & Agentic Systems Architect** | [SKILL.md](./skills/ai-llm-rag-agentic-architect/SKILL.md) | **Ingeniería de IA en producción.** RAG híbrido aislado por inquilino (`pgvector` + búsqueda léxica BM25 + *Cross-Encoder Reranker*), construcción de servidores **MCP**, salidas estructuradas JSON Schema y defensa contra *Indirect Prompt Injection*. |
| **21** | **Real-Time AI Streaming & Multi-Model Router** | [SKILL.md](./skills/realtime-ai-streaming-lead/SKILL.md) | **Inferencia en tiempo real (`< 600 ms` E2E) con SSE y WebSockets.** Enrutador multimodelo con fallback automático entre Groq LPU, Gemini Flash/Live, OpenAI y ejecución local offline con Ollama, resguardando API Keys en bóvedas cifradas. |
| **22** | **Speech-to-Speech & Anti-Echo Audio Architect** | [SKILL.md](./skills/speech-to-speech-audio-architect/SKILL.md) | **Traducción y conversación de voz a voz en tiempo real.** Topología de doble vía con aislamiento acústico total (*Zero-Feedback*), cancelación de eco WebRTC AEC3 e inyección hacia micrófonos virtuales para Meet, Zoom y Teams. |
| **23** | **Windows Core Audio & WASAPI Native Architect** | [SKILL.md](./skills/windows-audio-native-architect/SKILL.md) | **Captura acústica nativa en Windows 10/11.** Captura `WASAPI Loopback` sin drivers externos, conversión en memoria a PCM 16 kHz mono, detección de actividad de voz (VAD) y reconexión en caliente ante cambio de auriculares. |
| **24** | **Linux Audio Native Architect (PipeWire / PulseAudio)** | [SKILL.md](./skills/linux-audio-native-architect/SKILL.md) | **Captura acústica nativa en distribuciones Linux.** Conexión sin privilegios `root` a nodos `.monitor` de PipeWire y PulseAudio con resampling de ultra-baja latencia y bajo consumo de CPU. |

---

### Fase 4: Pagos Fintech, Logística WMS/TMS, Flotas IoT y Auditoría Ejecutiva

| # | Skill | Archivo | Propósito y Estándares Clave |
| :-: | :--- | :--- | :--- |
| **25** | **Fintech Payments, Subscriptions & Billing Architect** | [SKILL.md](./skills/fintech-payments-billing-architect/SKILL.md) | **Pasarelas de pago, suscripciones recurrentes y libro mayor.** Cumplimiento PCI-DSS SAQ-A (tokenización 3DS2), activación exclusiva por Webhooks firmados con HMAC-SHA256, idempotencia contra doble cobro, *Smart Dunning* y libro mayor *Append-Only*. |
| **26** | **Logistics, WMS & Supply Chain Systems Architect** | [SKILL.md](./skills/logistics-distribution-systems/SKILL.md) | **Almacenes (WMS), inventarios y distribución (TMS).** Reserva atómica contra saldos negativos, Kardex inmutable con costeo NIC 2, rotación por lotes/vencimientos (**FEFO/FIFO**), *Wave Picking* con Handhelds y prueba electrónica de entrega (*ePOD*). |
| **27** | **Fleet Telematics, CAN-Bus & IoT Logistics Architect** | [SKILL.md](./skills/fleet-telematics-iot-audit/SKILL.md) | **Telemetría vehicular GPS, cadena de frío y control de combustible.** Seguridad M2M en APN privado sobre MQTTS, monitoreo inmutable de temperatura en furgones refrigerados, lectura de computadora de a bordo (CAN Bus J1939) y detección de drenaje de combustible. |
| **28** | **Physical Security, CCTV/VMS & Access Control Auditor** | [SKILL.md](./skills/cctv-physical-security-audit/SKILL.md) | **Videovigilancia IP, control biométrico y redes PoE.** Cálculo de retención en arreglos RAID, aislamiento en VLAN CCTV dedicada sin salida a Internet, hardening contra botnets IoT, respaldo eléctrico UPS y cadena de custodia forense con hash SHA-256. |
| **29** | **IT Governance & Forensic Audit Lead** | [SKILL.md](./skills/it-governance-forensic-audit/SKILL.md) | **Auditoría técnica forense en Modo Solo Lectura.** Basado en COBIT 2019, ITIL v4 e ISO 22301 (BCP/DRP). Diagnóstico sin mutar código productivo estructurado en 3 partes obligatorias: *1. ¿Qué pasa?*, *2. ¿Por qué pasa?* y *3. ¿Qué hacer?*. |
| **30** | **Executive Board & C-Level Reporting Lead** | [SKILL.md](./skills/executive-board-reporting/SKILL.md) | **Informes para Junta Directiva, Accionistas y CFO.** Principio de la Pirámide de Minto (*BLUF*), traducción de riesgos técnicos a impacto patrimonial en dólares, mapas de calor de riesgos, análisis CAPEX/OPEX y plan de acción 30/60/90 días. |

---

### Fase 5: Calidad Empírica (QA), Automatización E2E, Ciberseguridad y Auto-Curación SRE

| # | Skill | Archivo | Propósito y Estándares Clave |
| :-: | :--- | :--- | :--- |
| **31** | **QA Evidence-Based Verification Lead** | [SKILL.md](./skills/qa-evidence-based-verification/SKILL.md) | **Verificación en caliente (*In-Flight QA*) y Tolerancia Cero a Mocks.** Segregación de funciones (quien codifica no auto-aprueba su entrega), prueba en las 4 capas (SQL, HTTP `curl`, UI con recarga `F5` y bitácora de auditoría), prueba de idempotencia y Bloque de Evidencia Cruda. |
| **32** | **E2E WebApp Automation & Load Testing Lead** | [SKILL.md](./skills/e2e-webapp-load-testing-lead/SKILL.md) | **Suites E2E en navegador real (Playwright) y pruebas de estrés (k6).** Cero `sleep()` arbitrarios, fallo automático ante cualquier `pageerror` de consola o `HTTP 500`, pruebas de concurrencia contra condiciones de carrera y validación de latencia P95/P99. |
| **33** | **Cybersecurity Zero-Trust & Defensive Architect** | [SKILL.md](./skills/cybersecurity-zero-trust-defensive/SKILL.md) | **Seguridad Zero-Trust y OWASP ASVS v4 Nivel 3.** Firma asimétrica RSA-RS256/JWKS, bloqueo de *Algorithm Confusion Attack*, defensa contra *Tenant Leakage* (Anti-IDOR/BOLA), cookies `HttpOnly`, sanitización de PII en logs y *Rate Limiting* por inquilino. |
| **34** | **Observability, SRE & Self-Healing Systems Architect** | [SKILL.md](./skills/observability-sre-self-healing/SKILL.md) | **Operación autónoma 24/7 y auto-recuperación (*Self-Healing*).** Telemetría OpenTelemetry (Las 4 Señales Doradas), auto-reinicio por `healthcheck`, *Circuit Breakers*, rotación automática de logs/disco, renovación desatendida de certificados SSL y alertas de alta señal. |

---

### Fase 6: Documentación Viva (Docs-as-Code), Procesos de Negocio y Videos Demo

| # | Skill | Archivo | Propósito y Estándares Clave |
| :-: | :--- | :--- | :--- |
| **35** | **Technical Documentation & Process Governance Lead** | [SKILL.md](./skills/technical-documentation-process-governance/SKILL.md) | **Gobernanza *Docs-as-Code*, co-autoría técnica y ADRs.** Sincronización obligatoria entre código, catálogo de APIs, diccionario de datos y manuales multilingües, plantillas **ADR** y *Prueba del Lector Frío*. |
| **36** | **Process Flow Governance & Business Modeling Lead** | [SKILL.md](./skills/process-flow-documentation/SKILL.md) | **Modelado formal de procesos (APQC N0–N3 + ISO 9001 §4.4).** Todo formulario o flujo transaccional se documenta con su ficha **SIPOC**, modelo funcional **IDEF0 (ICOM)**, diagrama de carriles Mermaid, tabla técnica paso a paso, matriz **RACI** y máquina de estados. |
| **37** | **Automated Demo Video & Training Producer** | [SKILL.md](./skills/automated-demo-video-training-producer/SKILL.md) | **Grabación automatizada de videos demostrativos 1080p (`.mp4`).** Captura la interfaz web real en ejecución con Playwright interactuando paso a paso, sincronizada con locución neural (`edge-tts`) y subtítulos masterizados con FFmpeg (`H.264 yuv420p`). |

---

### Fase 7: Publicación Cloud Inmutable, DevOps y Mantenimiento Evolutivo

| # | Skill | Archivo | Propósito y Estándares Clave |
| :-: | :--- | :--- | :--- |
| **38** | **DevOps & Immutable Cloud Infrastructure Lead** | [SKILL.md](./skills/devops-immutable-cloud-infrastructure/SKILL.md) | **Orquestación Docker idempotente y despliegues sin caída.** Mapeo seguro de volúmenes (`/var/lib/postgresql/data` y llaves de sesión), resolución DNS dinámica en Nginx (`resolver 127.0.0.11` con variables `$upstream`), protección de certificados Let's Encrypt y *Smoke Tests* bloqueantes. |
| **39** | **Code Simplifier & Software Maintenance Architect** | [SKILL.md](./skills/code-simplifier-refactoring-maintenance/SKILL.md) | **Mantenimiento evolutivo sin regresión (*Primum Non Nocere*).** Protocolo de refactorización en 4 fases (Caracterización, Intervención Quirúrgica, Verificación y Poda), cláusulas de guarda (*Early Return*), regla de Sandi Metz y purga física de archivos huérfanos (*Zero-Orphan-Files*). |

---

### Fase 8 & 9: Publicidad Multimedia, Automatización de Redes Sociales, Cron/Swarm y Talento

| # | Skill | Archivo | Propósito y Estándares Clave |
| :-: | :--- | :--- | :--- |
| **40** | **Advertising, Copywriting & Multimedia Production Lead** | [SKILL.md](./skills/advertising-marketing-lead/SKILL.md) | **Campañas publicitarias B2B/B2C y producción audiovisual determinista.** Copywriting de respuesta directa (Eugene Schwartz, PAS, AIDA, BAB, protocolo *Anti-AI-Slop*), fidelidad estricta al código y generación automatizada de Flyers (16:9 y 9:16), tarjetas ejecutivas y spots de video con FFmpeg + PIL + locución neural. |
| **41** | **Social Media Omnichannel & Growth Automation Lead** | [SKILL.md](./skills/social-media-omnichannel-automation/SKILL.md) | **Automatización integral de Redes Sociales y conversión de leads.** Multiplicación `1 activo ➔ 7 canales` (LinkedIn, Instagram Reels, TikTok, YouTube Shorts, X, Facebook, WhatsApp Business API y Telegram), cola idempotente de publicación vía APIs OAuth2, enlaces UTM y bots de respuesta inmediata (< 30s) hacia el CRM. |
| **42** | **Autonomous Workflow, Cron & Multi-Agent Swarm Orchestrator** | [SKILL.md](./skills/autonomous-workflow-cron-swarm/SKILL.md) | **Piloto automático 24/7 y coordinación de enjambres multi-agente.** Tareas Cron autocontenidas con zona horaria IANA (`CRON_TZ`), bloqueos contra solapamiento (`flock` / advisory locks), las 5 rutinas autónomas esenciales (salud, backups, conciliación, digest matutino y auditoría) y trabajo en paralelo con subagentes (*Swarm*). |
| **43** | **Job Hunter & B2B Opportunity Scout** | [SKILL.md](./skills/job-hunter-scout/SKILL.md) | **Inteligencia de oportunidades y contratos remotos B2B.** Filtrado de vacantes/pliegos, cálculo ponderado de afinidad técnica (*Fit Score 0–100%*) y redacción de propuestas directas en 3 párrafos orientadas a resultados. |
| **44** | **Resume & ATS Engineering Optimizer** | [SKILL.md](./skills/resume-ats-optimizer/SKILL.md) | **Optimización algorítmica de CV para filtros ATS.** Redacción de logros bajo la **Fórmula Google XYZ** (*Logré X, medido por Y, mediante Z*), estructura lineal limpia para Workday/Greenhouse y exportación PDF verificable. |
| **45** | **LinkedIn Profile & Executive Branding Optimizer** | [SKILL.md](./skills/linkedin-profile-optimizer/SKILL.md) | **Posicionamiento SEO en LinkedIn Recruiter y marca personal.** Ingeniería de titular (`≤ 220 caracteres`), narrativa de autoridad en "Acerca de", asociación de aptitudes verificadas por cargo y portafolio destacado. |

---

## 🚀 Cómo Instalar o Usar este Compendio en Cualquier Agente de IA

### Opción 1: Google Antigravity / Gemini CLI / Cursor
Copia la carpeta `skills/` dentro de `.agents/` en tu proyecto (o globalmente en `~/.gemini/antigravity/skills/`):

```bash
mkdir -p /ruta/a/tu-proyecto/.agents/
cp -r skills /ruta/a/tu-proyecto/.agents/
```

### Opción 2: Claude Code
```bash
mkdir -p /ruta/a/tu-proyecto/.claude/
cp -r skills /ruta/a/tu-proyecto/.claude/
```

### Opción 3: GitHub Copilot (VS Code Agent Mode)
```bash
mkdir -p /ruta/a/tu-proyecto/.github/
cp -r skills /ruta/a/tu-proyecto/.github/
```

### Opción 4: Clonar directamente o instalar mediante `skills.sh`
Puedes clonar este repositorio o instalar directamente sus skills mediante el CLI del estándar abierto:
```bash
git clone https://github.com/SIASA-NI/AI-SIASANI-Skills.git
# O mediante skills.sh CLI:
npx skills add SIASA-NI/AI-SIASANI-Skills
```
