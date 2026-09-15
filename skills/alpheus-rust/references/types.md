# Types

## Suffix Conventions

- `XxxOptions` — input bag
- `XxxResult` — output
- `XxxState` — builder internal state

Builder entry/state struct naming (`CreateXxx`, `XxxFunctions`) — see [`functions.md`](./functions.md).

## Explicit Types on Locals

Even when inferable, types are annotated for readability — a deliberate documentation-through-types style.

```rust
let rer: ResponseError = ResponseError::TooLarge;
let res: Res = CreateJsonResponse::failure();
```

## See Also

- Imports & Exports: [`imports-exports.md`](./imports-exports.md)
- Functions: [`functions.md`](./functions.md)
- Patterns: [`patterns.md`](./patterns.md)
- Comments: [`comments.md`](./comments.md)
