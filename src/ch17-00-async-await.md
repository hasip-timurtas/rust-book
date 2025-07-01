# Asynchronous Programming: Async, Await, Futures, and Streams

Modern applications require handling concurrent operations efficiently. Rust provides async programming through Futures, Streams, and the `async`/`await` syntax. This chapter covers async fundamentals and practical patterns for building concurrent systems.

You'll learn:
- `async`/`await` syntax and the Future trait
- Concurrent programming patterns with async
- Working with multiple futures and streams
- Trait details: `Future`, `Pin`, `Unpin`, `Stream`
- Combining async with threads

## Parallelism vs Concurrency

**Concurrency**: Single worker switches between tasks before completion (time-slicing).
**Parallelism**: Multiple workers execute tasks simultaneously.
**Serial**: Tasks must complete in sequence due to dependencies.

```
Concurrent:  A1 → B1 → A2 → B2 → A3 → B3
Parallel:    A1 → A2 → A3
             B1 → B2 → B3
```

CPU-bound operations benefit from parallelism. IO-bound operations benefit from concurrency. Most real systems need both.

In async Rust, concurrency is primary. The runtime may use parallelism underneath, but your async code provides concurrency semantics.

### Blocking vs Non-blocking

Traditional IO operations block execution until completion. Async operations yield control to the runtime when waiting, allowing other tasks to progress.

```rust,ignore
// Blocking - thread stops here
let data = std::fs::read("file.txt")?;

// Non-blocking - runtime can schedule other work
let data = tokio::fs::read("file.txt").await?;
```

Async enables writing sequential-looking code that's actually concurrent:

```rust,ignore,does_not_compile
let data = fetch_data_from(url).await;
println!("{data}");
```

This syntax compiles to a state machine that yields control at await points.
