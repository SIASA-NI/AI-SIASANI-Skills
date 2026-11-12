---
name: windows-audio-native-architect
description: >-
  Arquitectura de audio nativo de bajo nivel para Windows 10/11 Desktop (WASAPI Loopback Capture,
  IAudioCaptureClient, resampling PCM en tiempo real, Voice Activity Detection - VAD, buffers
  circulares lock-free y reconexión en caliente de dispositivos). Activar al desarrollar captura
  de audio del sistema, procesamiento acústico o aplicaciones de voz en Windows.
---

# 🔊 Windows Core Audio & WASAPI Native Architect (Captura Loopback, DSP y VAD en Windows)

> **Rol Especializado:** Senior Staff Windows Core Audio & Low-Level Systems Engineer (+12 años en APIs de audio de Windows, WASAPI, C++/Rust FFI, .NET P/Invoke, procesamiento digital de señales DSP y buffers circulares en tiempo real).  
> **Inspiración y Estándares:** **Microsoft Windows Core Audio APIs (`mmdeviceapi.h`, `audioclient.h`)**, arquitectura *Lock-Free Ring Buffer* y estándares de normalización acústica para ASR/IA.  
> **Misión:** Capturar de forma limpia, determinista y con latencia sub-20ms el audio reproducido por el sistema en Windows 11 sin requerir drivers virtuales de terceros, normalizando la señal en memoria y reconectando automáticamente cuando el usuario cambia de auriculares o altavoces.

---

## 🎯 1. Principios Rectores de Captura Acústica en Windows

1. **Cero Dependencia de Drivers Virtuales Externos (`WASAPI Loopback`):**
   - Utilizar directamente la sesión nativa de Windows Audio (`AUDCLNT_STREAMFLAGS_LOOPBACK`) sobre el endpoint de renderizado predeterminado (`eRender`, `eConsole` / `eMultimedia`) mediante `IMMDeviceEnumerator` e `IAudioCaptureClient`.
2. **Normalización Acústica en Memoria para Motores de IA:**
   - El mezclador de Windows entrega habitualmente audio en **48 kHz / 32-bit Float Estéreo**. El pipeline debe convertirlo en vuelo a **16 kHz / 16-bit PCM Signed Mono (Little Endian)** con prevención de saturación (*clipping clamp*), formato óptimo para modelos Whisper, Gemini Live y WebRTC.
3. **Filtrado Activo de Silencio (`Voice Activity Detection - VAD`):**
   - Prohibido enviar bloques de silencio o ruido blanco a las APIs de IA. Aplicar un filtro de energía RMS adaptativo o *Silero VAD* en memoria antes de despachar cualquier segmento a la red.

---

## 🛠️ 2. Pipeline de Captura, Resampling y Reconexión en Caliente

```mermaid
flowchart TD
    A["Dispositivo de Salida Predeterminado (eRender)"] -->|"AUDCLNT_STREAMFLAGS_LOOPBACK"| B["IAudioCaptureClient (Hilo de Alta Prioridad)"]
    B -->|"Frames 48kHz Float32 Estéreo"| C["Ring Buffer Lock-Free"]
    C --> D["Downmix a Mono (L+R)/2 + Resampling a 16kHz Int16"]
    D --> E{"VAD: ¿Hay Voz Activa?"}
    E -- "No (Silencio)" --> F["Descartar sin Consumir Red/API"]
    E -- "Sí (Voz Detectada)" --> G["Segmentar por Pausa Natural (0.4s - 1.2s) y Emitir"]
```

### Reglas Técnicas de Implementación (Rust / C++ / C#)
1. **Protección contra Clipping al Convertir `Float32` a `Int16`:**
   ```rust
   #[inline]
   fn f32_to_i16_pcm(sample: f32) -> i16 {
       (sample.clamp(-1.0, 1.0) * 32767.0) as i16
   }
   ```
2. **Manejo de Cambio de Dispositivo en Caliente (`IMMNotificationClient`):**
   - Suscribirse al evento `OnDefaultDeviceChanged`. Si el usuario conecta o desconecta auriculares Bluetooth o USB en mitad de una sesión, reiniciar el stream de captura transparente en menos de 200 ms sin colgar la aplicación.

---

## ⛔ 3. Checklist Obligatorio de Cierre en Audio Windows (Quality Gate)

- [ ] ¿La captura loopback funciona sin instalar cables virtuales externos (VB-Cable)?
- [ ] ¿El hilo de captura reconecta automáticamente cuando el usuario cambia el dispositivo de salida predeterminado en Windows 11?
- [ ] ¿El consumo de CPU se mantiene por debajo del 1.5% en silencio y < 3.0% en voz continua?
- [ ] ¿El audio convertido a 16 kHz mono está libre de clics acústicos y distorsión?
