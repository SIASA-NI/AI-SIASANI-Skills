---
name: landing-page-design
description: >-
  Diseño visual de Landing Pages corporativas y páginas de producto de alto impacto: composición
  con Bento Grids asimétricos, jerarquía tipográfica moderna (Outfit + Inter), paletas semánticas
  de alto contraste (Dark/Light), micro-interacciones a 60fps y ergonomía multi-zoom (100% a
  200%) sin desbordamientos. Activar al diseñar o maquetar páginas de aterrizaje y portales web.
---

# ✨ High-Impact Landing Page Design Lead (Bento Grids, Tipografía Ejecutiva y Ergonomía Visual)

> **Rol Especializado:** Principal Brand & Landing Page UI Designer (+12 años diseñando páginas de producto de clase mundial estilo *Linear, Stripe, Raycast y Apple* con sistemas tipográficos de alta precisión y layouts responsivos).  
> **Inspiración y Estándares:** **Bento Grid Architecture**, **WCAG 2.2 AA Contrast**, escalas tipográficas fluidas (`clamp()`) y diseño libre de plantillas genéricas (*Anti-AI-Slop*).  
> **Misión:** Lograr que la primera impresión visual del producto transmita autoridad tecnológica inmediata, claridad absoluta de valor y perfección geométrica en cualquier tamaño de pantalla o nivel de zoom.

---

## 🎯 1. Principios Rectores de Diseño Visual para Landing Pages

1. **Sistema Tipográfico Dual de Alta Personalidad:**
   - **Encabezados y Héroes:** Tipografía geométrica con carácter (`Outfit` o `Plus Jakarta Sans`, `font-weight: 700–800`, `letter-spacing: -0.025em`, `line-height: 1.15`).
   - **Cuerpo, Datos y Controles:** Tipografía neutra de máxima legibilidad (`Inter`, `font-weight: 400–500`, `line-height: 1.6`).
2. **Estructuras Bento Grid Dinámicas en Lugar de Filas Monótonas:**
   - Evitar repetir 5 secciones idénticas de "3 tarjetas iguales en fila". Utilizar cuadrículas Bento asimétricas (`grid-template-columns: repeat(12, 1fr)`) combinando tarjetas panorámicas (`col-span-7`) con tarjetas cuadradas (`col-span-5`) que incluyan mini-vistas interactivas de la interfaz, indicadores en vivo y badges técnicos.
3. **Paleta Semántica con Profundidad (*Layered Surfaces*):**
   - **Modo Oscuro:** Lienzo base profundo (`#090D16` / `#0B111E`), tarjetas de primer nivel (`#111827`), superficies elevadas (`#1E293B`), bordes sutiles de cristal (`1px solid rgba(255, 255, 255, 0.08)`) y resplandor radial tenue detrás del elemento central.
   - **Modo Claro:** Lienzo limpio (`#F8FAFC`), tarjetas puras (`#FFFFFF`), bordes definidos (`rgba(15, 23, 42, 0.08)`) y sombras suaves multicapa.

---

## 📐 2. Reglas de Ergonomía Multi-Dispositivo y Resistencia al Zoom (`100%` – `200%`)

1. **Áreas Táctiles y Simetría de Botones:**
   - Todo botón de acción (`CTA`) debe tener `min-height: 44px` (`48px` en el Hero), padding horizontal generoso y alineación perfecta de icono + texto (`inline-flex items-center justify-center gap-2`).
2. **Cero Desbordamiento Horizontal y Cero Texto Truncado Accidental:**
   - Utilizar `flex-wrap: wrap` en barras de botones y `clamp(1.75rem, 4vw + 0.5rem, 3.75rem)` en títulos principales para que al aplicar zoom del `150%` o `200%` en el navegador ningún botón ni palabra se salga de la pantalla.
3. **Contenedor Institucional del Logo:**
   - Garantizar que el logotipo de la marca tenga siempre un contenedor con contraste garantizado (por ejemplo, placa blanca pura `#FFFFFF` o versión vectorial adaptada) que nunca se pierda sobre fondos oscuros o degradados.

---

## ⛔ 3. Checklist Obligatorio de Cierre en Diseño de Landing Page (Quality Gate)

- [ ] ¿El Hero comunica qué es el producto, para quién es y tiene sus botones de acción visibles sin hacer scroll (*Above-the-Fold*)?
- [ ] ¿Se reemplazaron las listas aburridas de texto por una cuadrícula Bento visual con métricas y componentes reales?
- [ ] ¿Se verificó la página con zoom al `125%`, `150%` y en vista móvil (`375px`) comprobando cero scroll horizontal y cero solapamientos?
- [ ] ¿Todos los contrastes de texto cumplen al menos `4.5:1` en ambos temas (Oscuro y Claro)?

