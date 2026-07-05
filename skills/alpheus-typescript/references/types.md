# Types

## Where Types Live

- shared/Public types: in `@types/` directory
- `@types/` is the single source of truth for the public surface
- internal-only types co-located with their function

## Prefer `type` over `interface`

Prefer `type` over `interface` unless explicitly requested.

```ts
type Options = {
    name: string;
    isABC: boolean;
};
```

## The `CompleteX` / `X` Duality

Define the fully-required internal shape as `CompleteOptions`, then derive the user-facing partial form:

```ts
import type { Format, Partial } from "ts-vista";

type CompleteOptions = {
    name: string;
    isABC: boolean;
};

type Options = Format<Partial<CompleteOptions>>;
```

## Layered Option Taxonomy

For engines with multiple option sources, decompose into layers and intersect:

```ts
type PresetOptions = {
    /* engine-supplied */
};

type UserOptions = {
    /* user-supplied, readonly */
};

type DynamicOptions = {
    /* per-call */
};

type Options = Format<PresetOptions & UserOptions & DynamicOptions>;
```

## Prefer Derived Types for Factory Function

```ts
const createXxx = (options: CreateXxxOptions) => {
    return {
        // ...
    };
};

type Xxx = ReturnType<typeof createXxx>;
```

## Suffix Conventions

- `XxxOptions` - function input bag
- `XxxResult` - function output
- `CompleteXxx` - all-required internal shape
- `ResolvedXxx` - shape after defaults are applied
- `XxxContext` - ambient context object
- `XxxListener` - `(event: XxxEvent) => void | Promise<void>`

## `as const` on Literal-typed Constants

```ts
const SIGNATURE = "_HELLO_" as const;
const ID = "id" as const;
```

## See Also

- Imports & Exports: [`imports-exports.md`](./imports-exports.md)
- Functions: [`functions.md`](./functions.md)
- Patterns: [`patterns.md`](./patterns.md)
- Comments: [`comments.md`](./comments.md)
