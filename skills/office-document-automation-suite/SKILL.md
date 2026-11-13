---
name: office-document-automation-suite
description: Generación, lectura, auditoría y transformación programática determinista de documentos corporativos PDF, Word (.docx), Excel (.xlsx) y PowerPoint (.pptx) con diseño ejecutivo, fórmulas dinámicas verificadas, tablas sin desbordamiento y OCR.
---

# 📄 Enterprise Office Document Automation Suite (PDF, DOCX, XLSX & PPTX)

## 1. Propósito y Alcance
Este skill dota al agente de las capacidades de **Ingeniería Documental Corporativa Automatizada** (inspirado en la suite oficial `pdf`, `docx`, `xlsx` y `pptx` de `anthropics/skills`), permitiendo crear, analizar, auditar y transformar entregables de oficina listos para Junta Directiva, reguladores o clientes sin depender de intervención manual.

---

## 2. Estándares por Formato Documental

### 2.1. Hojas de Cálculo Financieras y Operativas (`.xlsx` — `openpyxl` / `xlsxwriter`)
- **Prohibición de Totales Hardcodeados:** Toda celda de subtotal, total, margen, KPI, VAN, TIR o variación porcentual **DEBE escribirse como fórmula nativa de Excel** (`=SUM(C5:C42)`, `=INDEX(..., MATCH(...))`, `=IFERROR(..., 0)`), jamás como un número estático precalculado en Python.
- **Convención Internacional de Colores Financieros (Wall Street Standard):**
  - **Texto Azul (`#0000FF`):** Inputs hardcodeados / supuestos del modelo.
  - **Texto Negro (`#000000`):** Fórmulas dinámicas dentro de la misma hoja.
  - **Texto Verde (`#008000`):** Vínculos hacia otras hojas del libro.
- **Formato Numérico y Auto-Ajuste de Columnas:** Aplicar formatos explícitos (`$#,##0.00;($#,##0.00);"-"`, `0.0%`, `yyyy-mm-dd`), congelar paneles superiores (`freeze_panes = "A2"`) y calcular el ancho de cada columna según la longitud máxima de su contenido + margen para evitar `###`.
- **Verificación de Cero Errores de Fórmula:** Auditar que ninguna celda contenga `#REF!`, `#DIV/0!`, `#VALUE!`, `#NAME?` o `#N/A`.

### 2.2. Documentos Corporativos y Contratos (`.docx` — `python-docx` / `docx-js`)
- **Jerarquía Tipográfica y Estilos Reales:** Utilizar estilos nativos de Word (`Heading 1`, `Heading 2`, `Normal`, `List Bullet`) con fuentes corporativas limpias (`Inter`, `Calibri`, `Arial` o `Georgia`), interlineado `1.15x` y espaciado posterior definido, nunca saltos de línea vacíos repetidos.
- **Tablas Ejecutivas con Repetición de Encabezado:** Configurar encabezados de tabla con fondo institucional oscuro, texto blanco en negrita, bordes sutiles (`#CBD5E1`) y propiedad `cantSplit` / `tblHeader` para que el encabezado se repita automáticamente al cruzar de página.
- **Encabezados, Pies de Página y Numeración:** Incluir membrete institucional, clasificación de confidencialidad y numeración dinámica (`Página X de Y`).

### 2.3. Reportes Oficiales y Dictámenes en PDF (`.pdf` — `Playwright PDF` / `ReportLab` / `WeasyPrint` / `pypdf`)
- **Generación de Alta Fidelidad (HTML/CSS `@page` ➔ Chromium PDF o ReportLab):**
  - Definir reglas CSS `@page { size: A4; margin: 18mm; }` y `break-inside: avoid;` en tarjetas, tablas, gráficos y bloques de firma para **prohibir cortes a mitad de tabla o títulos huérfanos** al final de una página.
- **Extracción, Fusión y OCR:** Usar `pypdf` / `pdfplumber` para extracción tabular exacta y `pytesseract` / `ocrmypdf` cuando el PDF de entrada sea una imagen escaneada sin capa de texto.

### 2.4. Presentaciones Ejecutivas (`.pptx` — `python-pptx`)
- **Formato Widescreen 16:9 (`13.333 x 7.5 pulgadas`):**
- **Regla de Densidad Visual:** Máximo 1 idea central por diapositiva, titular de acción (*Action Title* que resume la conclusión, ej. *"El margen EBITDA creció 14.2% impulsado por automatización"* en lugar de *"Resultados Q3"*), grilla de hasta 3–4 tarjetas KPI o gráfico limpio sin superposición de textos.

---

## 3. Compuerta de Verificación Visual Obligatoria
Antes de entregar cualquier archivo `.pdf`, `.docx`, `.xlsx` o `.pptx`:
1. Convertir o renderizar las páginas a imagen (usando `pdftoppm -png -r 150` o LibreOffice `--headless --convert-to pdf`) e inspeccionar que **no existan textos truncados, columnas superpuestas ni páginas en blanco accidentales**.

