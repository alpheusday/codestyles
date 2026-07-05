# Testing

## File Naming and Placement

- tests live in a separate workspace package(s), for example:
    - `test/src/...`
    - `tests/<pkg>/src/...`

- test fixtures live under `__fixtures__/`

- shared test helpers live under `helpers/`

- test directory names may shortened from the source path:
    - `functions/env/` → `env/`
    - `functions/http` → `http/`

- test files are `<unit>.test.ts` with kebab-case naming

- benchmarks use `<unit>.bench.ts` with kebab-case naming

## Framework

Prefer to use Vitest unless explicitly requested.

Import `{ describe, expect, it }` (and `afterEach` etc when needed) from `vitest`:

```ts
import { describe, expect, it } from "vitest";
```

Do configuration per test workspace (`vitest.config.ts`):

```ts
import { defineConfig } from "vitest/config";

export default defineConfig({
    resolve: {
        tsconfigPaths: true,
    },
    test: {
        logHeapUsage: true,
    },
});
```

## Every Callback is Explicitly Typed

```ts
describe("resolveOptions", (): void => {
    it("...", (): void => {
        // ...
    });

    it("...", async (): Promise<void> => {
        // ...
    });
});
```

## See Also

- Structure: [`structure.md`](./structure.md)
- Functions: [`functions.md`](./functions.md)
