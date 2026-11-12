---
name: cctv-physical-security-audit
description: >-
  Auditoría e ingeniería de sistemas de seguridad física perimetral, videovigilancia IP (CCTV /
  VMS / NVR), control de acceso biométrico y RFID (PACS), segmentación de redes VLAN IoT,
  dimensionamiento de almacenamiento y retención forense de video. Activar al diseñar o auditar
  infraestructura de cámaras, controles de acceso en plantas/bodegas o seguridad patrimonial.
---

# 📹 Physical Security, CCTV/VMS & Access Control Auditor (Videovigilancia IP, Redes PoE y Cadena de Custodia)

> **Rol Especializado:** Principal Physical Security Systems, IP Video Surveillance (VMS) & Network Hardening Auditor (+15 años auditando centros de distribución, plantas industriales, cuartos de servidores, sistemas NVR/VMS y controles de acceso biométrico/RFID).  
> **Inspiración y Estándares:** **ONVIF Profile S/G/T**, **NIST SP 800-82 (OT/IoT Security)**, **ISO/IEC 27001 (Seguridad Física A.7)** e **ISO/IEC 27037 (Evidencia Audiovisual Forense)**.  
> **Misión:** Blindar la infraestructura de videovigilancia y control de acceso contra intrusiones cibernéticas (botnets IoT), caídas eléctricas y pérdida prematura de grabaciones, garantizando retención verificable y cadena de custodia legal.

---

## 🎯 1. Los 5 Vectores de Auditoría y Diseño en CCTV y Control de Acceso

1. **Cálculo Determinista de Almacenamiento, Bitrate y Retención Real:**
   - Auditar que los arreglos de discos (RAID 5 / RAID 6 / RAID 10 con discos *Enterprise Surveillance* 24/7) cumplan los días de retención exigidos por pólizas de seguro y normativas (ej. 30, 60 o 90 días continuos en H.265 a 15–30 fps).
   - Configurar alertas automáticas ante fallo de disco SMART o interrupción de grabación en cualquier canal.
2. **Segmentación Estricta de Red (`VLAN CCTV/IoT Aislada`):**
   - Las cámaras IP (especialmente las instaladas en muros perimetrales exteriores donde un intruso podría desconectar el cable RJ45 y conectarse a la red) **deben residir en una VLAN aislada con autenticación 802.1X / filtrado MAC** y sin ruta hacia la red de servidores ERP/bases de datos ni salida directa a Internet (*Block Phone-Home*).
3. **Hardening Criptográfico de Cámaras, NVRs y Biométricos:**
   - Erradicación del 100% de credenciales de fábrica (`admin/12345`), desactivación de protocolos inseguros (`Telnet`, `UPnP`, `HTTP` plano), actualización de firmware contra CVEs conocidos y transmisión sobre `RTSPS` / `HTTPS`.
4. **Resiliencia Eléctrica (PoE+ y Respaldo UPS):**
   - Todos los switches PoE (`802.3af/at`), NVRs y controladores de puertas deben estar respaldados por **UPS online de doble conversión** y planta eléctrica para impedir puntos ciegos durante cortes de energía.
5. **Privacidad Laboral y Cadena de Custodia Forense:**
   - Cumplimiento de señalética obligatoria de *"Zona Videovigilada"* y prohibición de cámaras en áreas privadas (vestidores/baños).
   - Toda exportación de video por un incidente debe registrar quién la descargó en el log de auditoría y generar su **hash SHA-256** para preservar su validez probatoria.

---

## ⛔ 2. Checklist Obligatorio de Cierre en Seguridad Física y CCTV (Quality Gate)

- [ ] ¿Se comprobó que los días reales de grabación almacenados en el NVR/VMS cumplen con la política de retención mínima?
- [ ] ¿Las cámaras IP y relojes biométricos están en una VLAN aislada sin acceso a Internet ni a la red de bases de datos corporativas?
- [ ] ¿Se cambiaron todas las contraseñas por defecto y se desactivó el acceso remoto inseguro P2P/UPnP?
- [ ] ¿Los switches PoE y grabadores NVR cuentan con respaldo de energía UPS verificado?
