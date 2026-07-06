# Structure

## `mod.rs` Everywhere

Every directory that is a module has a `mod.rs`. Avoid using `<name>.rs` sibling-file style for a directory-with-children unless explicitly requested.

```text
// ✗ never
modules.rs
modules/
└── info.rs

// ✓
modules/
├── mod.rs
└── info/
    └── mod.rs
```

Leaf modules (no children) are a single `<name>.rs` file.

## File and Directory Naming

snake_case everywhere.

Plural for collection-directories:

- `routes`
- `services`
- `errors`
- `structs`
- `constants`

Single-concept leaf files:

- `service.rs`
- `response.rs`
- `env.rs`
- `path.rs`

## Role-based directories

All of them should be organized by kind of thing unless explicitly requested.

### API

- `common/` - shared logic, one file per concern, may be organized into nested sub dirs by domain
- `constants/` - runtime constants and default-option records
- `configs/` - cross-cutting singletons
- `modules/` - Individual module contains specific logic
- `router/` - the router connect routes from `modules/`
- `main.rs` - the entry

Top-level roles: `common` (cross-cutting shared code), `constants` (app-wide constants and env accessors), `modules` (feature modules), `router` (top-level router assembly).

Inside `modules/<feature>/`, the fixed trinity:

- `routes/` — HTTP handlers
- `schema/` — request/response DTOs
- `services/` — business logic

A new feature `foo` creates `src/modules/foo/{routes,schema,services}/mod.rs` plus `src/modules/foo/mod.rs` declaring:

```rust
pub mod routes;
pub mod schema;
pub mod services;
```

Binary is named `app`, not the framework name.

### Library

- `crate/src/` or `crates/<name>/src/`
    - `constants/` - runtime constants and default-option records (optional)
    - `functions/` - shared logic, one file per concern, may be organized into nested sub dirs by domain (optional)
    - `lib.rs` - the entry

- `test/src` or `tests/<name>/src`
    - `main.rs` - the entry

Top-level modules in the library are role-based by capability. Each has a `mod.rs` that declares submodules and re-exports public types.

The `test/` or `tests/<name>/` member is a binary crate (`publish = false`) that doubles as the test suite.

## Module Declarations in `lib.rs`

Bare `pub mod` declarations, each on its own line, separated by blank lines.

For example, `crate/src/lib.rs`:

```rust
pub mod extract;

pub mod layers;

pub mod response;
```

## Workspace Dependencies

All shared dependencies declared once in `[workspace.dependencies]` and consumed via `{ workspace = true }`.

```toml
[workspace.dependencies]
serde = { version = "^1.0.0", features = ["derive"] }
tokio = { version = "^1.40.0" }
```

## Crate Packaging

In publishable crates, `include` only the source and metadata:

```toml
include = ["src/**", "Cargo.toml", "README.md"]
```

## Feature Flags

- granular features with `dep:` syntax for optional deps

- an `all` umbrella feature

- kebab-case aliases for ergonomics

```toml
[features]
default = []
serde = ["dep:serde"]
utoipa = ["dep:utoipa"]
all = ["serde", "utoipa"]
```

```toml
[package.metadata.docs.rs]
features = ["all"]
```

## See Also

- Imports & Exports: [`imports-exports.md`](./imports-exports.md)
- Patterns: [`patterns.md`](./patterns.md)
- Testing: [`testing.md`](./testing.md)
