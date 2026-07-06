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

Helpers first, primary exported function last. The functions being used by another have higher priority in the file.

```rust
fn helper_a() -> u32 { /* ... */ }

fn helper_b() -> u32 { helper_a() }

pub fn main_fn() -> u32 { helper_b() }
```

## See Also

- Types: [`types.md`](./types.md)
- Patterns: [`patterns.md`](./patterns.md)
