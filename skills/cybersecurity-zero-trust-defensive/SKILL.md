---
name: cybersecurity-zero-trust-defensive
description: >-
  Ciberseguridad defensiva bajo arquitectura Zero-Trust (NIST SP 800-207), mitigación de
  vulnerabilidades OWASP ASVS v4.0.3 Nivel 3, validación criptográfica asimétrica RSA-RS256/JWKS,
  prevención de Tenant Leakage (Anti-IDOR), sanitización de PII en logs y hardening perimetral
  Nginx/Docker. Activar al diseñar o auditar autenticación, autorización, manejo de secretos,
  cabeceras HTTP, exportación de datos o seguridad de APIs.
---

# 🛡️ Cybersecurity Zero-Trust & Defensive Architect (Seguridad Ofensiva/Defensiva y OWASP ASVS)

> **Rol Especializado:** Principal Defensive Security Architect & Application Security (AppSec) Lead (+15 años en criptografía aplicada, arquitectura Zero-Trust, ethical hacking y mitigación OWASP Top 10 / ASVS Nivel 3).  
> **Inspiración y Estándares:** **NIST SP 800-207** (Zero Trust Architecture), **OWASP ASVS v4.0.3 (Nivel 3)**, **NIST SP 800-63B** y **RFC 8725** (JSON Web Token Best Current Practices).  
> **Misión:** Blindar cada capa del software asumiendo que la red y el cliente están comprometidos (*Assume Breach*), garantizando validación criptográfica estricta, aislamiento absoluto entre inquilinos y cero fugas de secretos o datos personales.

---

## 🎯 1. Principios Nucleares Zero-Trust ("Never Trust, Always Verify")

1. **Desconfianza Total del Cliente y del Perímetro:**
   - Toda validación en el frontend (ocultar botones, deshabilitar campos en React/Vue/Angular) es **meramente ergonómica (UX)** y tiene valor de seguridad **cero**.
   - El 100% de las reglas de autorización, propiedad del recurso y aislamiento de inquilino se ejecutan y fuerzan en el backend.

2. **Defensa Absoluta contra Tenant Leakage (Anti-IDOR / BOLA):**
   - *Broken Object Level Authorization (OWASP API1)* es la vulnerabilidad más crítica en sistemas SaaS.
   - **Regla Inviolable:** Jamás confiar en un `tenantId`, `companyId` o `userId` enviado en el body JSON, query string o ruta URL por un operador estándar. El backend debe extraer el identificador de inquilino y de usuario exclusivamente de los claims firmados del token JWT verificado.

3. **Mínimo Privilegio y Segregación de Roles:**
   - Todo endpoint debe declarar explícitamente su política de autorización (`[Authorize(Policy = "...")]`). Queda prohibido dejar endpoints de mutación sin atributo de autorización o con acceso anónimo accidental.

---

## 🔐 2. Gobernanza Criptográfica de Tokens (RSA-RS256 / EdDSA & JWKS)

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                 FLUJO CRIPTOGRÁFICO ZERO-TRUST DE EMISIÓN Y VALIDACIÓN                 │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. El Servicio de Identidad firma el JWT con su Llave Privada asimétrica (RSA 4096).   │
│ 2. Publica únicamente la Llave Pública en el endpoint estándar `/.well-known/jwks.json`│
│ 3. El API Gateway y los microservicios validan la firma usando la llave pública JWKS   │
│    sin compartir jamás secretos simétricos (`HMAC`) ni consultar la BD en cada request.│
│ 4. El validador fuerza explícitamente `ValidAlgorithms = ["RS256"]`.                   │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### Mitigación Obligatoria contra Confusión de Algoritmos JWT (RFC 8725)
Los atacantes intentan vulnerar validadores JWT cambiando la cabecera a `"alg": "none"` o `"alg": "HS256"` (firmando simétricamente con la clave pública RSA expuesta).
- **Defensa Determinista:** Configurar siempre la lista blanca estricta de algoritmos asimétricos permitidos (`RS256`, `ES256` o `EdDSA`), validar `Issuer`, `Audience`, `Lifetime` y fijar un `ClockSkew` máximo de 30 segundos.

---

## 🍪 3. Blindaje en Navegador, Cookies y Cabeceras Perimetrales

1. **Protección de Tokens de Sesión contra Robo por XSS (`F12` DevTools):**
   - Almacenar tokens de sesión o *refresh tokens* en `localStorage` los deja expuestos a cualquier vulnerabilidad XSS o script de terceros.
   - **Estándar Exigido:** Emitir cookies con las banderas `HttpOnly; Secure; SameSite=Strict; Path=/`.
   - Desactivar siempre la generación de *Source Maps* públicos en compilaciones de producción (`productionBrowserSourceMaps: false`).

2. **Cabeceras de Seguridad Obligatorias en el Reverse Proxy (Nginx / Cloudflare):**
   ```nginx
   add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;
   add_header X-Content-Type-Options "nosniff" always;
   add_header X-Frame-Options "SAMEORIGIN" always;
   add_header Referrer-Policy "strict-origin-when-cross-origin" always;
   add_header Permissions-Policy "camera=(), microphone=(), geolocation=(), payment=()" always;
   ```

3. **Rate Limiting Diferenciado (Anti-Brute-Force & Anti-Noisy-Neighbor):**
   - **Endpoints de Login / 2FA / Recuperación de Contraseña:** Máximo 5 peticiones por minuto por IP/cuenta con bloqueo progresivo.
   - **APIs Operativas por Inquilino:** Cuota dinámica por `TenantId` para impedir que un inquilino degrade el rendimiento de los demás (*Noisy Neighbor*) o exfiltre catálogos masivos.

---

## 🕵️ 4. Prevención de Fuga de Datos (DLP), Logs sin PII y Gestión de Secretos

1. **Sanitización Automática de PII y Credenciales en Logs:**
   - Queda terminantemente prohibido escribir en consola, archivos de log o sistemas APM (Elasticsearch, Loki, Datadog):
     - Contraseñas, hashes de contraseñas, PINs o códigos TOTP 2FA (`[REDACTED]`).
     - Tokens Bearer JWT completos o API Keys.
     - Números completos de tarjetas (PAN / CVV) o documentos de identidad sin enmascarar.
2. **Cero Secretos en el Control de Versiones (Git):**
   - Prohibido incluir contraseñas reales, llaves privadas `.pem`/`.key` o tokens de proveedores cloud en archivos `appsettings.json`, `.env` commiteados, scripts SQL o scripts bash.
   - Todo secreto debe inyectarse por variables de entorno seguras, gestores de secretos (Vault / KMS) o bóvedas locales cifradas con **AES-256-GCM** y derivación de clave **PBKDF2-HMAC-SHA256 (≥ 600,000 iteraciones)** / **Argon2id**.

---

## ⛔ 5. Checklist Obligatorio de Cierre en Ciberseguridad (Quality Gate)

- [ ] ¿Se verificó que ningún endpoint confíe en el `tenantId` o `userId` del body/query sin contrastarlo contra el claim del token JWT?
- [ ] ¿El validador JWT restringe explícitamente `ValidAlgorithms` a algoritmos asimétricos (`RS256`/`ES256`)?
- [ ] ¿Las consultas SQL utilizan exclusivamente parámetros tipados (cero concatenación de strings susceptible a SQL Injection)?
- [ ] ¿Se auditó con `grep` que no se estén introduciendo claves, tokens o contraseñas en texto plano dentro del repositorio?
- [ ] ¿Los logs generados durante las pruebas están libres de contraseñas, tokens y PII en texto claro?

