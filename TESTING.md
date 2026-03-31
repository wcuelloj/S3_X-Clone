# Estrategia de tests unitarios

## Por workspace

| Workspace | Runner | Entorno / notas |
|-----------|--------|-----------------|
| `app` | Vitest | `jsdom`; `@testing-library/react` + `app/src/test/setup.ts` |
| `api` | Vitest | Node (sin DOM) |
| `packages/shared` | Vitest | Node |

## Convenciones

- Tests **co-localizados** con el código: `*.test.ts` o `*.test.tsx` junto al módulo (ej. `TweetCard.tsx` → `TweetCard.test.tsx`).
- Nombres de tests orientados a **comportamiento observable**, no a detalles de implementación.
- Estructura **Given / When / Then** dentro de cada caso cuando ayude a la lectura.

## Flujo TDD acordado

1. Definir casos de uso.
2. Convertir casos de uso a casos de prueba.
3. Escribir tests primero y ejecutar para ver falla (RED).
4. Implementar el cambio mínimo (GREEN).
5. Refactorizar solo si aplica.
6. Ejecutar tests de verificación.

## Ejecución

- Toda la suite del monorepo:

```bash
npm run test
```

- Modo watch en todo el monorepo:

```bash
npm run test:watch
```

- Por workspace:

```bash
npm run test -w app
npm run test -w api
npm run test -w packages/shared
```
