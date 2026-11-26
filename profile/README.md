# Rust Framework

Three crates that port the **ASP.NET Core application model** to Rust. Take one, or use all three — they were designed together but none of them depends on the others being present.

| Crate | What it is | Docs |
|---|---|---|
| **[rust-webx](https://github.com/Rust-Framework/rust-webx)** | Web API framework: *request-as-endpoint*, DI + Mediator, declarative `#[validate]`, `problem+json`, OpenAPI, auth/session, transactional outbox, realtime fan-out | [docs](https://rust-framework.github.io/rust-webx/) · [crates.io](https://crates.io/crates/rust-webx) |
| **[rust-ef](https://github.com/Rust-Framework/rust-ef)** | EF Core-shaped ORM: `DbContext` unit of work, `DbSet<T>`, `linq!` queries, change tracking, migrations over SQLite / PostgreSQL / MySQL | [docs](https://rust-framework.github.io/rust-ef/) · [crates.io](https://crates.io/crates/rust-ef) |
| **[rust-dix](https://github.com/Rust-Framework/rust-dix)** | MEDI-shaped DI container: singleton / scoped / transient, keyed services, build-time validation (cycles, captive dependencies), zero `unsafe` | [docs](https://rust-framework.github.io/rust-dix/) · [crates.io](https://crates.io/crates/rust-dix) |

```rust
struct HelloRequest;

#[get("/hello")]
impl IRequest<String> for HelloRequest {}

#[derive(Default)]
struct HelloHandler;

#[handler]
#[async_trait]
impl IRequestHandler<HelloRequest, String> for HelloHandler {
    async fn handle(&mut self, _req: HelloRequest) -> ActionResult<String> {
        Ok("Hello, World!".to_string())
    }
}
```

**Coming from .NET?** The vocabulary maps directly: `IRequest`/`IRequestHandler` ↔ a request and its handler, `IMediator::send()` ↔ MediatR, `#[fromclaim]` ↔ `HttpContext.User`, `problem+json` ↔ `ProblemDetails`, `ServiceCollection`/`ServiceProvider` ↔ MEDI, `DbContext`/`SaveChanges` ↔ EF Core.

If instead you want a minimal HTTP toolkit and prefer to choose every layer yourself, [`axum`](https://github.com/tokio-rs/axum) and friends are the better fit — that is a different, and often correct, choice.

Licensed MIT. MSRV 1.95.
