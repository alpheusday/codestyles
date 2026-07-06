# Patterns

## Naming

| Element               | Convention                               | Example                                           |
| --------------------- | ---------------------------------------- | ------------------------------------------------- |
| Modules / files       | `snake_case`                             | `request_body_limit`, `connect_info`              |
| Types                 | `PascalCase` nouns                       | `CreateJsonResponse`, `ServiceError`              |
| Functions             | `snake_case`                             | `from_request`, `as_code`, `create_router`        |
| Constants             | `SCREAMING_SNAKE_CASE`                   | `REQUEST_BODY_LIMIT_DEFAULT`, `CONTENT_TYPE_JSON` |
| Generics              | single uppercase or descriptive          | `T`, `S`, `D`, `B`, `Data`, `Error`               |
| Local vars            | `snake_case`, often with type annotation | `let listener: TcpListener`                       |
| OpenAPI operation IDs | `snake_case`                             | `get_info`, `get_index`                           |

## `as_*` vs `to_*`

Borrowed (`as_*` returns `&str` / `&T`) vs owned (`to_*` returns `String` / `T`):

```rust
pub fn as_code(&self) -> &str { /* ... */ }
pub fn to_code(&self) -> String { self.as_code().to_string() }
```

## Feature Detection via `pub(crate) const bool`

In proc-macro crates, detect features via `pub(crate) const FOO: bool` constants and branch with ordinary `if`, rather than `#[cfg]` inside `quote!`. Centralizes feature flags and avoids `#[cfg]` inside generated code.

```rust
// crates/macros/src/utils/features.rs
#[cfg(feature = "serde")]
pub(crate) const SERDE: bool = true;

#[cfg(not(feature = "serde"))]
pub(crate) const SERDE: bool = false;

#[cfg(feature = "utoipa")]
pub(crate) const UTOIPA: bool = true;

#[cfg(not(feature = "utoipa"))]
pub(crate) const UTOIPA: bool = false;
```

## Anti-Patterns

- `self::` / `super::` — use `crate::`
- `mod.rs`-less directories — `mod.rs` everywhere
- `.unwrap()` in library code that can fail — only in tests or impossible cases
- `extern crate` (edition 2024)

## See Also

- Types: [`types.md`](./types.md)
- Functions: [`functions.md`](./functions.md)
- axum: [`axum.md`](./axum.md)
- Comments: [`comments.md`](./comments.md)
