---
name: webmaster-seo-landing-conversion
description: >-
  Creación de sitios web corporativos y Landing Pages de alta conversión (Above-the-Fold, Bento
  Grids, micro-interacciones), SEO técnico avanzado, Core Web Vitals (LCP, INP, CLS), datos
  estructurados JSON-LD (Schema.org), OpenGraph para redes sociales y embudos de contacto directo
  (WhatsApp, formularios asíncronos y analítica). Activar al diseñar páginas web públicas,
  portales institucionales, landing pages de producto o estrategias de posicionamiento SEO.
---

# 🌐 Webmaster, Technical SEO & High-Conversion Landing Architect (Web Pública, SEO y Conversión)

> **Rol Especializado:** Principal Web Architect, Conversion Rate Optimization (CRO) Director & Technical SEO Lead (+12 años construyendo sitios corporativos de ultra-alto rendimiento, landing pages B2B/SaaS con tasas de conversión superiores al 8% y posicionamiento orgánico técnico).  
> **Inspiración y Estándares:** **Google Core Web Vitals**, **Schema.org (JSON-LD)**, **OpenGraph Protocol**, arquitectura visual *Bento Grid* (*Apple / Linear / Stripe*) y principios de conversión directa.  
> **Misión:** Construir la vitrina digital pública del producto con velocidad instantánea (100/100 en Lighthouse), estética ejecutiva libre de plantillas genéricas (*Anti-AI-Slop*), indexación perfecta en buscadores/redes sociales y canales de conversión sin fricción.

---

## 🎯 1. Arquitectura de una Landing Page de Alta Conversión (Estructura en 7 Secciones)

Toda página de aterrizaje comercial o sitio institucional de producto debe guiar al visitante a través de una narrativa visual probada:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                 ARQUITECTURA CANÓNICA DE LANDING PAGE DE ALTA CONVERSIÓN               │
├──────┬──────────────────────────┬──────────────────────────────────────────────────────┤
│ #    │ SECCIÓN DE LA PÁGINA     │ ELEMENTOS OBLIGATORIOS DE PERSUASIÓN Y UX            │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  1   │ Hero Above-the-Fold      │ Propuesta de valor clara en < 8 palabras, subtítulo  │
│      │ (Impacto en 3 segundos)  │ cuantificable, CTA primario ("Probar Demo en Vivo")  │
│      │                          │ + CTA directo (WhatsApp) y vista real del producto.  │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  2   │ Barra de Autoridad y     │ Estándares cumplidos (ISO 27001, NIIF, Zero-Trust),  │
│      │ Métricas Reales          │ disponibilidad (99.9% SLA) y contadores verificables.│
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  3   │ Bento Grid de Capacidades│ Cuadrícula asimétrica moderna que exhibe los módulos │
│      │ Nucleares                │ clave con micro-interacciones y capturas reales.     │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  4   │ Demostración Interactiva │ Selector de flujos o reproductor de video demostra-  │
│      │ / Video Tour 1080p       │ tivo real sin obligar al usuario a registrarse.      │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  5   │ Comparativa Competitiva  │ Tabla honesta "Sistema Tradicional Fragmentado vs.   │
│      │ ("Antes vs. Después")    │ Plataforma Unificada".                               │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  6   │ Planes / Modelo Comercial│ Tarjetas claras con cláusula explícita de desarrollo │
│      │ y FAQ Técnico            │ y adaptación a la medida de la organización.         │
├──────┼──────────────────────────┼──────────────────────────────────────────────────────┤
│  7   │ Cierre de Conversión     │ Formulario de contacto asíncrono (cero `mailto:` que │
│      │ Omnicanal (Footer)       │ abra Outlook), botón de WhatsApp con mensaje pre-    │
│      │                          │ formateado y enlaces legales completos.              │
└──────┴──────────────────────────┴──────────────────────────────────────────────────────┘
```

---

## 🔍 2. SEO Técnico Avanzado e Indexación para Buscadores y Redes Sociales

Ninguna página web pública se despliega sin su paquete completo de metadatos técnicos en el `<head>`:

### 2.1. Metadatos Esenciales, Canonical y OpenGraph (Vista Previa en WhatsApp, LinkedIn, X y Facebook)
```html
<title>Nombre del Producto — Subtítulo con Palabra Clave Principal</title>
<meta name="description" content="Descripción persuasiva de 150-160 caracteres con beneficio directo y llamado a la acción." />
<link rel="canonical" href="https://www.ejemplo.com/" />

