# Functions

## Function Naming Prefixes

| Prefix     | Use                      | Example                          |
| ---------- | ------------------------ | -------------------------------- |
| `route_`   | HTTP handler             | `route_index`, `route_info`      |
| `service_` | business logic           | `service_info`                   |
| `router_`  | router-export            | `router_info`                    |
| `get_`     | env/option accessor      | `get_port`, `get_version`        |
| `is_`      | boolean predicate        | `is_dev`, `is_dev_https`         |
| `expand_`  | proc-macro expander      | `expand_str`, `expand_int`       |
| `create_`  | factory / router builder | `create_router`, `create_server` |

## Builder Pattern

### Consumed-`mut self` Chain

Builders hold a state struct; each setter takes `mut self`, mutates, and returns `Self`; the terminal `.create()` consumes.

```rust
impl CreateSomething {
    pub fn data<T>(
        mut self,
        data: T,
    ) -> Self {
        self.state.data = data;
        self
    }

    pub fn create(self) -> Something {
        /* ... */
    }
}
```

Chain usage:

```rust
CreateSomething::success()
    .data(
        // ...
    )
    .create()
```

### `Default` Delegates to `new()`

`new()` is the canonical constructor; `Default` always delegates to it.

```rust
impl Default for Something {
    fn default() -> Self { Self::new() }
}
```

### `Create*` Entry Naming

- entry struct: `CreateXxx`
- builder state struct: `XxxFunctions`
- internal state: `XxxState`
- terminal method: `.create()` or `.build()`

## Organization within a File

File flow, top to bottom: `use` statements, then items in dependency order, then an optional `#[cfg(test)] mod tests` (always last).

Dependency-first: a callee appears before its caller. The functions being used by another have higher priority in the file.

When two items have no dependency between them, break the tie by visibility: private first, then `pub(crate)`, then `pub`.

An item's supporting types appear immediately before the item that uses them. If item `X` depends on item `Y`, `X`'s section comes after `Y`'s section.

```rust
struct BOptions { /* ... */ }

fn normalize(options: &BOptions) -> u32 { /* ... */ }

fn b(options: &BOptions) -> u32 { normalize(options) }

struct Report { /* ... */ }

pub fn a() -> Report { /* ... */ }
```

Per type block: `trait` first, then `struct`, then inherent `impl`, then trait `impl`(s).

Within an inherent `impl`: `new` (if present) may lead, then private helper methods, then public methods — each group in dependency order.

```rust
struct ResponseConfig {
    pretty: bool,
}

pub struct JsonResponse<D> {
    data: Option<D>,
    config: ResponseConfig,
}

impl<D> JsonResponse<D> {
    pub fn new() -> Self { /* ... */ }

    fn etag(&self) -> String { /* ... */ }

    fn format_errors(&self) -> Vec<String> { /* ... */ }

    pub fn render(&self) -> String { /* ... */ }
}
```

Within a trait `impl`, methods mirror the trait declaration order — never alphabetical.

```rust
trait Validate {
    fn name(&self) -> &str;

    fn strictness(&self) -> u8;

    fn validate(&self) -> bool;
}

impl Validate for Report {
    fn name(&self) -> &str { /* ... */ }

    fn strictness(&self) -> u8 { /* ... */ }

    fn validate(&self) -> bool { /* ... */ }
}
```

## See Also

- Types: [`types.md`](./types.md)
- Patterns: [`patterns.md`](./patterns.md)
