# Testing

## Separate Test Workspace Member

The published library crate should not contain inline test modules unless explicitly requested. No `benches/` directory inside the crate. All testing and benches live in a separate workspace member.

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

## See Also

- Structure: [`structure.md`](./structure.md)
- Functions: [`functions.md`](./functions.md)
