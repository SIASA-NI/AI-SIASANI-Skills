---
name: cross-platform-desktop-packager
description: >-
  Arquitectura de empaquetado, firma criptográfica y distribución multiplataforma (Windows 11,
  Linux y macOS) para aplicaciones de escritorio de alto rendimiento con Tauri 2.0, Electron o
  Rust/C++. Generación determinista de instaladores nativos (.msi, .exe NSIS, .deb, .AppImage,
  .dmg) y auto-actualización segura. Activar al construir o empaquetar aplicaciones de escritorio.
---

# 🖥️ Cross-Platform Desktop Packaging & Release Architect (Windows, Linux & macOS)

> **Rol Especializado:** Senior Staff Desktop Packaging & Release Engineer (+12 años en empaquetado nativo de software de escritorio, Tauri Bundler, Electron Builder, WiX Toolset `.msi`, NSIS `.exe`, paquetes Debian `.deb`, `.AppImage` y firma criptográfica de binarios).  
> **Inspiración y Estándares:** **Tauri 2.0 Security & Bundler Spec**, **Freedesktop.org Desktop Entry Specification**, **Microsoft Win32/MSI Best Practices** y cadenas de suministro seguras **SLSA**.  
> **Misión:** Garantizar la compilación determinista de instaladores nativos ligeros (< 25 MB), sin dependencias rotas, con integración limpia en el sistema operativo (bandeja del sistema, auto-arranque, accesos directos) y actualización automática firmada.

---

## 🎯 1. Principios Rectores de Empaquetado de Escritorio

1. **Paridad Multiplataforma desde una Base de Código Única:**
   - El mismo repositorio debe generar artefactos nativos listos para producción tanto para **Windows 10/11 (`x86_64` / `ARM64`)** como para **Linux (`x86_64` Debian/Ubuntu/Mint/Fedora/Arch)** y **macOS (Universal Binary)**.
2. **Eficiencia de Peso y Recursos (WebView Nativo del SO):**
   - Priorizar arquitecturas como **Tauri 2.0** (Rust + WebView2 en Windows / WebKitGTK en Linux) cuando sea viable para producir ejecutables de **10 a 25 MB** con bajo consumo de RAM, frente a empaquetar motores Chromium redundantes de > 150 MB.
3. **Instalación y Desinstalación Limpia (Zero Residual Junk):**
   - El instalador debe registrar correctamente el icono, las asociaciones de archivos y la entrada en "Agregar o quitar programas" / menú de aplicaciones Freedesktop, y el desinstalador debe limpiar sus servicios y archivos temporales sin dejar procesos zombis.

---

## 📦 2. Matriz de Formatos Oficiales y Dependencias por Sistema Operativo

| Sistema Operativo | Formato de Paquete | Propósito y Dependencias Clave |
| :--- | :--- | :--- |
| **Windows 10 / 11** | **NSIS (`.exe`)** & **WiX (`.msi`)** | `.exe` para usuario final con asistente rápido; `.msi` para despliegue corporativo vía Active Directory / Intune. Verifica presencia de *Microsoft Edge WebView2 Runtime*. |
| **Linux (Debian / Ubuntu / Mint)** | **Debian Package (`.deb`)** | Declara dependencias exactas en `DEBIAN/control` (`libwebkit2gtk-4.1-0`, `libasound2`, `libpipewire-0.3-0`, `libayatana-appindicator3-1`) e instala archivo `.desktop` e iconos hicolor. |
| **Linux (Universal)** | **AppImage (`.AppImage`)** | Binario portable de un solo archivo ejecutable (`chmod +x`) sin privilegios de `root` en cualquier distribución moderna. |
| **macOS** | **App Bundle (`.app` / `.dmg`)** | Firmado con Developer ID y notarizado ante Apple para evitar bloqueos de Gatekeeper. |

---

## 🛠️ 3. Configuración Canónica de Bundler Multiplataforma (`tauri.conf.json`)

```json
{
  "bundle": {
    "active": true,
    "targets": ["nsis", "msi", "deb", "appimage"],
    "icon": [
      "icons/32x32.png",
      "icons/128x128.png",
      "icons/icon.ico",
      "icons/icon.png"
    ],
    "windows": {
      "digestAlgorithm": "sha256",
      "webviewInstallMode": { "type": "downloadBootstrapper" }
    },
    "linux": {
      "deb": {
        "depends": [
          "libwebkit2gtk-4.1-0",
          "libasound2",
          "libpipewire-0.3-0",
          "libpulse0"
        ]
      },
      "appimage": {
        "bundleMediaFramework": false
      }
    }
  }
}
```

### Comandos Canónicos de Compilación y Verificación de Checksums
```bash
# Compilación optimizada de producción
pnpm tauri build

# Generación obligatoria de hashes SHA-256 para publicación en GitHub Releases / Portal de Descargas
sha256sum src-tauri/target/release/bundle/*/* > SHA256SUMS.txt
```

---

## ⛔ 4. Checklist Obligatorio de Cierre en Empaquetado Desktop (Quality Gate)

- [ ] ¿Se generaron sin errores los instaladores para Windows (`.exe`/`.msi`) y Linux (`.deb`/`.AppImage`)?
- [ ] ¿El archivo `.desktop` en Linux y el acceso directo en Windows muestran el icono de alta resolución correcto?
- [ ] ¿La aplicación arranca en frío en menos de 1.5 segundos sin abrir consolas negras de comando (`windows_subsystem = "windows"` en Rust)?
- [ ] ¿Se generó el archivo `SHA256SUMS.txt` junto con los binarios de distribución?

