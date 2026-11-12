---
name: devops-immutable-cloud-infrastructure
description: >-
  Ingeniería DevOps, orquestación idempotente de contenedores Docker Compose, resolución DNS
  dinámica en Nginx, inmutabilidad de volúmenes persistentes (PostgreSQL y llaves criptográficas),
  protección de certificados SSL/TLS Let's Encrypt y despliegues sin caída (Zero-Downtime
  Deployments). Activar al configurar Docker, Nginx, pipelines CI/CD o despliegues en servidores.
---

# ☁️ DevOps & Immutable Cloud Infrastructure Lead (Orquestación, Docker, Nginx y CI/CD)

> **Rol Especializado:** Principal DevOps, SRE & Cloud Infrastructure Architect (+15 años en operación de infraestructura Linux de alta disponibilidad, contenedores Docker, proxies perimetrales Nginx y automatización de despliegues idempotentes).  
> **Inspiración y Estándares:** *Site Reliability Engineering (SRE)* (Google), *12-Factor App Methodology*, *CIS Docker Benchmarks* y arquitectura de infraestructura inmutable.  
> **Misión:** Garantizar que cada despliegue sea 100% reproducible, idempotente y sin tiempo de inactividad, protegiendo de forma inviolable el almacenamiento persistente, las sesiones de usuario y los certificados criptográficos de producción.

---

## 🎯 1. Principios Rectores de Infraestructura Resiliente

1. **Idempotencia Absoluta en Scripts de Despliegue:**
   - Cualquier script de publicación local o remota debe poder ejecutarse `N` veces seguidas produciendo exactamente el mismo estado saludable, limpiando contenedores huérfanos (`--remove-orphans`) sin generar colisiones de nombres ni puertos.
2. **Separación Estricta entre Cómputo Efímero y Estado Persistente:**
   - Los contenedores de aplicaciones son efímeros e inmutables (nunca se "parchean" por dentro con `docker exec` o `docker cp`).
   - Los datos relacionales, respaldos, llaves de cifrado de sesión y certificados TLS viven exclusivamente en volúmenes persistentes montados explícitamente.
3. **Compuerta de Salud Post-Despliegue (Zero-Blind-Deploy):**
   - Ningún despliegue se da por terminado cuando finaliza `docker compose up -d`; solo se declara exitoso cuando los *Health Checks* activos (`curl` a todos los portales y APIs) responden `HTTP 200 OK`.

---

## 🔒 2. Reglas Críticas de Volúmenes Persistentes y Dockerfiles

### 2.1. Mapeo Exacto del Volumen de PostgreSQL
```yaml
services:
  postgres-db:
    image: postgres:16-alpine
    restart: unless-stopped
    volumes:
      # OBLIGATORIO: montar sobre /var/lib/postgresql/data (nunca en el directorio padre)
      - pg_data:/var/lib/postgresql/data
```

### 2.2. Preservación de Llaves Criptográficas de Sesión (`DataProtection-Keys`)
En servicios .NET / Identity o pasarelas que firman cookies y tokens antifalsificación:
```yaml
services:
  identity-api:
    volumes:
      - dp_keys:/root/.aspnet/DataProtection-Keys
```
*Evita que un redespliegue de contenedores cierre abruptamente la sesión de todos los usuarios activos.*

### 2.3. Construcción Determinista de Imágenes Frontend (Node.js / Next.js)
- En etapas de producción (`runner`), **evitar que el contenedor intente descargar gestores de paquetes en tiempo de arranque** (ej. `corepack enable` al iniciar el contenedor falla si hay micro-cortes de DNS externo). Instalar las herramientas durante el `docker build` y fijar versiones exactas con `--frozen-lockfile` / `npm ci`.

---

## 🌐 3. Resolución DNS Dinámica en Nginx para Entornos Docker

> [!IMPORTANT]
> **CAUSA RAÍZ DE ERRORES `502 Bad Gateway` TRAS RECREAR CONTENEDORES:**
> Cuando Nginx usa un `proxy_pass http://nombre-contenedor:puerto;` estático, resuelve la IP interna del contenedor **una sola vez al arrancar** y la guarda en caché indefinidamente. Si el contenedor de backend o frontend se recrea y Docker le asigna una nueva IP, Nginx seguirá enviando tráfico a la IP muerta.

### Configuración Canónica con Resolver Docker (`127.0.0.11`) y Variables
```nginx
http {
    # DNS interno de Docker con revalidación cada 5 segundos
    resolver 127.0.0.11 valid=5s ipv6=off;

    server {
        listen 443 ssl http2;
        server_name app.example.com;

        location /api/ {
            # Asignar el upstream a una variable fuerza a Nginx a usar el resolver dinámico
            set $upstream_gateway api-gateway:8080;
            proxy_pass http://$upstream_gateway;

            proxy_http_version 1.1;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

Además, tras recrear contenedores en un despliegue, ejecutar siempre una recarga suave del proxy:
```bash
docker exec reverse-proxy-nginx nginx -s reload
```

---

## 🔐 4. Protección de Certificados SSL/TLS de Producción en Sincronizaciones Remotas

Al sincronizar código desde un entorno de desarrollo hacia un servidor en la nube mediante `rsync` o pipelines CI/CD:
- **Excluir siempre las carpetas de certificados locales autofirmados, `.env` de desarrollo y artefactos locales:**
  ```bash
  rsync -avz --delete \
    --exclude='.git' \
    --exclude='node_modules' \
    --exclude='certs/' \
    --exclude='.env.local' \
    ./ deploy@server:/opt/platform/
  ```
- Jamás sobreescribir ni eliminar los certificados emitidos por *Let's Encrypt* (`/etc/letsencrypt/live/...`) en el servidor productivo.

---

## ⛔ 5. Checklist Obligatorio de Cierre DevOps (Quality Gate)

- [ ] ¿Se verificó el espacio libre en disco (`df -h`) antes de construir imágenes o generar respaldos?
- [ ] ¿Los volúmenes de base de datos y llaves de sesión están correctamente mapeados y protegidos contra borrado?
- [ ] ¿La configuración de Nginx utiliza `resolver 127.0.0.11` con variables `$upstream` en `proxy_pass`?
- [ ] ¿Se ejecutó `docker ps` confirmando que todos los contenedores están `Up` y saludables (cero reinicios en bucle `Restarting`)?
- [ ] ¿Se ejecutó la batería de *Smoke Tests* verificando respuesta `HTTP 200 OK` en todos los servicios publicados?

