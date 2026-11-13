---
name: edge-serverless-cdn-architect
description: Arquitectura de Edge Computing, Serverless Workers (Cloudflare Workers, Vercel Edge), Durable Objects, caché distribuido (Stale-While-Revalidate), mitigación DDoS/WAF en el borde y enrutamiento global de latencia ultra-baja.
---

# 🌐 Edge Computing, Serverless Workers & CDN Security Architect

## 1. Propósito y Alcance
Este skill capacita al agente como **Principal Edge & Serverless Architect** (inspirado en `cloudflare/skills` y `vercel-labs/agent-skills`), dominando la ejecución distribuida en el borde de la red (Edge PoPs), caché inteligente, estado coordinado globalmente y defensa perimetral WAF/DDoS antes de que el tráfico toque los servidores de origen.

---

## 2. Patrones de Arquitectura en el Borde (*Edge-First Patterns*)

### 2.1. Seguridad y Autenticación en el Edge (*Zero-Origin-Hit Validation*)
- **Verificación JWT Asimétrica en el Borde:** Validar firmas `RS256` / `EdDSA` contra el endpoint `JWKS` cacheado en el Edge usando la API nativa `Web Crypto API` (`crypto.subtle.verify`). Las peticiones con tokens expirados o inválidos son rechazadas en `< 2 ms` en el PoP más cercano al usuario sin consumir CPU ni ancho de banda del servidor de origen.
- **Rate Limiting Distribuido y Protección Anti-Bot:**
  - Aplicar límites de tasa por IP, huella TLS (`JA3/JA4`), ruta y `TenantId` en el borde.
  - Proteger formularios públicos de login, registro y recuperación de contraseña con retos criptográficos invisibles (Cloudflare Turnstile o Canvas/Proof-of-Work sin fricción).

### 2.2. Ingeniería de Caché HTTP Avanzada (RFC 9111)
- **Activos Estáticos con Hash en Nombre (`/_next/static/*`, `/assets/*`):**
  - `Cache-Control: public, max-age=31536000, immutable` (1 año de caché sin revalidación).
- **Contenido Dinámico Público / Catálogos / Landing Pages:**
  - `Cache-Control: public, s-maxage=60, stale-while-revalidate=86400, stale-if-error=604800` (el CDN responde instantáneamente desde caché y actualiza en segundo plano, o sirve la copia válida si el origen entra en mantenimiento).
- **Datos Privados o Multi-Tenant Autenticados (`/api/*`):**
  - `Cache-Control: private, no-store, no-cache, must-revalidate` o particionamiento estricto de clave de caché por `Vary: Authorization, X-Tenant-Id` para **jamás filtrar respuestas de un inquilino a otro (*Web Cache Deception*)**.

### 2.3. Restricciones del Runtime V8 Isolates (Workers / Edge Middleware)
1. **Cero Dependencias Pesadas de Node.js:** En runtimes basados en V8 Isolates, utilizar estándares Web (`fetch`, `Request`, `Response`, `TransformStream`, `TextEncoder`, `Web Crypto`) en lugar de librerías nativas de C++ o `fs` síncrono.
2. **Conexiones a Base de Datos desde Serverless:** Utilizar *Connection Poolers* transaccionales (`PgBouncer`, `Supavisor`) o drivers HTTP/WebSocket con estado efímero para no agotar el límite `max_connections` de PostgreSQL ante picos de miles de instancias concurrentes.
3. **Estado Coordinado con `Durable Objects` / Redis Edge:** Usar actores con afinidad geográfica única para salas colaborativas WebSocket, contadores atómicos y colas de eventos en tiempo real.

