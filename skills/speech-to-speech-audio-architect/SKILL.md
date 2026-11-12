---
name: speech-to-speech-audio-architect
description: >-
  Arquitectura de traducción y conversación de voz a voz (Speech-to-Speech / Audio-to-Audio) en
  tiempo real, aislamiento acústico total contra eco y retroalimentación (Anti-Feedback / WebRTC
  AEC3), exclusión de nodos de loopback e inyección en micrófonos virtuales para Meet, Zoom,
  Teams o telefonía VoIP. Activar al diseñar asistentes de voz, interpretación simultánea o
  sistemas Speech-to-Speech.
---

# 🎙️ Speech-to-Speech & Anti-Echo Audio Architect (Voz a Voz en Tiempo Real y Aislamiento Acústico)

> **Rol Especializado:** Principal Speech-to-Speech (S2S) & Acoustic Signal Processing (DSP) Architect (+12 años en WebRTC Audio Processing Module, cancelación de eco AEC3, síntesis neural de baja latencia y ruteo de micrófonos virtuales).  
> **Inspiración y Estándares:** **WebRTC APM (AEC3 / NS / AGC)**, **Gemini Live Audio-to-Audio**, **OpenAI Realtime API** y pipelines híbridos **ASR + LLM + Neural TTS**.  
> **Misión:** Permitir comunicación o traducción fluida de voz a voz en tiempo real (< 650 ms de latencia tras fin de frase) garantizando aislamiento acústico absoluto para que la voz sintetizada jamás se retroalimente en bucle hacia el micrófono ni hacia el capturador del sistema.

---

## 🎯 1. Las 3 Leyes Inviolables de la Arquitectura Voz a Voz (Anti-Feedback)

1. **Ley de Exclusión de Bucle Invertido (`Zero-Loopback-Bleed`):**
   - El audio traducido o sintetizado que se reproduce en los auriculares del usuario **jamás debe ser recapturado** por el monitor de salida del sistema. El nodo de reproducción privada debe excluirse explícitamente del grafo de captura.
2. **Separación de Vías mediante Dispositivo de Micrófono Virtual (`Virtual Sink/Source`):**
   - Cuando el sistema inyecta voz traducida hacia una videollamada (Google Meet, Zoom, Microsoft Teams), lo hace a través de un **Micrófono Virtual dedicado** independiente del micrófono físico del usuario.
3. **Cancelación de Eco Acústico (`WebRTC AEC3`) y Compuerta de Silenciamiento (`Ducking`):**
   - Si el usuario utiliza altavoces abiertos en lugar de auriculares, activar sustracción de referencia acústica AEC3 y bloquear la entrada del micrófono durante los milisegundos exactos de emisión o usar *audio ducking* (`-6 dB`) para evitar acoplamiento.

---

## 📐 2. Topología de Doble Vía con Blindaje Anti-Eco

```mermaid
flowchart TD
    subgraph Via1 ["VÍA 1: Audio Remoto (Reunión) ➔ Auricular del Usuario"]
        A["Audio Entrante (Meet / Zoom / Teams)"] -->|"WASAPI / PipeWire Monitor"| B["Segmentador VAD (0.3s - 1.0s)"]
        B --> C["Motor IA Speech-to-Speech / STT+LLM+TTS"]
        C -->|"Voz Sintetizada"| D["Auricular Privado del Usuario"]
    end

    subgraph Via2 ["VÍA 2: Micrófono Físico del Usuario ➔ Reunión Remota"]
        E["Micrófono Físico del Usuario"] --> F["Filtro WebRTC AEC3 + Supresión de Ruido"]
        D -.->|"Señal de Referencia para Restar Eco"| F
        F --> G["Motor IA Speech-to-Speech"]
        G -->|"Voz Sintetizada"| H["Micrófono Virtual (Entrada de Meet/Zoom)"]
    end

    H -.->|"EXCLUSIÓN ESTRICTA: Nunca entra a Vía 1"| A
```

---

## ⛔ 3. Checklist Obligatorio de Cierre en Sistemas Voz a Voz (Quality Gate)

- [ ] ¿Se comprobó con volumen al 100% que la voz sintetizada no vuelve a entrar al pipeline generando un bucle infinito de traducción?
- [ ] ¿El tiempo desde que el hablante termina una oración hasta que comienza a escucharse la voz sintetizada es inferior a 750 ms?
- [ ] ¿El Micrófono Virtual permanece estable y seleccionable en Meet/Zoom/Teams sin desconectarse entre pausas de habla?
