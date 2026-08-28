# Testing

## Separate Test Workspace Member

The published library crate should not contain inline test modules, unless the repo uses inline test modules. No `benches/` directory inside the crate. All testing and benches live in a separate workspace member.

A dedicated `test/` or `tests/` workspace member owns all integration tests and fixtures.

For example, `test/Cargo.toml`:

```toml
[package]
name = "test"
version = "0.1.0"
edition = "2024"
publish = false

[dependencies]
my_lib = { workspace = true, features = ["all"] }
serde = { workspace = true }
```

The test crate always enables `features = ["all"]`, so feature-gated test submodules are unconditional in the test crate — feature gating happens at the library level.

## Test Crate Root

The whole test crate is gated to `#[cfg(test)]` at the top level — it compiles to nothing in a non-test build.

For example, `test/src/lib.rs`:

```rust
#[cfg(test)]
mod fixtures;

#[cfg(test)]
mod tests;
```

## Test Function Names Describe the Property

Prefer to go with `test_*` prefixed on the test function names:

```rust
#[test]
fn test_value() { /* ... */ }

#[test]
fn test_deref() { /* ... */ }
```

## No `mod tests` Wrapper Inside Test Files

Tests are NOT wrapped in `#[cfg(test)] mod tests { ... }` inside each file — because the whole test crate is already a test crate. Each test file is a flat list of `#[test] fn`:

## `#[tokio::test]` for Async

Every async test gets `#[tokio::test]`.

```rust
#[tokio::test]
async fn test() {
    // ...
}
```

## `.unwrap()` Only in Tests

`.unwrap()` is fine in tests where failure is genuinely a test failure. Avoid in library code paths that can fail.

## `--nocapture`

Run tests with `--nocapture` so `println!` output is visible:

```just
test:
    cargo test -p tests -- --nocapture
```

## Per-Feature Test Files

Each feature may get its own `test/src/tests/<feature>.rs` with multiple test functions named `test_<scenario>`:

```rust
#[tokio::test]
async fn test() { /* ... */ }

#[tokio::test]
async fn test_empty_id() { /* ... */ }

#[tokio::test]
async fn test_empty_body() { /* ... */ }
```

A `tests/src/tests/mod.rs` re-exports per-feature test modules and includes a smoke test that hits `/`.

## Test Module Layout

Applies to both an inline `#[cfg(test)] mod tests` and separate test-crate files.

Flow within a test module or file, top to bottom: `use` statements, then shared test helpers, then test groups.

Group tests by the item under test. Order the groups in the same order those items appear in the source file (dependency order — see [`functions.md`](./functions.md)). If the source defines `b` before `pub a`, the test groups run: `b`'s group, then `a`'s group.

Within each group, simple tests come before complex tests:

- **Simple test:** single concept, minimal setup, few assertions.
- **Complex test:** multiple concepts, multi-step fixture, many assertions.

## Inline Test Module

An inline `#[cfg(test)] mod tests` is optional — some repos use it, others use the separate test workspace member (the default). When present, it is always the last item in the file.

Inside it, `use super::*;` is permitted — the single allowed exception to the `crate::` rule (see [`imports-exports.md`](./imports-exports.md)).

For example:

```rust
use crate::constants::MAX_ID_LEN;

struct BOptions {
    max_len: usize,
}

fn normalize_id(raw: &str) -> String { /* ... */ }

fn b(raw: &str, options: &BOptions) -> Option<String> {
    let id: String = normalize_id(raw);
    /* ... */
    Some(id)
}

pub struct UserId { /* ... */ }

pub fn a(raw: &str) -> UserId { /* ... */ }

#[cfg(test)]
mod tests {
    use super::*;

    fn options() -> BOptions {
        BOptions { max_len: 8 }
    }

    // ---- tests for `b`: simple first ----

    #[test]
    fn test_b_trims_and_lowercases() { /* ... */ }

    // ---- tests for `b`: complex after ----

    #[test]
    fn test_b_pipeline() { /* ... */ }

    // ---- tests for `a`: simple first ----

    #[test]
    fn test_a_wraps_id() { /* ... */ }

    // ---- tests for `a`: complex after ----

    #[test]
    fn test_a_full_pipeline() { /* ... */ }
}
```

## See Also

- Structure: [`structure.md`](./structure.md)
- Functions: [`functions.md`](./functions.md) — function/item ordering rules live there.
