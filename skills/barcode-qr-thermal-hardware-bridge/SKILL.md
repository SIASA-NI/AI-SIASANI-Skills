---
name: barcode-qr-thermal-hardware-bridge
description: Conexión e integración de hardware periférico comercial e industrial (lectores de código de barras 1D/2D, lectores y generadores de QR, básculas y balanzas, terminales POS, impresoras térmicas ESC/POS, puertos COM/Serie RS-232, WebSerial, WebHID y WebUSB).
---

# 📟 Barcode, QR, Thermal Printer & Hardware Peripherals Bridge Lead

## 1. Propósito y Alcance
Este skill convierte al agente en un **Especialista Principal en Integración de Hardware Periférico, Dispositivos POS y Trazabilidad Comercial/Industrial**, capaz de conectar aplicaciones web, de escritorio y backend a lectores de códigos de barras (1D/2D), generadores y decodificadores de códigos QR de alta densidad, impresoras térmicas de tickets y etiquetas (ESC/POS, ZPL, TSPL), balanzas y puertos serie.

---

## 2. Lectores de Códigos de Barras y QR (Modos de Integración)

### 2.1. Modo Emulación de Teclado (*Keyboard Wedge Mode*)
- **Detección Determinista de Ráfaga de Caracteres:** Un lector de código de barras emite caracteres a una velocidad sobrehumana (intervalo `< 25 ms` entre teclas) finalizando con `Enter` (`\r` o `\n`).
- **Buffer de Captura Global sin Foco Dependiente:**
  ```typescript
  // Buffer global que captura lecturas sin requerir que el cursor esté enfocado en un input específico
  let barcodeBuffer = "";
  let lastKeyTime = Date.now();

  window.addEventListener("keydown", (e) => {
    const currentTime = Date.now();
    if (currentTime - lastKeyTime > 50) {
      barcodeBuffer = ""; // Reset si pasaron más de 50ms (escritura humana manual)
    }
    lastKeyTime = currentTime;

    if (e.key === "Enter") {
      if (barcodeBuffer.length >= 4) {
        e.preventDefault();
        dispatchBarcodeEvent(barcodeBuffer);
        barcodeBuffer = "";
      }
    } else if (e.key.length === 1) {
      barcodeBuffer += e.key;
    }
  });
  ```

### 2.2. Modo Nativo Directo: WebHID y WebSerial (`navigator.hid` / `navigator.serial`)
- Para lectores industriales (Honeywell, Zebra, Datalogic) configurados en modo USB-HID o COM Virtual (RS-232):
  - **Cero interferencia con el teclado:** La lectura no escribe caracteres en la pantalla ni interrumpe la navegación del usuario.
  - Recepción de tramas crudas completas en un solo evento `inputreport` con prefijos y sufijos de control (AIM Symbology Identifiers como `]C1` para GS1-128 o `]Q1` para QR Code).

---

## 3. Generación y Decodificación de Códigos QR y 1D (GS1 / EAN / DataMatrix)
1. **Estándares Soportados:**
   - **1D:** EAN-13, EAN-8, UPC-A, Code 128 (alfanumérico de alta densidad), Interleaved 2 of 5 (cajas logísticas).
   - **2D:** QR Code (Modelo 2 con corrección de errores L/M/Q/H), Micro QR, DataMatrix (estándar farmacéutico e industrial aeroespacial).
   - **GS1 Digital Link y Códigos Compuestos:** Estructuración canónica con Identificadores de Aplicación `(01) GTIN + (10) Lote + (17) Vencimiento + (21) Serie`.
2. **Generación Determinista en Vectorial SVG/Canvas:**
   - Prohibido renderizar códigos como imágenes borrosas. Generar matrices SVG puras o bitmaps con escalado entero exacto para garantizar una legibilidad óptica del 100% bajo cualquier escáner o cámara de teléfono.
3. **Escaneo Óptico en Vivo mediante Cámara Web/Móvil:**
   - Integración de streams de video en tiempo real (`MediaStream` 60 FPS) procesados con `BarcodeDetector` nativo del navegador o WebAssembly (`ZXing / Quagga2`) con soporte de auto-enfoque y linterna (*Torch API*).

---

## 4. Impresión Térmica de Recibos y Etiquetas (ESC/POS & ZPL)
1. **Protocolo ESC/POS (Epson / Star / Genéricas POS 58mm y 80mm):**
   - Transmisión directa de bytes de control: inicialización (`\x1B@`), alineación (`\x1Ba1`), texto enriquecido (negrita, doble altura), impresión nativa de QR hardware (`GS ( k`) y corte de papel parcial/total (`GS V 66 0`).
   - Apertura de cajón de dinero (*Cash Drawer*) mediante pulso de solenoide (`\x1Bp\x00\x19\xFA`).
2. **Impresoras de Etiquetas Industriales (ZPL II / Zebra & TSPL / TSC):**
   - Generación de scripts declarativos de coordenadas milimétricas para etiquetas de inventario, pacientes, muestras de laboratorio o encomiendas con códigos de barras nítidos a 203 DPI / 300 DPI.
3. **Balanzas y Básculas de Peso (Protocolo Toledo / CAS / Dibal vía RS-232):**
   - Lectura continua de peso bruto, tara y peso neto mediante comandos de sondeo `W\r\n` o flujo continuo con validación de peso estable (*Stable Weight Flag*).
