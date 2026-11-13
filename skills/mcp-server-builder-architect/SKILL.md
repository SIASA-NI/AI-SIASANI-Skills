---
name: mcp-server-builder-architect
description: Arquitectura, diseño e implementación de servidores Model Context Protocol (MCP) de grado productivo en TypeScript y Python. Cubre JSON-RPC 2.0, transportes Stdio y Streamable HTTP/SSE, diseño ergonómico de Tools/Resources/Prompts y seguridad OAuth2.
---

# 🔌 Model Context Protocol (MCP) Server Builder & Architect

## 1. Propósito y Alcance
Este skill capacita al agente para **diseñar, construir, depurar y empaquetar servidores Model Context Protocol (MCP)** de grado empresarial (basado en la especificación oficial `modelcontextprotocol.io` y `anthropics/skills/mcp-builder`), permitiendo que agentes como Google Antigravity, Claude Code, Cursor y Copilot se conecten de forma segura y determinista a bases de datos, APIs internas, ERPs y sistemas operativos.

---

## 2. Los 3 Primitivos Fundamentales de un Servidor MCP
Todo servidor MCP debe separar limpiamente sus capacidades en tres primitivos:
1. **Tools (Controladas por el Modelo):** Funciones ejecutables con esquemas JSON (`inputSchema` con Zod o Pydantic) que realizan cálculos, consultas o acciones con efectos secundarios.
2. **Resources (Controlados por la Aplicación/Usuario):** Datos estructurados de solo lectura identificados por URIs canónicas (ej. `postgres://coredb/tables/UsrUsuarios/schema`, `docs://manuales/finanzas.md`) que el cliente inyecta como contexto sin gastar llamadas a herramientas.
3. **Prompts (Controlados por el Usuario):** Plantillas reutilizables e interactivas (slash commands) con argumentos tipados que estandarizan flujos de trabajo complejos.

---

## 3. Reglas de Oro para el Diseño de Herramientas (`MCP Tools`)

### 3.1. Ergonomía para Agentes (Diseño Orientado a Flujos, no a CRUD Crudo)
- **No exponer 1:1 endpoints REST atómicos sin contexto:** En lugar de obligar al agente a invocar 5 herramientas secuenciales (`get_user_id` ➔ `get_account_id` ➔ `get_invoices` ➔ `get_payments`), diseñar herramientas consolidadas orientadas a intención (ej. `get_customer_billing_summary`) que acepten identificadores humanos (email o código) y devuelvan el contexto cohesionado.
- **Nombres Semánticos con Prefijo de Dominio:** Usar `snake_case` con prefijo del servicio: `github_create_pull_request`, `siasani_erp_audit_ledger`, `postgres_explain_query`.

### 3.2. Protección del Presupuesto de Tokens en las Respuestas
- **Paginación y Límite Estricto por Defecto:** Toda herramienta que devuelva listas debe incluir `limit` (por defecto `20`, máximo `100`) y `cursor` / `offset`, informando `total_count` y `has_more`.
- **Truncado Inteligente y Mensajes Accionables:** Jamás devolver payloads JSON crudos de varios megabytes. Filtrar campos irrelevantes y, cuando un error ocurra, devolver un mensaje con `isError: true` que le indique explícitamente al agente **cómo corregir los parámetros** en el siguiente intento.

### 3.3. Anotaciones de Comportamiento (`ToolAnnotations`)
Declarar siempre las propiedades semánticas de cada herramienta:
- `readOnlyHint: true | false` (indica si la herramienta solo lee o modifica estado).
- `destructiveHint: true | false` (alerta al cliente para exigir confirmación HITL antes de borrar o mutar).
- `idempotentHint: true | false` (indica si llamarla repetidamente con los mismos argumentos es seguro).

---

## 4. Transportes, Seguridad y Verificación
1. **Transportes Estándar:**
   - **`stdio`:** Para servidores locales ejecutados como subprocesos. **REGLA CRÍTICA:** En transporte `stdio`, queda **TERMINANTEMENTE PROHIBIDO** escribir logs en `stdout` (`console.log` / `print`), ya que corrompe los frames JSON-RPC 2.0. Todo log de diagnóstico debe enviarse exclusivamente a `stderr` (`console.error` / `sys.stderr`).
   - **`Streamable HTTP` / `SSE`:** Para servidores remotos multi-tenant con autenticación `Authorization: Bearer <JWT>` (OAuth 2.1) y aislamiento por `TenantId`.
2. **Validación Empírica:**
   - Probar todo servidor MCP con el **MCP Inspector** (`npx @modelcontextprotocol/inspector`) o enviando tramas JSON-RPC (`initialize`, `tools/list`, `tools/call`) por tubería y verificando respuestas válidas antes de registrarlo en `mcp_config.json`.

