---
name: git-worktree-release-engineering
description: Ingeniería de control de versiones avanzado con Git, aislamiento paralelo con Git Worktrees, Conventional Commits semánticos, versionado SemVer automatizado, Changelogs auditables, resolución quirúrgica de conflictos y protección de ramas.
---

# 🌿 Git Worktrees, Semantic Release Engineering & Branch Governance

## 1. Propósito y Alcance
Este skill dota al agente de la maestría en **Ingeniería de Versionado, Trabajo Paralelo con Git Worktrees y Gestión de Lanzamientos (Release Engineering)** (inspirado en `obra/superpowers/using-git-worktrees` y estándares de monorepos empresariales).

---

## 2. Trabajo en Paralelo sin Contaminación con `Git Worktrees`
Cuando un agente o enjambre de subagentes (*Swarm*) necesita desarrollar una funcionalidad aislada, ejecutar una refactorización profunda o validar un hotfix urgente sin alterar el directorio de trabajo principal:
1. **Creación de un Worktree Aislado:**
   ```bash
   git worktree add ../wt-feature-nombre -b feat/nombre-modulo main
   ```
   Esto monta una copia de trabajo independiente compartiendo la misma base de objetos `.git` en milisegundos, sin duplicar el historial ni romper procesos en ejecución en la carpeta principal.
2. **Limpieza Segura Post-Merge:**
   Una vez verificados los tests e integrado el cambio en `main`, remover limpiamente el worktree:
   ```bash
   git worktree remove ../wt-feature-nombre
   git worktree prune
   ```

---

## 3. Estándar de `Conventional Commits` y Atomicidad
1. **Un Commit = Una Unidad Lógica Compilable y Verificada:**
   - Prohibido hacer commits gigantescos de tipo `"wip"` o `"varios cambios"` que mezclen refactorizaciones cosméticas, cambios de esquema SQL y nuevas funcionalidades.
2. **Formato Semántico Estricto:**
   ```text
   <tipo>(<alcance>): <descripción imperativa concisa en minúsculas>

   [Cuerpo opcional explicando el QUÉ y el POR QUÉ, no el cómo obvio del diff]
   [Footer opcional: Refs #123, BREAKING CHANGE: detalle]
   ```
   - **Tipos permitidos:** `feat` (nueva funcionalidad ➔ sube `MINOR`), `fix` (corrección de bug ➔ sube `PATCH`), `perf` (mejora de rendimiento), `refactor` (cambio de código sin alterar comportamiento), `security` (endurecimiento de seguridad), `docs` (documentación), `test` (pruebas), `ci` / `chore` (infraestructura/build).

---

## 4. Seguridad en Operaciones Git y Protección de Historial
- **Higiene Pre-Commit Obligatoria (`git status` & `git diff --staged`):**
  - Antes de cada `git commit`, inspeccionar qué archivos están en el área de preparación (*staging*) para **garantizar que jamás se suban archivos `.env`, llaves privadas `.pem/.key`, dumps `.sql/.backup`, binarios temporales ni carpetas `node_modules/` o `bin/obj/`**.
- **Prohibición de Reescritura Destructiva en Ramas Compartidas:**
  - Queda **TERMINANTEMENTE PROHIBIDO** ejecutar `git push --force`, `git reset --hard` sobre commits ya publicados o eliminar ramas productivas sin confirmación humana explícita. Si se requiere revertir un commit en `main`, utilizar siempre `git revert <sha>` para preservar la trazabilidad histórica completa.

