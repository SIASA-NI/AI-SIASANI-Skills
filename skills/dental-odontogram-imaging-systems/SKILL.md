---
name: dental-odontogram-imaging-systems
description: Arquitectura de software odontológico y dental (EHR Dental), odontograma interactivo multi-nomenclatura (FDI ISO 3950, Universal ADA, Palmer), periodontograma con sondeo 6 puntos, integración con sensores de rayos X intraorales/panorámicos (TWAIN, DICOM dental) y planes de tratamiento.
---

# 🦷 Dental Software, Interactive Odontogram & Imaging Systems Architect

## 1. Propósito y Alcance
Este skill capacita al agente como **Arquitecto Especialista en Software Odontológico, Clínicas Dentales y Sistemas de Imagenología Oral**, dominando la construcción de odontogramas vectoriales interactivos, periodontogramas clínicos de alta precisión, captura de radiografías digitales (sensores intraorales, ortopantomografías y tomografía CBCT) y planes de tratamiento presupuestados.

---

## 2. Nomenclatura Dental Internacional y Odontograma Vectorial

El sistema debe permitir alternar en caliente entre las tres nomenclaturas odontológicas estándar del mundo:

```text
1. Sistema FDI / Internacional (ISO 3950) — Estándar Global:
   - Cuadrante 1: Superior Derecho (18 al 11)  | Cuadrante 2: Superior Izquierdo (21 al 28)
   - Cuadrante 4: Inferior Derecho (48 al 41)  | Cuadrante 3: Inferior Izquierdo (31 al 38)
   - Dentición Temporal: Cuadrantes 5, 6, 7 y 8 (55 al 85)

2. Sistema Universal / Numérico (ADA — Estándar Estados Unidos):
   - Dientes Permanentes: 1 al 32 (iniciando en molar superior derecho #1 hasta molar inferior derecho #32)
   - Dientes Temporales: Letras A a la T

3. Notación Palmer (Ortodoncia y Reino Unido):
   - Notación por cuadrantes con símbolos de ángulo ┘ └ ┐ ┌ (1 al 8 permanentes, A a E temporales)
```

### 2.1. Anatomía Vectorial por Superficies Dentales (SVG)
Cada diente se modela como un grupo SVG interactivo con sus 5 superficies geométricas anatómicas independientes:
- **Oclusal / Incisal (Centro):** Superficie de masticación o corte.
- **Vestibular / Bucal (Exterior):** Cara orientada a los labios o mejillas.
- **Lingual / Palatina (Interior):** Cara orientada a la lengua o paladar.
- **Mesial (Lateral):** Cara proximal orientada hacia la línea media dental.
- **Distal (Lateral):** Cara proximal orientada hacia el fondo de la arcada.

### 2.2. Convención Cromática y Simbólica de Patologías y Tratamientos
- **Color Rojo:** Patología activa / Tratamiento requerido o pendiente (Caries activa, fractura dental, cálculo gingival).
- **Color Azul:** Tratamiento existente y en buen estado (Resina compuesta, amalgama, corona de porcelana, endodoncia obturada).
- **Simbología Estandarizada:**
  - **Diente Ausente / Exodoncia:** Aspa (*X*) azul o gris sobre la corona y raíz.
  - **Prótesis Fija / Puente:** Línea de unión horizontal continua entre los pilares preparados.
  - **Implante Dental:** Tornillo endoóseo representado en la zona radicular.
  - **Aparato de Ortodoncia:** Línea continua segmentada con brackets rectangulares por arcada.

---

## 3. Periodontograma Clínico (Sondeo Periodontal de 6 Puntos)
1. **Profundidad de Sondaje (PS) y Nivel de Inserción Clínica (NIC):**
   - Registro de 6 puntos por diente: **Mesio-Vestibular, Centro-Vestibular, Disto-Vestibular, Mesio-Lingual, Centro-Lingual y Disto-Lingual**.
   - Alerta visual automática: Profundidades $\ge 4\text{ mm}$ (bolsa periodontal activa) se resaltan en rojo brillante.
2. **Indicadores Clínicos de Acompañamiento:**
   - **Sangrado al Sondaje (BOP — Bleeding on Probing):** Punto rojo activo.
   - **Supuración (PUS):** Indicador de infección activa.
   - **Compromiso de Furca (Grados I, II, III):** Triángulo en piezas multirradiculares.
   - **Movilidad Dental (Grados 0 a 3):** Clasificación de Miller.

---

## 4. Imagenología Dental, Sensores de Rayos X y TWAIN/DICOM
1. **Captura Directa desde Sensores Intraorales y Panorámicos:**
   - Comunicación nativa vía driver **TWAIN / WIA** o puente directo USB con sensores de fósforo (PSP) y sensores CMOS/CCD (DEXIS, Carestream, Schick, Vatech, Planmeca).
2. **Cumplimiento DICOM Dental:**
   - Exportación de series periapicales y bitewings en formato DICOM conforme a las directrices de la ADA y la AAOMR.
   - Preservación de la relación dimensional (1:1) y calibración espacial en milímetros para planificación de implantes dentales y mediciones endodónticas seguras.

---

## 5. Planes de Tratamiento y Presupuestos Dinámicos
- Vinculación biunívoca entre el odontograma interactivo y el catálogo arancelario de la clínica:
  - Al marcar una caries en superficie oclusal y seleccionar "Restauración con Resina", el sistema genera automáticamente la línea del presupuesto con su código de procedimiento ADA / CDT, costo del arancel, cobertura del seguro dental y saldo del paciente.
