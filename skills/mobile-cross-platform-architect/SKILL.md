---
name: mobile-cross-platform-architect
description: >-
  Arquitectura y desarrollo de aplicaciones móviles nativas y multiplataforma (React Native, Expo,
  Flutter, iOS Swift / Android Kotlin), sincronización Offline-First (SQLite/WatermelonDB y colas
  de mutación), notificaciones Push (FCM/APNs), almacenamiento criptográfico (Keychain/Keystore),
  autenticación biométrica y publicación en App Store y Google Play. Activar al diseñar o
  construir apps móviles.
---

# 📱 Mobile Cross-Platform & Offline-First Architect (iOS, Android, React Native & Flutter)

> **Rol Especializado:** Principal Mobile Systems Architect & Release Lead (+12 años construyendo aplicaciones móviles empresariales, financieras y de campo con soporte Offline-First, biometría y despliegues automatizados en App Store / Google Play).  
> **Inspiración y Estándares:** **Apple Human Interface Guidelines (HIG)**, **Google Material Design 3**, **OWASP MASVS (Mobile Application Security Verification Standard)** y arquitectura *Local-First / Offline-First*.  
> **Misión:** Construir aplicaciones móviles fluidas a 60/120 fps que sigan operando sin conexión a internet, resguarden tokens con seguridad de hardware y pasen a la primera las revisiones de Apple App Store y Google Play Store.

---

## 🎯 1. Principios Rectores de Ingeniería Móvil Empresarial

1. **Arquitectura Offline-First (La Red Móvil es Inestable por Naturaleza):**
   - Una app móvil jamás debe mostrar una pantalla en blanco ni perder datos capturados por el usuario si entra a un ascensor, bodega o zona sin señal celular.
   - **Lecturas desde Base de Datos Local:** La UI lee de una base local rápida (`SQLite` / `WatermelonDB` / `MMKV`) y sincroniza deltas en segundo plano (`pull/push sync`).
   - **Cola de Mutaciones Persistente (*Outbox Móvil*):** Las acciones creadas sin señal se encolan localmente con un `Idempotency-Key` UUID generado en el dispositivo y se despachan automáticamente al recuperar conectividad.

2. **Cero Tokens en Almacenamiento Plano (`AsyncStorage` / `SharedPreferences` Prohibidos para Secretos):**
   - Cumpliendo **OWASP MASVS**, los tokens JWT, *Refresh Tokens* y claves criptográficas se almacenan exclusivamente en el enclave seguro de hardware del sistema operativo: **iOS Keychain (`kSecAttrAccessibleWhenUnlockedThisDeviceOnly`)** y **Android Keystore (`EncryptedSharedPreferences`)**, protegidos opcionalmente con biometría (`FaceID` / `TouchID` / `BiometricPrompt`).

3. **Rendimiento Nativo a 60/120 FPS:**
   - Virtualizar todas las listas largas (`FlashList` / `FlatList` optimizada o `ListView.builder`).
   - Ejecutar animaciones y gestos en el hilo de UI/nativo (`Reanimated Worklets` / `Impeller`) sin bloquear el hilo de negocio.

---

## 🔄 2. Máquina de Sincronización Offline-First y Resolución de Conflictos

```mermaid
sequenceDiagram
    autonumber
    participant UI as Vista Móvil (60fps)
    participant LocalDB as SQLite Local + Outbox
    participant Sync as Sync Engine (NetInfo)
    participant API as Backend API Gateway

    UI->>LocalDB: Guardar Pedido / Formulario (UUID + Timestamp)
    LocalDB-->>UI: Confirmación Instantánea (< 15ms, Estado: PENDIENTE_SYNC)
    Note over Sync: Detecta Red Activa (Wi-Fi / 4G / 5G)
    Sync->>LocalDB: Leer mutaciones pendientes en orden FIFO
    Sync->>API: POST /api/v1/sync (Idempotency-Key: UUID)
    API-->>Sync: HTTP 200 OK (Server Version + Timestamp)
    Sync->>LocalDB: Marcar registro como SINCRONIZADO
    LocalDB-->>UI: Actualizar indicador visual a Check Verde
```

---

## 🔔 3. Notificaciones Push, Deep Linking y Ciclo de Vida en Segundo Plano

1. **Registro Idempotente de Tokens Push (FCM / APNs):**
   - Asociar el `push_token` al `(user_id, tenant_id, device_id)` en el backend al iniciar sesión, y **desvincularlo obligatoriamente al cerrar sesión (`logout`)** para que un usuario posterior en el mismo dispositivo nunca reciba notificaciones ajenas.
2. **Universal Links (iOS) y App Links (Android):**
   - Configurar `apple-app-site-association` y `assetlinks.json` para que los enlaces compartidos por WhatsApp o correo abran directamente la pantalla exacta dentro de la app nativa.

---

## 📦 4. Requisitos Bloqueantes para Aprobación en App Store (Apple) y Google Play

Para evitar rechazos durante la revisión oficial de las tiendas:
1. **Eliminación de Cuenta Integrada (Apple Guideline 5.1.1(v) & Google Play Data Safety):**
   - Si la app permite crear una cuenta, **debe incluir dentro de la propia app** la opción para solicitar la eliminación/baja de la cuenta.
2. **Justificación Clara de Permisos Sensibles (`Info.plist` / `AndroidManifest.xml`):**
   - Declarar textos explicativos precisos para cámara, ubicación, micrófono o galería, solicitando el permiso *just-in-time* (solo cuando el usuario toca la función que lo requiere, nunca de golpe al abrir la app por primera vez).
3. **Actualizaciones OTA Seguras y Versionado Semántico:**
   - Respetar la compatibilidad de API mínima (`minSupportedAppVersion`), mostrando un modal de "Actualización Requerida en Tienda" cuando cambien contratos nativos.

---

## ⛔ 5. Checklist Obligatorio de Cierre Móvil (Quality Gate)

- [ ] ¿Los tokens de sesión se guardan en Keychain/Keystore cifrado y se purgan completamente en el `logout`?
- [ ] ¿Se probó activar el "Modo Avión" en mitad de una captura de datos verificando que la app no crashea y sincroniza al reconectar?
- [ ] ¿Las vistas respetan las zonas seguras de pantalla (*SafeAreaView* para Notch, Dynamic Island y barra de gestos inferior) y el teclado virtual (`KeyboardAvoidingView`) no tapa el botón de envío?
- [ ] ¿Están configurados los iconos adaptativos, Splash Screen y firmas de release (`.aab` / `.ipa`)?
