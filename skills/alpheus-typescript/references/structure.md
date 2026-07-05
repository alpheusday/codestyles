# Project Structure

## Role-based directories inside source code directory like `./src/`

All of them should be organized by kind of thing unless explicitly requested.

### Common

- `@types/` - shared type definitions. The `@`-prefix exempts it from kebab-case filename rules
- `const/` or `constants/` - runtime constants and default-option records
- `configs/` - cross-cutting singletons
- `functions/` - shared logic, one file per concern, may be organized into nested sub dirs by domain
- `modules/` - Individual module contains specific logic
- `index.ts` - the entry

### Library Specific

- `plugins/` - framework-adapter code, each in its own directory with an `index.ts`

### API Specific

- `middlewares/` - framework-adapter code, each in its own directory with an `index.ts`
- `router/` - the router connect routes from `modules/`

### Frontend (React) Specific

- `components/` - shared components for different modules
- `contexts/` - React contexts
- `hooks/` - React hooks
- `i18n` - including the config and translations
- `router/` - the router connect routes from `modules/`

## File naming

Always go with kebab-case everywhere.

Examples: `on-error.ts`, `virtual-entry.ts`, `resolve-options.test.ts`. Only `@types/` is exempt.

## Barrel files

Every directory exports through `index.ts`. Internal imports point at the directory, avoid using `index.ts` directly:

```ts
import { infoSchema } from "#/modules/info/schemas"; // ✓ directory
import { infoSchema } from "#/modules/info/schemas/index.ts"; // ✗ never
```

## Monorepo Layout

pnpm workspaces mostly go with the following directories:

- `package` or `packages`
- `test` or `tests`
- `bench` or `benches`
- `examples`

Each package is a workspace, each test package is its own workspace that depends on the built dist via `workspace:*`.

Test-package internal layout — fixtures, helpers, `<unit>.test.ts` naming — is defined in [`testing.md`](./testing.md).

## See Also

- Path aliases: [`imports-exports.md`](./imports-exports.md)
- Types: [`types.md`](./types.md)
- Naming: [`patterns.md`](./patterns.md)
- Tests: [`testing.md`](./testing.md)
