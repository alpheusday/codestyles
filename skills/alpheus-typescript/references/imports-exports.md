# Imports & Exports

## Imports

### Node builtins

`node:` prefix and `import * as Name` namespace form with `PascalCase` aliases: `Path`, `Fs`, `Fsp`, `Http`, `Os`.

Never `import path from "node:path"`.

```ts
import * as Fs from "node:fs";
import * as Fsp from "node:fs/promises";
import * as Path from "node:path";
import * as Os from "node:os";
```

### Path Aliases

Avoid relative paths like `./` or `../`. Always prefer the absolute `#/` alias for all intra-package imports.

`#/*` → `./src/*` is configured in every package's `tsconfig.json`:

```json
{
    "compilerOptions": {
        "paths": {
            "#/root/*": ["./*"],
            "#/*": ["./src/*"]
        }
    }
}
```

## Exports

### Named Only

Prefer named exports only; avoid `export default` except where a runtime contract requires it (`export default app;` in `src/index.ts`).

### Declaration Without Export

Avoid inline `export const` or `export type` on the declaration:

```ts
type Options = {};

type Result = {};

const main = (options?: Options): Result => {};

export type { Options, Result };
export { main };
```

### Entrypoint Re-exports

In a library, `src/index.ts` or other entrypoints act as a barrel file to re-export types and values from internal files:

```ts
export type { Options } from "#/@types/options";
export { createXxx } from "#/functions/create";
```

### Renamed Re-exports for Public Ergonomics

For things like plugin routers, keep the local name generic and alias on export:

```ts
export { plugin as xxx };
export { plugin as pluginXxx };
export { router as routerXxx };
```

## See Also

- Structure: [`structure.md`](./structure.md)
- Types: [`types.md`](./types.md)
- Functions: [`functions.md`](./functions.md)
