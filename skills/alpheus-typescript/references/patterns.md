# Patterns

## Naming

- variables / parameters / functions: `camelCase`, for example:
    - `createX`
    - `getX`
    - `toX`
    - `resolveX`
    - `emitX`
    - `buildX`
    - `serviceX`
    - `onX`
- types: `PascalCase` nouns. No `T`-prefix
- module constants: `UPPER_SNAKE_CASE`, apply `as const` to literal-typed constants
- boolean: `is`/`has`/`should` prefix on locals and context fields
- OpenAPI operation IDs: `snake_case` (`get_info`, `get_index`)

## Anti-Patterns

- `interface` - use `type` unless redeclaraing module interface in `.d.ts`
- relative imports `../` / `./` - use path alias like `#/`
- classes for application state — use a `createX` factory
- `.then()` / `.catch()` — use `async`/`await`
- the literal `undefined` as a value — use `void 0`:

```ts
// ✗ never
if (options.foo === undefined) {
}
const foo: string | undefined = opts.bar ?? undefined;

// ✓
if (options.foo === void 0) {
}
const foo: string | undefined = opts.bar ?? void 0;
```

## See Also

- Imports & Exports: [`imports-exports.md`](./imports-exports.md)
- Types: [`types.md`](./types.md)
- Functions: [`functions.md`](./functions.md)
