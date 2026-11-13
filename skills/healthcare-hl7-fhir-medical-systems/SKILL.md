---
name: healthcare-hl7-fhir-medical-systems
description: Arquitectura de software médico y hospitalario (EHR/HIS), interoperabilidad con estándares HL7 (v2.x pipe-delimited sobre MLLP/TCP) y FHIR (R4/R5 REST JSON), imágenes médicas DICOM / PACS (DICOMweb WADO-RS), seguridad HIPAA, trazabilidad de receta electrónica y dispositivos clínicos.
---

# 🏥 Healthcare Software, HL7, FHIR & Medical Devices Architect

## 1. Propósito y Alcance
Este skill dota al agente de la especialidad como **Principal Healthcare Systems Architect & Biomedical Interoperability Engineer**, dominando el diseño y desarrollo de software para hospitales, clínicas, laboratorios y telemedicina bajo los estándares internacionales **HL7 v2.x**, **HL7 FHIR (R4/R5)**, **DICOM** y regulaciones de confidencialidad médica (**HIPAA / GDPR Health Data**).

---

## 2. Los 3 Estándares Cardinales de la Informática Médica

```mermaid
flowchart TD
    subgraph Hospital ["Red Clínica Hospitalaria / Dispositivos"]
        Admit["Admisión / Citas (HIS)"] -->|HL7 v2 ADT (MLLP/TCP)| Engine["HL7 Integration Engine"]
        Order["Orden Médica (Lab / Rayos X)"] -->|HL7 v2 ORM| Engine
        Scanner["Equipos Médicos (Tomógrafo / Resonancia / Ecógrafo)"] -->|DICOM C-STORE| PACS["Servidor PACS / DICOMweb"]
    end

    subgraph ModernCloud ["Plataforma Cloud / Portal Paciente / App Médica"]
        Engine -->|Transformación Semántica| FHIR["FHIR Server R4/R5 (REST JSON)"]
        PACS -->|DICOMweb WADO-RS / QIDO-RS| Viewer["Visor Médico Web Zero-Footprint"]
        FHIR --> WebApp["Expediente Clínico Electrónico (EHR)"]
        Viewer --> WebApp
    end
```

---

## 3. Mensajería HL7 v2.x (El Estándar Hospitalario Legado)
1. **Protocolo de Transporte MLLP (Minimal Lower Layer Protocol sobre TCP/IP):**
   - Envoltura de tramas obligatoria: byte de inicio `0x0B` (`<VT>`), payload de texto delimitado por pipes (`|`), bytes de fin `0x1C 0x0D` (`<FS><CR>`).
2. **Tipos de Mensajes Fundamentales:**
   - **`ADT` (Admission, Discharge, Transfer):** `ADT^A01` (ingreso de paciente), `ADT^A08` (actualización de datos demográficos), `ADT^A03` (alta médica).
   - **`ORM` (Order Message):** Solicitudes de laboratorio, patología o estudios de imagen.
   - **`ORU` (Observation Result):** Resultados de análisis clínicos (`ORU^R01`) con rangos de referencia y valores críticos.
   - **`ACK` (Acknowledgment):** Respuesta de confirmación obligatoria con código `AA` (Application Accept) o `AE/AR` (Application Error/Reject).

---

## 4. HL7 FHIR (Fast Healthcare Interoperability Resources — R4/R5)
1. **Arquitectura Orientada a Recursos (RESTful JSON/XML):**
   - Recursos esenciales: `Patient`, `Practitioner`, `Encounter`, `Condition`, `Observation` (signos vitales con códigos LOINC), `MedicationRequest` (recetas electrónicas con RxNorm), `DiagnosticReport` e `ImagingStudy`.
2. **Seguridad y Extensiones SMART-on-FHIR:**
   - Autenticación OAuth 2.0 con perfiles SMART, control de acceso basado en roles (*Scopes*: `patient/Patient.read`, `user/Observation.write`) y cifrado obligatorio de datos en tránsito (`TLS 1.3`) y en reposo (`AES-256`).
3. **Identificadores Médicos Universales:**
   - Estandarización obligatoria de diagnósticos con **CIE-10 / CIE-11 / SNOMED CT**, exámenes de laboratorio con **LOINC** y medicamentos con **ATC / RxNorm**.

---

## 5. Imágenes Médicas DICOM & PACS (DICOMweb)
1. **Jerarquía DICOM:**
   - `Patient ➔ Study (Estudio global) ➔ Series (Secuencia de cortes) ➔ SOP Instance (Corte/Imagen individual)`.
2. **DICOMweb (APIs REST para Visores Web Médicos):**
   - **`WADO-RS` (Web Access to DICOM Objects):** Recuperación de imágenes individuales o series completas en formatos nativos o renderizados (`image/jls`, `image/jp2`, `image/jpeg`).
   - **`QIDO-RS` (Query based on ID for DICOM Objects):** Búsqueda de estudios por ID de paciente, modalidad (`CT, MR, US, CR, DX`) o fecha.
   - **`STOW-RS` (Store Over the Web):** Subida de nuevos estudios desde digitalizadores y sensores.
3. **Visores Web Médicos Zero-Footprint (CornerstoneJS / OHIF Viewer):**
   - Soporte de manipulaciones diagnósticas: ajuste de brillo/contraste (*Window Level / Window Width* en Unidades Hounsfield HU), zoom sub-píxel, mediciones de distancia y ángulos de Cobb.

---

## 6. Seguridad y Confidencialidad de Datos de Salud (HIPAA / GDPR)
- **Trazabilidad y Bitácora de Acceso a Fichas Clínicas (Audit Event):**
  - Todo acceso a la ficha de un paciente debe registrar quién consultó, a qué hora, desde qué IP y bajo qué justificativo asistencial.
- **Anonimización Forense para Investigación:** Eliminación estricta de los 18 identificadores directos e indirectos HIPAA (nombres, fechas exactas, números de seguridad social, teléfonos, biometría y metadatos DICOM privados) antes de procesar con modelos de IA.
