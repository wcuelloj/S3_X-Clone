---
name: defining-tdd1
description: Delimits TDD for this repo: map existing code first, define use cases, derive tests before implementation, and verify behavior. Use when starting features, bugfixes, or refactors where test-first order and explicit acceptance criteria are required, or when the user asks for TDD, use cases, or "tests before code".
---

# Defining TDD (delimitación)

## Propósito

Este skill fija **límites claros** del proceso TDD en el monorepo y obliga a:

1. **Entender el código base** antes de diseñar pruebas.
2. **Definir casos de uso** con criterios de aceptación observables.
3. **Escribir y acordar tests primero** (RED), luego implementar lo mínimo (GREEN).
4. **Verificar** con comandos del proyecto y un reporte breve.

**Regla de prioridad:** si hay conflicto entre velocidad y orden, gana **test definido antes que implementación**. Sin excepción salvo que se documente un bloqueo y una alternativa de verificación.

## Qué entra y qué queda fuera

| Dentro del TDD aquí | Fuera (no llamarlo TDD) |
|---------------------|-------------------------|
| Comportamiento observable (entrada/salida, errores, UI estable) | Solo manual "probar en el navegador" sin test automatizable |
| Tests unitarios/integración con Vitest en workspaces | E2E completo sin casos de uso previos (planificar aparte) |
| Un caso de uso → al menos un test que lo demuestre | Implementar primero y "añadir tests después" sin RED previo |

## Cuándo aplicar

- Nueva funcionalidad o cambio de comportamiento.
- Bugfix (primero test que reproduzca el fallo).
- Refactor con garantía de no romper contratos.
- Peticiones explícitas: "TDD", "casos de uso", "test first", "definir pruebas antes".

## Paso 0: Entender el código base

Antes de escribir tests, **ubicar dónde vive el comportamiento** y qué ya está cubierto.

1. **Ámbito del cambio**  
   - `app`: UI React (Vite), `jsdom` en tests.  
   - `api`: backend TypeScript.  
   - `packages/shared`: utilidades compartidas.

2. **Exploración mínima**  
   - Buscar el módulo o feature por nombre (rutas, hooks, servicios).  
   - Leer el código que ya maneja el flujo (entrada → lógica → salida / error).  
   - Si existe, leer `*.test.ts` / `*.test.tsx` **co-localizados** (`src/foo/bar.test.ts` junto a `bar.ts`).

3. **Contrato actual**  
   - Anotar qué es público (exports, props, endpoints) vs interno.  
   - Los tests deben preferir **comportamiento observable**, no detalles de implementación.

4. **Riesgo**  
   - Cambios en límites (API, storage, auth): anotar dependencias para tests de integración o mocks acotados.

Sin este paso, los casos de uso pueden desalinearse del diseño real del repo.

## Paso 1: Definir casos de uso

Por cada caso de uso, dejar explícito:

- **Actor / contexto**
- **Disparador** (acción o evento)
- **Resultado esperado** (observable)
- **Bordes** (vacío, límites, concurrencia si aplica)
- **Errores** (mensaje, código, estado)

Plantilla:

```md
Caso de uso: <nombre corto>
- Given: <estado inicial>
- When: <acción>
- Then: <resultado / efecto observable>
- Errores: <si aplica>
```

Si algo es ambiguo, **preguntar antes** de codificar tests o producción.

## Paso 2: De casos de uso a tests (obligatorio antes de implementar)

Orden estricto:

1. Listar casos de prueba **mapeados 1:1** a casos de uso cuando sea posible.
2. Por caso de uso: al menos **happy path**, un **borde**, y **error** si aplica.
3. **Escribir tests primero** que fallen (RED) por la razón correcta.
4. Solo entonces implementar el mínimo para pasar (GREEN).

Nombres de tests orientados a comportamiento (ej. `returns empty state when no items`), no a helpers internos.

Patrón dentro del test: Given / When / Then; assertions sobre resultado observable.

## Paso 3: Verificar

1. Test puntual del área (confirmar RED → GREEN).
2. Regresión: `npm run test` cuando el cambio toque contratos compartidos o flujos críticos.
3. Calidad estática: `npm run lint` si hubo cambios de código relevantes.

Comandos útiles por workspace: `npm run test -w app`, `npm run test -w api`, `npm run test -w packages/shared`. Watch: `npm run test:watch`.

Reporte mínimo sugerido:

```md
Verificación TDD
- Casos de uso cubiertos: <n>/<total>
- Tests nuevos/actualizados: <lista>
- Comandos: <lista>
- Resultado: pass/fail + nota breve
- Riesgos o gaps: <bullets>
```

## Checklist (copiar y marcar)

```md
TDD1 Progress
- [ ] Código base localizado y contrato entendido
- [ ] Casos de uso escritos (Given/When/Then + errores)
- [ ] Plan de tests alineado a casos de uso
- [ ] Tests escritos primero y RED verificado
- [ ] Implementación mínima (GREEN)
- [ ] Refactor solo con tests en verde (si aplica)
- [ ] Verificación y reporte
```

## Bloqueos

Si no es posible automatizar (p. ej. hardware, terceros sin sandbox), documentar **por qué**, proponer verificación alternativa (contract test, adapter mock, checklist manual mínima) y no presentar el flujo como TDD completo.

## Relación con otros skills

- Para exploración profunda de UI en navegador, puede complementarse con el skill de ejecución en browser.
- El skill `defining-tdd` del mismo repo amplía detalles de estructura de tests y ejemplos; este skill (`defining-tdd1`) prioriza **delimitación**, **mapa del código base** y **orden test-first**.
