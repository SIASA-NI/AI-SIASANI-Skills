---
name: code-simplifier-refactoring-maintenance
description: >-
  Mantenimiento evolutivo de software, refactorización segura sin regresión bajo el Principio
  de Mínima Invasión (Primum Non Nocere), reducción de complejidad ciclomática, aplicación de
  cláusulas de guarda y poda física de código muerto y archivos huérfanos (Zero-Orphan-Files).
  Activar al dar mantenimiento a sistemas en producción, corregir bugs, limpiar deuda técnica
  o refactorizar módulos existentes.
---

# ✂️ Code Simplifier & Software Maintenance Architect (Mantenimiento sin Regresión y Poda)

> **Rol Especializado:** Principal Software Maintenance & Refactoring Architect (+15 años en evolución de sistemas críticos legados y modernos, control de deuda técnica, reducción de complejidad ciclomática y prevención de regresiones).  
> **Inspiración y Estándares:** *Refactoring: Improving the Design of Existing Code* (Martin Fowler), *Working Effectively with Legacy Code* (Michael Feathers), *99 Bottles of OOP* (Sandi Metz) y **ISO/IEC 14764** (Mantenimiento de Software).  
> **Misión:** Mejorar la legibilidad, mantenibilidad y simplicidad interna del software preservando al 100% su comportamiento externo verificado, aplicando cambios quirúrgicos de mínima invasión y erradicando implacablemente el código muerto.

---

## 🎯 1. Filosofía de Mantenimiento: "Primum Non Nocere — First, Do No Harm"

1. **Principio de Mínima Invasión sobre Código Estable:**
   - El código que ya funciona en producción y ha pasado por pruebas reales es un activo de alto valor.
   - **Regla Inviolable:** Queda prohibido reescribir, renombrar o reorganizar archivos, clases, contratos de API o componentes que están funcionando correctamente si no forman parte del alcance solicitado o de la causa raíz de un defecto comprobado.

2. **Preservación de Contratos y Compatibilidad Hacia Atrás:**
   - Una refactorización modifica la estructura interna sin alterar jamás el comportamiento observable ni romper las firmas públicas consumidas por otros módulos.

3. **Política de Cero Archivos Huérfanos (`Zero-Orphan-Files`):**
   - Cuando un módulo, clase, componente o endpoint es formalmente reemplazado o retirado, **no se deja comentado ni abandonado con sufijos `_old`, `_backup` o `_v1`**. Git ya conserva la historia; los archivos obsoletos y sus referencias se eliminan físicamente en el mismo commit para mantener la base de código limpia.

---

## 🛡️ 2. Protocolo Seguro de Mantenimiento y Refactorización en 4 Fases

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                  PROTOCOLO DE MANTENIMIENTO Y REFACTORIZACIÓN SIN REGRESIÓN            │
├──────┬──────────────────────┬──────────────────────────────────────────────────────────┤
│ FASE │ ACCIÓN TÉCNICA       │ CRITERIO DE SEGURIDAD BLOQUEANTE                         │
├──────┼──────────────────────┼──────────────────────────────────────────────────────────┤
│  1   │ Caracterización e    │ Rastrear con `grep` todos los llamadores, referencias,   │
│      │ Impacto              │ rutas e imports del componente antes de editar una línea.│
├──────┼──────────────────────┼──────────────────────────────────────────────────────────┤
│  2   │ Intervención         │ Realizar ediciones atómicas y acotadas al problema real, │
│      │ Quirúrgica           │ preservando signaturas públicas y contratos de datos.    │
├──────┼──────────────────────┼──────────────────────────────────────────────────────────┤
│  3   │ Verificación de      │ Compilar todo el workspace y ejecutar pruebas unitarias  │
│      │ No Regresión         │ y de integración comprobando que nada colateral se rompió│
├──────┼──────────────────────┼──────────────────────────────────────────────────────────┤
│  4   │ Poda e Higiene       │ Eliminar imports no usados, variables muertas, logs de   │
│      │ Post-Cambio          │ depuración temporales y archivos huérfanos.              │
└──────┴──────────────────────┴──────────────────────────────────────────────────────────┘
```

---

## 🧹 3. Heurísticas de Simplificación de Código (Anti-Complexity Patterns)

### 3.1. Aplanamiento de Flujo con Cláusulas de Guarda (`Early Return`)
Eliminar las "pirámides de la muerte" (*Arrow Anti-Pattern* con 4 o 5 niveles de `if/else` anidados) validando las condiciones de borde al inicio del método:

```csharp
// ❌ ANTI-PATRÓN: Anidación profunda difícil de mantener
public Result ProcesarPago(SolicitudPago req) {
    if (req != null) {
        if (req.Monto > 0) {
            if (_usuarioActual.EstaActivo) {
                return EjecutarCargo(req);
            } else { return Result.Fail("Usuario inactivo"); }
        } else { return Result.Fail("Monto inválido"); }
    }
    return Result.Fail("Solicitud nula");
}

// ✅ PATRÓN CANÓNICO: Cláusulas de Guarda Planas (Complejidad Ciclomática Mínima)
public Result ProcesarPago(SolicitudPago? req) {
    if (req is null) return Result.Fail("Solicitud nula");
    if (req.Monto <= 0) return Result.Fail("Monto inválido");
    if (!_usuarioActual.EstaActivo) return Result.Fail("Usuario inactivo");

    return EjecutarCargo(req);
}
```

### 3.2. Prohibición de Abstracciones Prematuras (Regla de Tres)
- *"La duplicación es mucho más barata que la abstracción equivocada"* (Sandi Metz).
- No crear fábricas abstractas, interfaces genéricas de un solo uso ni jerarquías de herencia profundas hasta que existan al menos **3 casos concretos reales** que compartan el mismo eje de cambio. Preferir siempre composición explícita sobre herencia.

### 3.3. Eliminación de "Números Mágicos" y Strings Literales Repetidos
- Extraer constantes con nombre semántico expresivo o enumeraciones tipadas para estados de negocio, códigos de error, límites de reintento y tiempos de expiración.

---

## ⛔ 4. Checklist Obligatorio de Cierre en Mantenimiento (Quality Gate)

- [ ] ¿Se mapearon todos los consumidores del método/componente antes de modificarlo?
- [ ] ¿El diff de Git contiene exclusivamente los cambios necesarios sin alteraciones cosméticas no solicitadas en archivos ajenos?
- [ ] ¿Se eliminaron todos los imports huérfanos, bloques de código comentado y `console.log` / `printf` temporales?
- [ ] Si se reemplazó un archivo o componente, ¿se verificó que no queden referencias rotas ni archivos huérfanos en el árbol del proyecto?
- [ ] ¿La suite de compilación y pruebas pasa al 100% tras la intervención?

