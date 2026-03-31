# 10X Clone — scaffolding

Monorepo **npm workspaces** para un clon de X (Twitter):

| Workspace | Rol |
|-----------|-----|
| `app` | React + Vite + TypeScript + Tailwind; cliente Supabase |
| `api` | API Node + TypeScript (dev con `tsx watch`) |
| `packages/shared` | Código compartido (`@10x/shared`), consumido por `app` (y por `api` cuando haga falta) |

Fuera de workspaces: **`supabase/`** (CLI local, migraciones, `config.toml`).

## Requisitos

- Node.js `>=20` (includes `npm`). Si acabas de instalar Node en Windows, **cierra y vuelve a abrir** la terminal para que se actualice el `PATH`.
- npm
- [Supabase CLI](https://supabase.com/docs/guides/cli) (para `supabase start`, migraciones y `npm run supabase:gen`). El paquete `supabase` está en `devDependencies` del root para ejecutar la CLI vía `npx` / `node_modules/.bin`.

## Setup

```bash
npm install
```

Copia `.env.example` a `app/.env` y ajusta `VITE_*` según tu proyecto Supabase.

## Comandos (desde la raíz)

| Comando | Descripción |
|---------|-------------|
| `npm run dev` | Arranca `dev` en cada workspace que lo defina (p. ej. Vite + API) |
| `npm run build` | Build en todos los workspaces |
| `npm run test` | Vitest en **todo** el monorepo |
| `npm run test:watch` | Vitest en modo watch en todos los workspaces |
| `npm run lint` | Typecheck (`tsc --noEmit`) por workspace |
| `npm run supabase:gen` | Regenera `app/src/types/database.ts` desde el esquema local (`supabase gen types typescript --local`). Requiere instancia local coherente con las migraciones |
| `npm run supabase:cli -- <args>` | Ejecuta la CLI de Supabase del repo (ej. `npm run supabase:cli -- status`). En Windows usa `npm.cmd` si falla la política de ejecución de PowerShell |

Tests por paquete: `npm run test -w app`, `npm run test -w api`, `npm run test -w @10x/shared`.

## Estructura

```text
.
├── app/                 # Frontend Vite (`@10x/app`)
├── api/                 # Backend (`@10x/api`)
├── packages/
│   └── shared/          # `@10x/shared`
├── supabase/            # Migraciones y config Supabase
├── package.json         # workspaces + scripts globales
├── tsconfig.base.json   # Base TS compartida
├── TESTING.md           # Estrategia de tests unitarios y TDD
├── docs/
│   └── QA_SUBAGENT.md   # Definición y requisitos del subagente qa-engineer (verificación)
└── .env.example
```

## Windows: terminal (`npm` / `npx` / `supabase`)

Si PowerShell dice que no reconoce `npm` o `npx`, **Node no está en el `PATH` de esa ventana**. Prueba:

1. Cerrar y abrir la terminal (o reiniciar Cursor) tras instalar Node.
2. O en esa sesión:  
   `$env:Path = "C:\Program Files\nodejs;" + $env:Path`
3. Si `npm` falla por **execution policy** (`npm.ps1`), usa **`npm.cmd`** (y **`npx.cmd`**) en lugar de `npm` / `npx`.

**No uses** `npm install -g supabase`: el paquete oficial **rechaza** instalación global por npm (ver error *“Installing Supabase CLI as a global module is not supported”*). En este repo la CLI ya está en `devDependencies`; tras `npm install`:

- `npm.cmd run supabase:cli -- --version`
- `npm.cmd run supabase:cli -- init` (este proyecto **ya tiene** carpeta `supabase/`; `init` suele ser innecesario o puede avisar que ya existe)

Alternativa global en Windows: **`winget install Supabase.CLI`** (o instalar [Scoop](https://scoop.sh/) y luego su bucket de Supabase).

## Tests unitarios

Convenciones, flujo TDD y comandos: **`TESTING.md`**.
