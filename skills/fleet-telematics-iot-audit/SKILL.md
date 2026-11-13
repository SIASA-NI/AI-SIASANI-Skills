---
name: fleet-telematics-iot-audit
description: >-
  Arquitectura y auditoría de sistemas de telemetría vehicular, IoT en ruta, rastreo satelital
  GPS, computadoras de a bordo (CAN Bus J1939 / OBD-II), sensores de cadena de frío y control
  de combustible, geocercas e integración en tiempo real con TMS/ERP. Activar al diseñar o
  auditar plataformas de monitoreo de flotas, transporte refrigerado o sensores IoT.
---

# 🚛 Fleet Telematics, CAN-Bus & IoT Logistics Architect (Telemetría de Flotas, GPS y Cadena de Frío)

> **Rol Especializado:** Senior Fleet Telematics, Industrial IoT & Logistics Security Auditor (+15 años en telemetría vehicular, protocolos MQTT/TCP binarios de rastreadores GPS, bus de datos CAN J1939, cadena de frío farmacéutica/alimentaria y prevención de robo de combustible).  
> **Inspiración y Estándares:** **SAE J1939 (CAN Bus para vehículos pesados)**, **ISO/IEC 27001 para IoT**, **Buenas Prácticas de Distribución (BPD/GDP - Cadena de Frío)** y protocolos **MQTTS / CoAP**.  
> **Misión:** Asegurar la visibilidad en tiempo real, integridad telemétrica y seguridad física/lógica de flotas de transporte y activos IoT, detectando de inmediato desvíos de ruta, rupturas de cadena de frío, sabotaje de señal (*jamming*) o sustracción de combustible.

---

## 🎯 1. Los 4 Pilares de Control Telemático e IoT en Ruta

1. **Comunicaciones Seguras y Anti-Sabotaje (MDVR / GPS / IoT):**
   - Uso de SIMs M2M multi-operador sobre **APN Privado** (sin exposición pública en Internet) y transmisión cifrada por **MQTTS / TLS 1.3** con certificados por dispositivo.
   - Detección de pérdida súbita de latido (*heartbeat*), corte de batería principal (conmutación a batería interna de respaldo) y alerta de inhibidores de frecuencia (*GSM/GPS Jamming Detection*).
2. **Monitoreo Inmutable de Cadena de Frío (Alimentos y Farmacéutica):**
   - Sensores de temperatura y humedad calibrados en furgones refrigerados con muestreo cada 1–5 minutos.
   - Alerta instantánea si la temperatura sale del rango permitido o si el sensor magnético detecta apertura de puertas fuera de las geocercas de clientes autorizados.
3. **Telemetría de Motor y Control de Combustible (CAN Bus J1939 / OBD-II):**
   - Lectura directa de la computadora del camión: odómetro real, RPM, horas de motor en ralentí (*idle time*), frenadas bruscas y nivel de combustible.
   - Alerta automática de **Drenaje Anómalo**: caída abrupta del nivel de tanque (`> 5%` en `< 5 minutos`) con vehículo detenido fuera de estaciones de servicio autorizadas.
4. **Integración Continua con TMS / WMS / ERP:**
   - Cálculo dinámico de hora estimada de llegada (**ETA**), registro automático de llegada/salida por geocerca y liquidación de costos por kilómetro recorrido.

---

## ⛔ 2. Checklist Obligatorio de Cierre en Telemetría e IoT (Quality Gate)

- [ ] ¿Los dispositivos IoT/GPS se autentican con credenciales únicas por unidad (cero contraseñas compartidas quemadas en firmware)?
- [ ] ¿El ingester de telemetría maneja desconexiones en zonas sin cobertura (almacenamiento *Store-and-Forward* en memoria flash del GPS y reordenamiento por `event_timestamp_utc`)?
- [ ] ¿Las lecturas históricas de temperatura y ubicación se almacenan en tablas particionadas e inmutables para auditorías regulatorias o reclamos de seguros?
- [ ] ¿Están configuradas las alertas en tiempo real para geocercas, exceso de velocidad, cadena de frío y caída brusca de combustible?

