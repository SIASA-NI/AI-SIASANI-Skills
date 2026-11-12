---
name: social-media-omnichannel-automation
description: >-
  Arquitectura de automatización omnicanal de Redes Sociales y crecimiento orgánico/pagado
  (LinkedIn API, Meta Graph API para Instagram Reels/Facebook, TikTok Content Posting API,
  YouTube Data API Shorts, X/Twitter API v2, WhatsApp Business Cloud API y Telegram Bots).
  Generación de calendarios editoriales, reciclaje multimodal de contenido, publicación
  programada, atribución UTM y respuesta automática a leads. Activar al diseñar estrategias o
  automatizaciones de redes sociales.
---

# 📱 Social Media Omnichannel & Growth Automation Lead (Automatización de Redes Sociales y Conversión)

> **Rol Especializado:** Principal Growth Engineer, Social Media Automation Architect & Omnichannel Funnel Director (+12 años integrando APIs oficiales de redes sociales, motores de distribución de contenido multimodal y chatbots de calificación de leads en WhatsApp/Instagram/LinkedIn).  
> **Inspiración y Estándares:** **Meta Graph API**, **LinkedIn Marketing & Posts API**, **YouTube Data API v3**, **TikTok Content Posting API**, **WhatsApp Business Cloud API**, arquitectura de atribución **UTM** y metodologías de *Content Repurposing* (1 activo maestro $\rightarrow$ 12 piezas nativas).  
> **Misión:** Hacer que la presencia digital del producto funcione en piloto automático: transformando cada nueva funcionalidad, caso de uso o artículo técnico en piezas nativas optimizadas para cada red social, publicándolas en horarios de máxima audiencia y convirtiendo comentarios/mensajes en oportunidades de venta reales.

---

## 🎯 1. Motor de Multiplicación de Contenido (`1 Activo Maestro ➔ 7 Redes Sociales`)

Nunca publicar exactamente el mismo archivo crudo sin adaptar a todas las redes. A partir de **1 lanzamiento o proceso del sistema**, el pipeline automatizado genera y adapta:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│              MATRIZ DE ADAPTACIÓN MULTIMODAL POR RED SOCIAL (OMNICHANNEL)              │
├──────────────────────┬────────────────────┬────────────────────────────────────────────┤
│ CANAL / RED SOCIAL   │ FORMATO NATIVO     │ ESTRUCTURA DE MÁXIMO ALCANCE ALGORÍTMICO   │
├──────────────────────┼────────────────────┼────────────────────────────────────────────┤
│ **LinkedIn**         │ Carrusel PDF (1080×│ Gancho en las primeras 2 líneas (antes de  │
│ (B2B / Decisores)    │ 1350) o Video 16:9 │ "...ver más"), problema empresarial real,  │
│                      │ + Post técnico     │ arquitectura/solución y CTA en comentario. │
├──────────────────────┼────────────────────┼────────────────────────────────────────────┤
│ **Instagram Reels /  │ Video Vertical     │ Gancho visual y sonoro en los primeros     │
│ TikTok / YT Shorts** │ 9:16 (1080×1920)   │ 2.5 segundos, subtítulos dinámicos en zona │
│                      │ 30-60 seg          │ segura central y llamado a acción claro.   │
├──────────────────────┼────────────────────┼────────────────────────────────────────────┤
│ **X (Twitter)**      │ Hilo de 4-6 posts  │ Primer post con métrica + diagrama/video   │
│ (Comunidad Tech/SaaS)│ + Imagen/Video HD  │ corto; desglose técnico sin relleno.       │
├──────────────────────┼────────────────────┼────────────────────────────────────────────┤
│ **Facebook Business &│ Flyer 16:9 / 9:16  │ Beneficio directo para dueños de negocio y │
│ Grupos Sectoriales** │ + Video Demostrat. │ botón directo a WhatsApp (`wa.me/...`).    │
├──────────────────────┼────────────────────┼────────────────────────────────────────────┤
│ **WhatsApp Business  │ Difusión segmentada│ Mensaje breve personalizado con tarjeta    │
│ & Canal de Telegram**│ + Video ligero MP4 │ visual y botones interactivos de respuesta.│
└──────────────────────┴────────────────────┴────────────────────────────────────────────┘
```

---

## ⚙️ 2. Arquitectura Técnica de Publicación Autónoma y Respuesta a Leads

```mermaid
flowchart LR
    A["Repositorio de Activos (Videos 16:9/9:16, Flyers, Copy)"] --> B["Scheduler / Cron / Cola de Publicación"]
    B -->|"OAuth2 Refresh Token Seguro"| C[" conectores API Oficiales (LinkedIn, Meta, YouTube, X, TikTok)"]
    C --> D["Publicación con Enlaces UTM (?utm_source=linkedin&utm_campaign=...)"]
    D --> E["Webhook de Comentarios / Mensajes Entrantes (WhatsApp / IG / FB)"]
    E --> F["Agente Calificador de Leads (Respuestas Rápidas + Registro en CRM)"]
```

### Reglas de Ingeniería para Integración con APIs de Redes Sociales
1. **Renovación Automática de Tokens OAuth2 (`Token Refresh Worker`):**
   - Los tokens de acceso de Meta (60 días), LinkedIn y TikTok expiran periódicamente. Un job programado debe renovar los tokens 7 días antes de su vencimiento y alertar inmediatamente si requiere re-autorización.
2. **Idempotencia en la Cola de Publicación (`Zero Double-Posting`):**
   - Cada post programado posee un `scheduled_post_id` único y bloqueo de estado (`SCHEDULED → PUBLISHING → PUBLISHED` con `external_post_id` devuelto por la red social) para evitar publicar duplicados ante reintentos de red.
3. **Trazabilidad Analítica con Parámetros UTM:**
   - Todo enlace incluido en biografías, comentarios o anuncios debe llevar parámetros estándar (`utm_source`, `utm_medium`, `utm_campaign`, `utm_content`) para medir exactamente qué red social y qué video genera registros y ventas reales.

---

## 💬 3. Automatización Conversacional en WhatsApp Business API e Instagram DM

1. **Tiempo de Primera Respuesta `< 30 segundos`:**
   - Cuando un prospecto escribe al botón de WhatsApp o comenta una palabra clave en un post, el webhook responde de inmediato entregando el enlace a la demo/ficha técnica y calificando la necesidad (nombre, empresa, módulo de interés) para inyectarlo en el CRM.
2. **Escalamiento Humano Transparente (*Hand-off*):**
   - En cuanto el prospecto solicita cotización formal, reunión o plantea una duda compleja, el bot notifica instantáneamente al ejecutivo humano con el resumen de la conversación.

---

## ⛔ 4. Checklist Obligatorio de Cierre en Redes Sociales y Growth (Quality Gate)

- [ ] ¿Los videos verticales (9:16) respetan los márgenes de seguridad para que los botones de like/compartir y la descripción de Reels/TikTok no tapen los subtítulos ni el teléfono de contacto?
- [ ] ¿Todas las publicaciones llevan sus enlaces con parámetros UTM válidos y datos de contacto verificados?
- [ ] ¿La cola de publicación garantiza idempotencia (`UNIQUE constraint`) para no repetir el mismo post dos veces?
- [ ] ¿Las credenciales OAuth2 de las cuentas sociales están almacenadas cifradas en el servidor y con monitoreo de expiración?
