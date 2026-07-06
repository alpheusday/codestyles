# Types

## `pub(crate)` for Internal State

Builder state fields and internal modules use `pub(crate)` so submodules can read them but external users cannot.

```rust
pub struct CreateSuccessJsonResponse<D> {
    pub(crate) state: JsonResponseState<D>,
}
```

`response/json/mod.rs`:

```rust
pub(crate) mod create;
pub(crate) mod error;
pub(crate) mod response;
```

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