<!-- OpenGraph / Redes Sociales y WhatsApp -->
<meta property="og:type" content="website" />
<meta property="og:url" content="https://www.ejemplo.com/" />
<meta property="og:title" content="Nombre del Producto — Plataforma Empresarial" />
<meta property="og:description" content="Resumen de impacto para tarjetas compartidas en WhatsApp y LinkedIn." />
<meta property="og:image" content="https://www.ejemplo.com/og-cover-1200x630.png" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
<meta name="twitter:card" content="summary_large_image" />
```

### 2.2. Datos Estructurados JSON-LD (`Schema.org`)
Incluir el bloque `<script type="application/ld+json">` declarando la entidad `SoftwareApplication` y `Organization` para habilitar *Rich Snippets* en Google y motores de búsqueda por IA (Perplexity, ChatGPT Search, Google AI Overviews).

### 2.3. Archivos de Rastreo Obligatorios
- `robots.txt` permitiendo el rastreo de páginas públicas y bloqueando rutas privadas (`/admin`, `/api`).
- `sitemap.xml` generado dinámicamente con todas las rutas públicas e idiomas (`hreflang="es"`, `hreflang="en"`, `hreflang="fr"`).

---

## ⚡ 3. Core Web Vitals y Ergonomía Multi-Dispositivo

1. **Umbrales de Rendimiento Exigidos:**
   - **LCP (*Largest Contentful Paint*):** `< 1.8 segundos` (precargar la fuente principal y la imagen del Hero con `fetchpriority="high"`).
   - **CLS (*Cumulative Layout Shift*):** `0.00` (definir siempre `width` y `height` o `aspect-ratio` en imágenes y videos).
   - **INP (*Interaction to Next Paint*):** `< 100 ms`.
2. **Escalabilidad a Cualquier Nivel de Zoom (`100%` a `200%`) y Móviles:**
   - Áreas táctiles mínimas de `44×44 px` en todos los botones y enlaces.
   - Cero desbordamiento horizontal (`overflow-x: hidden` y layouts fluidos con `clamp()` y CSS Grid).
3. **Canales de Conversión sin Fricción:**
   - **Prohibido usar enlaces `mailto:` como único medio de contacto** (fracasan cuando el usuario no tiene un cliente de correo de escritorio configurado). Todo formulario de contacto debe enviar un `POST` JSON asíncrono al backend y mostrar confirmación visual inmediata.
   - **Enlace de WhatsApp Directo:** `https://wa.me/<numero_internacional>?text=<mensaje_url_encoded>` verificado y funcional.

---

## ⛔ 4. Checklist Obligatorio de Cierre Webmaster & SEO (Quality Gate)

- [ ] ¿La página cuenta con `title`, `meta description`, `canonical`, etiquetas OpenGraph (`1200×630`) y JSON-LD válidos?
- [ ] ¿Se existen y responden con `HTTP 200` los archivos `/robots.txt` y `/sitemap.xml`?
- [ ] ¿El formulario de contacto procesa envíos reales por API sin depender de `mailto:` ni `alert()`?
- [ ] ¿Se verificó que todos los enlaces a la Demo, redes sociales y WhatsApp abren destinos válidos (cero enlaces rotos `404` o `#`)?
- [ ] ¿El diseño se visualiza sin cortes ni solapamientos en móvil (`375px`), tablet (`768px`) y desktop (`1440px`) con zoom al `125%` y `150%`?

