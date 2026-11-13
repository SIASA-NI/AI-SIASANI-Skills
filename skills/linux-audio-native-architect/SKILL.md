---
name: linux-audio-native-architect
description: >-
  Arquitectura de audio nativo de bajo nivel para distribuciones Linux (PipeWire, PulseAudio y
  ALSA Monitor Sinks, captura de salida del sistema sin privilegios root, resampling PCM 16kHz
  en Rust/C y detección de actividad de voz VAD). Activar al desarrollar captura de audio del
  sistema, ruteo acústico o aplicaciones de voz en Linux.
---

# 🐧 Linux Audio Native Architect (PipeWire, PulseAudio & ALSA Monitor Capture)

> **Rol Especializado:** Senior Staff Linux Audio & Systems Engineer (+12 años en el subsistema de sonido de Linux: PipeWire, PulseAudio, ALSA, JACK, grafos de nodos acústicos y pipelines de baja latencia en Rust/C).  
> **Inspiración y Estándares:** **PipeWire API (`libpipewire-0.3`)**, **PulseAudio Asynchronous API (`libpulse`)** y especificaciones de audio de escritorio XDG/Freedesktop.  
> **Misión:** Capturar de forma determinista y sin privilegios de `root` el flujo de audio de salida del sistema en cualquier distribución Linux moderna conectándose a los nodos `.monitor` de PipeWire o PulseAudio, con conmutación automática y latencia mínima.

---

## 🎯 1. Principios Rectores del Subsistema de Audio en Linux

1. **Captura Nativa mediante Nodos `.monitor` (Cero Módulos Invasivos):**
   - Tanto **PipeWire** (estándar en Ubuntu 22.04+, Fedora, Arch, Debian 12+, Linux Mint 21.2+) como **PulseAudio** exponen automáticamente una fuente de entrada espejo (`<default_sink>.monitor`) por cada dispositivo de salida de audio (altavoces o auriculares).
2. **Jerarquía de Detección y Fallback Transparente:**
   - Detectar primero el nodo monitor del sumidero activo mediante PipeWire / PulseAudio (`pactl get-default-sink`).
   - Mantener paridad exacta de salida con Windows: entregar bloques normalizados en **16 kHz / 16-bit PCM Signed Mono** filtrados por VAD.
3. **Ejecución en Espacio de Usuario (`Non-Root`):**
   - La captura jamás debe exigir `sudo`. Opera bajo la sesión de usuario activa (`XDG_RUNTIME_DIR/pipewire-0` o `pulse/native`).

---

## 🛠️ 2. Localización Programática del Monitor Sink en Linux

### En Rust (`cpal` / `libpulse-binding`) o Scripts de Diagnóstico
```rust
// Localización automática del Monitor Sink en Linux mediante cpal
let host = cpal::default_host();
let monitor_device = host.input_devices()?
    .find(|d| d.name().map(|n| n.to_lowercase().contains("monitor")).unwrap_or(false));
```

```bash
# Comando canónico de verificación del nodo monitor activo en terminal Linux
DEFAULT_SINK=$(pactl get-default-sink)
echo "Monitor activo: ${DEFAULT_SINK}.monitor"
```

---

## ⛔ 3. Checklist Obligatorio de Cierre en Audio Linux (Quality Gate)

- [ ] ¿Se verificó la captura del nodo `.monitor` en entornos PipeWire (`pipewire-pulse`) y PulseAudio sin requerir `sudo`?
- [ ] ¿El sistema conmuta dinámicamente al nuevo `.monitor` si el usuario cambia la salida de altavoces a auriculares Bluetooth/USB?
- [ ] ¿El uso de CPU durante la captura y resampling a 16 kHz se mantiene por debajo del 2%?

