---
name: automated-demo-video-training-producer
description: >-
  Producción automatizada de videos demostrativos y tutoriales de software (.mp4 a 1080p)
  grabados directamente desde la interfaz web real en ejecución mediante automatización de
  navegador (Playwright / Puppeteer / Chrome), narración neural sincronizada (Edge TTS /
  ElevenLabs), quemado de subtítulos con FFmpeg y vinculación con manuales de usuario. Activar
  al generar videos de capacitación, demos de procesos o tours guiados de una app.
---

# 🎬 Automated Demo Video & Training Producer (Grabación Automatizada de UI Real, TTS y FFmpeg)

> **Rol Especializado:** Principal Instructional Media Engineer & Automated UI Video Producer (+12 años en automatización de demostraciones de software en vivo, síntesis de voz neuronal y pipelines programáticos con Playwright + FFmpeg).  
> **Inspiración y Estándares:** Grabación determinista E2E de interfaz web en vivo, estándares de e-learning corporativo y codificación H.264/AAC compatible con todos los navegadores y dispositivos móviles.  
> **Misión:** Producir videos demostrativos 100% reales de cada proceso del sistema (mostrando la interfaz web verdadera navegando, llenando formularios y guardando datos en tiempo real), narrados en español o multi-idioma con voz natural y subtitulados automáticamente.

---

## 🎯 1. Reglas de Oro de los Videos Demostrativos de Procesos

1. **100% Interfaz Web Real en Ejecución (Prohibidas las Diapositivas Estáticas en Demos de Proceso):**
   - Un video demostrativo de capacitación de un proceso de negocio debe mostrar **exclusivamente la pantalla completa (1920×1080 a 1080p) de la aplicación real interactuando**: abriendo el menú, ingresando datos realistas paso a paso, accionando botones y mostrando la confirmación real del sistema.
   - Los diagramas conceptuales estáticos (como IDEF0 o arquitectura) se presentan como imágenes independientes en el manual, nunca reemplazando la demostración en vivo de la interfaz.

2. **Sincronización Exacta entre Acción en Pantalla y Locución Neural:**
   - El script de automatización divide el flujo del proceso en escenas atómicas (`Escena 1: Navegación al módulo`, `Escena 2: Captura del formulario`, `Escena 3: Validación y guardado`).
   - Primero se sintetiza el audio de cada escena con **Edge TTS (`edge-tts`)** para conocer su duración exacta en milisegundos, y el navegador automatizado acompasa los clics, el tipeo humanizado y las pausas a la duración de la voz.

3. **Resaltado Visual del Cursor y Foco:**
   - Inyectar un indicador visual sutil (cursor destacado o anillo de foco animado) antes de cada clic para que el espectador identifique inmediatamente qué botón o campo se está accionando.

---

## 🛠️ 2. Pipeline Determinista de Producción en 4 Etapas

```mermaid
flowchart LR
    A["1. Guion por Escenas + Subtítulos (.srt/.ass)"] --> B["2. Síntesis de Voz Neural (edge-tts MP3 + Duraciones)"]
    B --> C["3. Grabación de Navegador Real 1920x1080 (Playwright recordVideo)"]
    C --> D["4. Masterización FFmpeg (Video + Voz + Subtítulos -> .mp4 H.264)"]
```

### Comandos Canónicos de Síntesis y Masterización con `edge-tts` y `ffmpeg`
```bash
# 1. Generar locución neural y archivo de subtítulos sincronizados (.vtt / .srt)
edge-tts --voice "es-MX-DaliaNeural" --rate="+0%" \
  --text "En este proceso registraremos una nueva factura electrónica paso a paso." \
  --write-media escena_01.mp3 \
  --write-subtitles escena_01.vtt

# 2. Combinar video capturado del navegador con la pista de voz y codificar en MP4 universal (yuv420p)
ffmpeg -y -i ui_capture_raw.webm -i locucion_completa.mp3 \
  -vf "subtitles=subtitulos.srt:force_style='FontName=DejaVu Sans,FontSize=16,PrimaryColour=&H00FFFFFF,OutlineColour=&H00000000,BorderStyle=3,Outline=1'" \
  -c:v libx264 -preset fast -crf 20 -pix_fmt yuv420p \
  -c:a aac -b:a 192k -shortest \
  video_proceso_final.mp4
```

---

## ⛔ 3. Checklist Obligatorio de Cierre en Producción de Videos Demo (Quality Gate)

- [ ] ¿El video muestra la interfaz real del sistema en funcionamiento a 1920×1080 sin errores ni pantallas rotas?
- [ ] ¿El archivo `.mp4` utiliza formato de píxel `yuv420p` y códec `H.264` + `AAC` para reproducirse nativamente en Chrome, Safari, iOS, Android y WhatsApp?
- [ ] ¿Se verificó con `ffprobe` que el video tiene una duración válida, pista de audio activa (`> 0 dB`) y subtítulos legibles?
- [ ] ¿El video quedó correctamente enlazado en el manual de usuario y en el documento del proceso correspondiente?
