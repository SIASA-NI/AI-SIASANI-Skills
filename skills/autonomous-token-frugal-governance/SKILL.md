---
name: autonomous-token-frugal-governance
description: Gobernanza de ejecución autónoma acotada (Bounded Assume YES), ahorro extremo de tokens (Token-Frugal Context Engineering), consenso multidisciplinario de skills, modo diagnóstico de solo lectura y paradas de seguridad obligatorias (Safety Stop HITL) ante bases de datos o comandos ambiguos.
---

# ⚡ Autonomous Execution, Token-Frugal Context & Safety Stop Governance

## 1. Propósito y Mandato Operativo
Este skill codifica las **Directivas Nucleares de Gobernanza Operativa y Eficiencia de Contexto** para agentes de IA de ingeniería de software. Su objetivo es lograr **máxima velocidad de ejecución autónoma** en tareas seguras sin interrumpir al usuario, combinada con **paradas de seguridad innegociables (Safety Stop)** ante riesgos de persistencia y **ahorro extremo de tokens** para evitar la degradación de la ventana de contexto.

---

## 2. Ejecución Autónoma Acotada (*Bounded Assume YES*)

### 2.1. Dónde APLICAR `Assume YES` (100% Autonomía sin Preguntas)
- Programación de componentes Frontend y Backend, refactorización limpia, corrección de errores de compilación (`tsc`, `dotnet build`, `cargo check`, `pytest`), ajustes de estilos UI/UX, generación de tests automatizados y sincronización documental.
- Ante márgenes técnicos no destructivos, el agente **adopta inmediatamente el estándar arquitectónico más robusto del repositorio** (DDD, Clean Architecture, tipado estricto, OAuth2/JWT RS256) y completa el ciclo `Inspección ➔ Diseño ➔ Implementación ➔ Pruebas ➔ Verificación` sin pausas ni preguntas triviales (*"¿Deseas que continúe?"*).

### 2.2. Perímetro Inviolable donde está PROHIBIDO `Assume YES` (*Hard Safety Stop — HITL*)
El agente **DEBE DETENERSE INMEDIATAMENTE Y CONSULTAR AL USUARIO** en los siguientes 4 escenarios:
1. **Mutación o Alteración de Base de Datos Viva:** Ejecución de `DROP`, `TRUNCATE`, `DELETE` masivo, `ALTER TABLE` destructivo o migraciones sobre bases de datos con datos reales.
2. **Modificación de Credenciales, Usuarios o Llaves Criptográficas:** Alteración de contraseñas, secretos `.env`, certificados SSL/TLS Let's Encrypt de producción o llaves privadas RSA/JWKS.
3. **Comandos Destructivos de Infraestructura:** `rm -rf`, `docker volume rm`, `docker system prune --volumes`, `git reset --hard` o `git push --force`.
4. **Ambigüedad Semántica en Verbos Polisémicos:** Cuando el usuario utiliza términos como *"publicar"*, *"actualizar"*, *"subir"* o *"limpiar"* sin especificar si se refiere a:
   - Control de versiones (`git push` a GitHub),
   - Despliegue de contenedores en servidor Cloud/Producción, o
   - Migración de base de datos.

---

## 3. Regla de Respaldo Pre-Mutación (*Pre-Mutation Backup Rule*)
Incluso cuando el usuario ha autorizado explícitamente una migración o mantenimiento sobre una base de datos:
1. **Ejecutar primero un snapshot físico (`pg_dump -Fc`)** de la base de datos activa.
2. **Verificar el tamaño en bytes (> 0)** y la integridad del archivo de respaldo.
3. **Monitorear el espacio libre en disco (`df -h`)** garantizando un margen mínimo de seguridad (`≥ 15 GB` libres) antes de proceder.

---

## 4. Modo Diagnóstico de Solo Lectura (*Read-Only Diagnostic Directive*)
Cuando el usuario solicita un **diagnóstico, análisis, auditoría o revisión de un problema**:
- Queda **TERMINANTEMENTE PROHIBIDO** modificar archivos de código fuente o reiniciar servicios de forma autónoma durante el diagnóstico.
- El reporte debe estructurarse obligatoriamente en tres bloques:
  1. **Qué pasa:** Descripción objetiva del síntoma con evidencia cruda (logs, estado HTTP, métricas).
  2. **Por qué pasa:** Trazabilidad exacta de la causa raíz (archivo, línea y condición lógica).
  3. **Qué se puede hacer para corregirlo:** Plan de acción concreto paso a paso, esperando la instrucción explícita del usuario antes de alterar producción.

---

## 5. Ingeniería de Contexto y Ahorro Extremo de Tokens (*Token-Frugal Execution*)
1. **Búsqueda Quirúrgica antes de Lectura:** Utilizar siempre `ripgrep` (`rg`) o `grep` con filtros de ruta y tipo de archivo antes de abrir archivos completos.
2. **Lecturas Acotadas por Rango de Líneas:** Al inspeccionar archivos grandes, leer únicamente rangos específicos (`StartLine`–`EndLine` de 50 a 120 líneas) alrededor del símbolo de interés; prohibido volcar archivos de miles de líneas completos en el contexto.
3. **Ediciones Atómicas de Mínima Invasión:** Realizar reemplazos quirúrgicos sobre el bloque exacto afectado sin reescribir ni reformatear el resto del archivo.
4. **Truncado de Salidas de Terminal:** Filtrar salidas de compilación, logs de contenedores (`docker logs --tail 50`) y consultas SQL (`LIMIT 20`) para no contaminar la memoria de trabajo del agente.
5. **Modularización Progresiva (`SKILL.md`):** Mantener las reglas específicas de cada dominio en su respectivo `SKILL.md` bajo demanda en lugar de sobrecargar el archivo raíz `GEMINI.md` / `CLAUDE.md`.

