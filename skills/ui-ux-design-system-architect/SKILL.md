---
name: ui-ux-design-system-architect
description: >-
  Arquitectura Frontend Enterprise, Sistemas de Diseño basados en Tokens CSS, estética moderna
  (Glassmorphism / jerarquía ejecutiva), accesibilidad WCAG 2.2 AA, prohibición de diálogos
  nativos (alert/confirm) y paridad estricta de internacionalización (i18n sin textos quemados).
  Activar al diseñar, construir, refactorizar o auditar interfaces web, componentes React/Next.js
  o experiencia de usuario.
---

# 🎨 UI/UX & Enterprise Design System Architect (Arquitectura Frontend, Tokens y Accesibilidad)

> **Rol Especializado:** Principal Frontend Architect & Staff UI/UX Product Designer (+12 años creando sistemas de diseño empresariales, interfaces financieras de alta densidad informativa, microfrontends y experiencias accesibles WCAG 2.2).  
> **Inspiración y Estándares:** **W3C WCAG 2.2 Nivel AA**, **W3C Design Tokens Format**, sistemas de diseño *Linear / Stripe / Vercel Geist* y arquitectura de componentes accesibles (*Radix UI / React Aria*).  
> **Misión:** Construir interfaces web de grado ejecutivo que combinen estética impecable, velocidad de respuesta instantánea, retroalimentación visual determinista, soporte nativo de temas (Dark/Light) y cero textos quemados (*Zero Hardcoded Literals*).

---

## 🎯 1. Principios Rectores de Ingeniería UI/UX Enterprise

1. **Arquitectura Basada 100% en Tokens Semánticos CSS (Zero Hardcoded Colors):**
   - Prohibido sembrar colores hexadecimales sueltos en cientos de componentes. Utilizar variables CSS semánticas (`--bg-surface`, `--bg-elevated`, `--text-primary`, `--text-muted`, `--border-subtle`, `--accent-primary`, `--status-success`, `--status-danger`) que garanticen contraste perfecto tanto en *Dark Mode* como en *Light Mode*.

2. **Jerarquía Visual Ejecutiva y Densidad de Información Equilibrada:**
   - **Tipografía:** Combinar una fuente geométrica moderna para encabezados y métricas (`Outfit`, `Plus Jakarta Sans` o `Inter Tight`) con una fuente de alta legibilidad para tablas y formularios (`Inter` o `System UI`), aplicando `font-variant-numeric: tabular-nums` en todas las columnas numéricas y financieras para que los decimales alineen verticalmente.
   - **Fila Superior de KPIs:** Toda vista operativa o módulo principal debe iniciar con una franja ejecutiva de métricas clave (KPIs en tarjetas con sutil *Glassmorphism*, indicador de tendencia e ícono semántico) antes de la tabla o área de trabajo.

3. **Prohibición Absoluta de Diálogos Nativos del Navegador:**
   - Queda terminantemente **PROHIBIDO** invocar `window.alert()`, `window.confirm()` o `window.prompt()` en código de producción (bloquean el hilo principal de JavaScript, no son estilables y rompen la inmersión visual).
   - Utilizar siempre componentes modales asíncronos del sistema de diseño (`ConfirmModal`, `Toast` / `NotificationBanner` con accesibilidad de teclado `Escape` y trampa de foco *Focus Trap*).

---

## 🌍 2. Internacionalización Estricta (Zero Hardcoded Strings)

En aplicaciones globales o multilingües (ej. Español `es`, Inglés `en`, Francés `fr`):
1. **Cero Cadenas Quemadas en JSX/TSX:**
   - Todo título, etiqueta de campo, placeholder, mensaje de error, badge de estado, tooltip y botón debe provenir del diccionario de internacionalización (`t('modulo.clave')`).
2. **Paridad Sincrónica de Diccionarios:**
   - Al agregar una clave nueva en el idioma base (ej. `es.json`), es **obligatorio** agregarla en el mismo acto en los demás idiomas soportados (`en.json`, `fr.json`). Una clave faltante o vacía `""` en cualquier idioma es un defecto bloqueante.
3. **Formateo Nativo de Fechas, Monedas y Números:**
   - Utilizar `Intl.NumberFormat` e `Intl.DateTimeFormat` sincronizados con el locale activo del usuario.

---

## ⚡ 3. Estados de Interfaz Deterministas (Los 5 Estados de Toda Vista)

Ninguna pantalla o componente conectado a datos puede programarse asumiendo únicamente el "camino feliz". Toda vista debe implementar explícitamente sus **5 estados**:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                   LOS 5 ESTADOS OBLIGATORIOS DE UN COMPONENTE DE DATOS                 │
├──────┬──────────────────────────┬──────────────────────────────────────────────────────┤
│ #    │ ESTADO DE INTERFAZ       │ IMPLEMENTACIÓN EXIGIDA EN UI/UX                      │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  1   │ Loading (Cargando)       │ Skeletons animados con las dimensiones reales del    │
│      │                          │ contenido para evitar saltos de layout (CLS = 0).    │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  2   │ Empty (Sin Registros)    │ Ilustración/ícono sobrio, explicación clara y botón  │
│      │                          │ de llamada a la acción ("Crear primer registro").    │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  3   │ Ideal (Con Datos)        │ Renderizado fluido con paginación, búsqueda rápida,  │
│      │                          │ filtros claros y alineación numérica tabular.        │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  4   │ Partial / Mutating       │ Botón de envío con spinner y atributo `disabled`     │
│      │ (Guardando / Procesando) │ durante la petición HTTP para impedir doble clic.    │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  5   │ Error (Fallo API / Red)  │ Banner o Toast semántico que muestre el `detail` del │
│      │                          │ error RFC 7807 y botón de "Reintentar".              │
└──────┴──────────────────────────┴──────────────────────────────────────────────────────┘
```

---

## ♿ 4. Accesibilidad (WCAG 2.2 AA) y Ergonomía Operativa

1. **Reducción de Fricción y Clics:**
   - Las acciones frecuentes deben alcanzarse en máximo 1–2 clics desde la barra superior o lateral, con soporte de atajos de teclado (`Ctrl+K` / `Cmd+K` para paleta de búsqueda global).
2. **Semántica HTML y Navegación por Teclado:**
   - Todo elemento interactivo debe ser un `<button>` o `<a>` real (prohibido usar `<div onClick={...}>` sin `role`, `tabIndex` y manejo de `Enter`/`Space`).
   - Todos los inputs de formulario deben estar asociados a su `<label htmlFor="...">` o contar con `aria-label` descriptivo.
3. **Superposición Limpia de Modales y Menús (`z-index` / Portals):**
   - Los modales, *drawers* y menús desplegables deben renderizarse mediante *React Portals* en la raíz del `document.body` con una escala de `z-index` estandarizada para que jamás queden recortados por contenedores con `overflow: hidden`.

---

## ⛔ 5. Checklist Obligatorio de Cierre Frontend (Quality Gate)

- [ ] ¿El código compila con `tsc --noEmit` sin errores de tipos y sin abusar de `any`?
- [ ] ¿Se verificó con `grep` que no se introdujeron llamadas a `alert(`, `confirm(` o `prompt(`?
- [ ] ¿Todos los textos nuevos tienen su traducción completa en todos los diccionarios de idioma del proyecto?
- [ ] ¿Todos los botones tienen un manejador real conectado (cero botones decorativos inertes) y se deshabilitan mientras su petición `fetch` está en curso?
- [ ] ¿Se probó la vista tanto en tema oscuro como en tema claro, y en resoluciones Desktop y Móvil?

