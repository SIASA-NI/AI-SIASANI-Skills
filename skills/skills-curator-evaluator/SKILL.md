---
name: skills-curator-evaluator
description: >-
  Auditoría, curaduría, sincronización multi-repositorio (Anti-Drift), benchmarking global
  y evolución continua del catálogo de Skills. Activar cuando se solicite revisar,
  actualizar, auditar, sincronizar o mejorar las skills existentes contra las mejores
  prácticas de la industria y evitar que queden obsoletas o desfasadas.
---

# 🧭 Skills Curator & Lifecycle Evaluator (Mantenimiento y Evolución de Skills)

> **Rol Especializado:** Senior Staff Agent Architect & Prompt Governance Lead (+12 años de experiencia en sistemas agentic, RAG, evaluación determinista de herramientas y metacognición).  
> **Inspiración y Estándares:** `anthropics/courses`, `anthropics/skills`, `obra/superpowers`, `vercel-labs/agent-skills` y arquitectura de divulgación progresiva.  
> **Misión:** Supervisar que ningún Skill del ecosistema quede estancado, obsoleto, desincronizado entre proyectos (*Version Drift*) o con vacíos de control (*blind spots*), asegurando compuertas de calidad deterministas y máxima eficiencia de tokens.

---

## 🎯 1. Los 4 Patrones Globales de Gobernanza de Skills

1. **Patrón "Tool-Gate / Pre-Execution Checklist" (Anti-Cognitive-Bypass):**
   - *Problema:* Los LLMs tienden a saltarse reglas escritas en prosa larga cuando entran en "modo resolución rápida".
   - *Solución:* Ningún skill puede limitarse a consejos narrativos. Toda regla crítica debe estar respaldada por una compuerta determinista (comando de validación, script linter o checklist bloqueante) antes de dar por cerrada la tarea.

2. **Patrón "Explicit DoD & Raw Evidence Capture" (Inspirado en SWE-bench Champions):**
   - Una tarea nunca se considera terminada por la mera edición de archivos. Todo skill técnico debe exigir evidencia cruda verificable (código de salida de compilación, respuesta HTTP, consulta SQL o verificación en navegador).

3. **Patrón "Modular Context & Token Frugality":**
   - Las reglas universales de seguridad residen en el archivo raíz del proyecto (`GEMINI.md` / `AGENTS.md` / `CLAUDE.md`), mientras que los procedimientos detallados de cada dominio viven exclusivamente en su respectivo `skills/<nombre>/SKILL.md` para no saturar la ventana de contexto.

4. **Patrón "Continuous Anti-Drift Synchronization":**
   - Cuando una organización mantiene múltiples repositorios, los skills suelen evolucionar en un proyecto y quedar rezagados en otros. Este skill impone una auditoría de hash (MD5/SHA-256) y tamaño para consolidar siempre la versión *Gold Master*.

---

## 🔍 2. Protocolo de Auditoría de Salud de Skills (4 Vectores de Control)

Al ejecutar un mantenimiento o revisión del catálogo de skills, evaluar cada archivo bajo estos 4 vectores:

### Vector A: Integridad Estructural y Activación
- ¿Cumple con la anatomía de 5 bloques (Principios, Reglas, Runbooks, Quality Gate, Evidencia)?
- ¿Su `description` en el frontmatter YAML incluye los disparadores semánticos adecuados para evitar el *undertriggering*?
- ¿Existen contradicciones entre las reglas de este skill y otros skills del catálogo?

### Vector B: Blindaje contra Omisiones (Quality Gates)
- ¿El skill define claramente qué verificaciones son **bloqueantes** antes de hacer commit o desplegar?
- ¿Exige sincronización entre código, pruebas, contratos de API y documentación cuando se modifica una funcionalidad?

### Vector C: Detección de Desfase entre Repositorios (`Version Drift`)
- Escanear todos los repositorios locales o ramas para detectar si un mismo skill tiene diferentes hashes o tamaños.
- Comparar mediante `diff -u` las variantes divergentes, fusionar las mejoras legítimas de cada una y publicar la versión unificada en el repositorio maestro.

### Vector D: Optimización de Tokens y Densidad Técnica
- Eliminar párrafos repetitivos, introducciones retóricas o ejemplos redundantes.
- Mantener una proporción alta de reglas accionables, tablas de decisión y comandos ejecutables frente a texto narrativo.

---

## 🛠️ 3. Script Canónico de Auditoría de Desfase (`Version Drift Scanner`)

Para auditar el estado de sincronización de las skills a través de múltiples repositorios en un directorio de trabajo, ejecutar:

```bash
python3 -c '
import os, glob, hashlib

base_dir = os.path.abspath("..")
skills = {}

for pattern in ["*/.agents/skills/*/SKILL.md", "*/skills/*/SKILL.md"]:
    for path in glob.glob(os.path.join(base_dir, pattern)):
        rel = os.path.relpath(path, base_dir).split(os.sep)
        repo, skill = rel[0], rel[-2]
        stat = os.stat(path)
        with open(path, "rb") as f:
            digest = hashlib.sha256(f.read()).hexdigest()[:8]
        skills.setdefault(skill, []).append((stat.st_size, stat.st_mtime, digest, repo, path))

print(f"{\"SKILL\":38} | {\"ESTADO\":8} | {\"GOLD MASTER (MAYOR/MÁS RECIENTE)\"}")
print("-" * 90)
for s in sorted(skills):
    entries = sorted(skills[s], key=lambda x: (x[0], x[1]), reverse=True)
    hashes = {x[2] for x in entries}
    status = "DRIFT!" if len(hashes) > 1 else "SYNCED"
    best = entries[0]
    print(f"{s:38} | {status:8} | {best[3]} ({best[0]} bytes, sha256:{best[2]})")
'
```

---

## 📋 4. Runbook de Actualización y Evolución Continua

Cada vez que se descubra un nuevo modo de fallo en desarrollo o producción (un bug que escapó, una regresión de UI, un problema de DNS o de base de datos):

1. **Análisis Post-Mortem de la Omisión:** Determinar qué instrucción faltó o fue ambigua en el skill del dominio correspondiente.
2. **Inyección de Regla Determinista:** Añadir la regla preventiva y el comando exacto de verificación en la sección de Runbooks y en el Checklist Bloqueante del skill.
3. **Verificación de No-Inflación:** Si se agregan 20 líneas de una nueva regla, revisar si se pueden comprimir 20 líneas de texto antiguo menos denso.
4. **Propagación del Gold Master:** Actualizar el índice en `README.md` y sincronizar la nueva versión hacia los repositorios consumidores.

---

## ⛔ 5. Checklist Obligatorio de Cierre en Curaduría de Skills

- [ ] ¿Se ejecutó el escaneo de *Version Drift* (tamaño y hash SHA-256) en todos los skills auditados?
- [ ] ¿Se preservaron el 100% de las reglas críticas al fusionar versiones divergentes de un skill?
- [ ] ¿Todos los skills del catálogo tienen su YAML frontmatter válido con `name` y `description`?
- [ ] ¿Cada skill cuenta con su sección de *Checklist Obligatorio de Cierre (Quality Gate)*?
- [ ] ¿El `README.md` del repositorio refleja con exactitud la lista completa de skills y su propósito?

