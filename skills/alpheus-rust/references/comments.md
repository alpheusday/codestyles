# Comments

## `//!` for Crate and Module Docs

Crate-level `//!` docs in `lib.rs` are the primary doc surface (they double as the README). Go with `## Usage`, `## Features`, and `## Example` (or `## Examples`) sections.

## `///` for Items

Every public item gets a `///` doc. Pattern: short summary line, optional blank `///` line, optional `## Example` section.

````rust
/// Create something.
///
/// ## Example
///
/// ```rust
/// create_xxx("Hello, World!");
/// ```
pub fn create_xxx<M: Into<String>>(
    message: M,
) {
    // ...
}
````

## Doctests

Use `no_run` for examples that require a runtime (most axum handlers):

````rust
/// ```rust,no_run
/// use axum::http::StatusCode;
/// use jder_axum::response::{Response, CreateResponse};
///
/// async fn route() -> Response {
///     CreateResponse::success()
///         .status(StatusCode::CREATED)
///         .body("created")
///         .create()
/// }
/// ```
````

Use `ignore` for examples that need an external crate not in doctest scope:

````rust
/// ```rust,ignore
/// use literalize::literal;
///
/// #[literal("not_found")]
/// struct NotFound;
/// ```
````

## Per-Field `///` on Public Type Fields

Describe the field's purpose and, when applicable, its default.

```rust
pub struct State {
    /// Whether it is ...
    pub is_xxx: bool,
}
```

## Intra-Doc Links with Full Paths

```rust
/// For validation on key value, see
/// [`get_header_from_key_value`](crate::response::header::get_header_from_key_value).
```

## Markdown Links to External Crates

```rust
/// Based on `MustBeStrVisitor` from [`monostate`](https://crates.io/crates/monostate).
```

## `## Example` Section on Public APIs

Prefer to include a `## Example` section on public APIs. Overloaded functions get `///` on each overload signature, not on the implementation.

## Inline `//` Comments

Go with inline comments only when logic requires explanation to avoid deletion that would affect behavior.

## Conditional Docs for Feature-Gated Items

Feature-gated items mention the feature in the doc.

```rust
/// Form extractor module,
/// available with `form` feature.
#[cfg(feature = "form")]
pub mod form;
```

## See Also

- Types: [`types.md`](./types.md)
- Functions: [`functions.md`](./functions.md)
- Patterns: [`patterns.md`](./patterns.md)
