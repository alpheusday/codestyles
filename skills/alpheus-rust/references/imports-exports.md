# Imports & Exports

## Imports

### Ordering

Three blocks, blank-line separated: `std` → external → `crate::`.

```rust
use std::time::Duration;

use axum::Router;

use crate::constants::env::get_port;
```

### One Crate per `use`

External imports are one crate per `use` statement, alphabetized by crate then by path. Multi-line braces use trailing commas, one item per line.

```rust
use xxx::{
    A,
    b::B,
    c::{
        d::D,
        e::E,
    },
};
```

### `crate::` Always

Use `crate::` for all intra-crate paths. Never `self::` or `super::`.

```rust
// ✓
use crate::Xxx;

// ✗ never
use super::Xxx;
use self::Xxx;
```

### Avoid Glob Imports

Every import is explicit. Avoid `use crate::prelude::*` style.

```rust
// ✗ avoid
use xxx::prelude::*;
use crate::prelude::*;

// ✓
use xxx::{A as _, B, C};
```

### `as _` for Trait-into-Scope

Trait imports for method resolution are aliased to `_` to bring only the method surface into scope without polluting the namespace.

```rust
use xxx::{A as _, B, C};
```

### Collision-Avoidance Aliases

When wrapping an upstream type, import the upstream with a leading-underscore alias and let the local type take the canonical name.

```rust
use axum::extract::{Json as _Json};

pub struct Json<T>(pub T);
```

### `mod` Before `use`

Private `mod` declarations come before `use` statements.

```rust
mod a;
mod b;

use crate::xxx;
```

## See Also

- Structure: [`structure.md`](./structure.md)
- Types: [`types.md`](./types.md)
- Patterns: [`patterns.md`](./patterns.md)
