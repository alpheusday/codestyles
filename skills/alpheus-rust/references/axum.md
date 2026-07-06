# axum

## `#[tokio::main]`

Entry point uses `#[tokio::main]` with `tokio` `features = ["full"]`.

## `async fn` for Handlers and Services

All HTTP handlers and service-layer functions are `async fn`, even when trivially synchronous, for future-proofing.

```rust
pub async fn service_info() -> Result<Info, ServiceError> {
    Ok(Info {
        version: get_version(),
    })
}
```

## `#[axum::debug_handler]` on Handlers

Every route handler gets `#[axum::debug_handler]` for better debug error messages.

```rust
#[axum::debug_handler]
async fn route_info() -> Response { /* ... */ }
```

## Routes Return `Response`, Never `Result`

Routes always return `axum`'s `Response` and branch on the service result. Services return `Result<T, ServiceError>`.

```rust
async fn route_info() -> Response {
    match service_info().await {
        | Ok(data) => CreateJsonResponse::success().data(data).create(),
        | Err(err) => err.into_failure_response(),
    }
}
```

## See Also

- Functions: [`functions.md`](./functions.md)
- Patterns: [`patterns.md`](./patterns.md)
