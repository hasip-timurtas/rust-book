## Futures, Tasks, and Threads

Understanding when to use threads vs async requires considering their different tradeoffs and capabilities.

### Execution Models

**Threads**:
- OS-managed, preemptive multitasking
- Heavy resource overhead (~2-8MB stack per thread)
- True parallelism on multi-core systems
- Context switches managed by OS scheduler

**Tasks (Async)**:
- Runtime-managed, cooperative multitasking  
- Lightweight (~KB overhead per task)
- Concurrency via yielding at await points
- Can run millions concurrently

### Interchangeable APIs

Many patterns work with both models:

<Listing number="17-41" caption="Thread-based implementation" file-name="src/main.rs">

```rust,editable
{{#rustdoc_include ../listings/ch17-async-await/listing-17-41/src/main.rs:threads}}
```

</Listing>

**Key differences**:
- `thread::spawn` vs `trpl::spawn_task`
- `thread::sleep` vs `trpl::sleep`  
- Both return handles that can be awaited/joined
- Same resulting streams despite different execution models

### Architectural Hierarchy

```
Runtime (manages tasks)
├── Task 1 (manages futures)
│   ├── Future A
│   ├── Future B
│   └── Future C
├── Task 2 (manages futures)
│   └── Future D
└── OS Threads (work-stealing execution)
    ├── Thread 1 
    ├── Thread 2
    └── Thread 3
```

**Three levels of concurrency**:
1. **Futures**: Finest granularity, cooperative yield points
2. **Tasks**: Group related futures, runtime-scheduled
3. **Threads**: OS-scheduled, can run tasks in parallel

### Performance Characteristics

**Threads excel at**:
- CPU-intensive work (parallel computation)
- Blocking operations without async alternatives
- Fire-and-forget background work
- Simple parallelism requirements

**Async excels at**:
- IO-intensive operations (network, file system)
- High-concurrency scenarios (thousands of connections)
- Structured concurrency patterns
- Resource-constrained environments

### Hybrid Patterns

Combine both approaches for optimal performance:

<Listing number="17-42" caption="Mixing threads and async" file-name="src/main.rs">

```rust,editable
{{#rustdoc_include ../listings/ch17-async-await/listing-17-42/src/main.rs:all}}
```

</Listing>

**Pattern**: Use threads for blocking operations, async channels for coordination.

### Work-Stealing Runtimes

Modern async runtimes use thread pools with work-stealing:

```
┌─────────────────────────────────────────┐
│ Async Runtime                           │
├─────────────────────────────────────────┤
│ Task Queue: [Task1, Task2, Task3, ...]  │
├─────────────────────────────────────────┤
│ Thread Pool:                            │
│ ├─ Worker Thread 1 ──┐                  │
│ ├─ Worker Thread 2 ──┼─ Work Stealing   │  
│ ├─ Worker Thread 3 ──┘                  │
│ └─ Worker Thread 4                      │
└─────────────────────────────────────────┘
```

Tasks can migrate between threads for load balancing, combining the benefits of both models.

### Decision Framework

**Choose Threads when**:
- Heavy CPU computation (image processing, mathematical calculations)
- Blocking C libraries without async bindings  
- Simple parallel algorithms (embarrassingly parallel problems)
- Need guaranteed execution progress (real-time systems)

**Choose Async when**:
- Network servers handling many connections
- IO-heavy applications (file processing, database queries)
- Event-driven architectures
- Memory-constrained environments

**Use Both when**:
- Web applications (async for request handling, threads for background jobs)
- Data pipelines (async for coordination, threads for CPU processing)
- Gaming (async for network, threads for physics/rendering)

### Cancellation and Cleanup

**Threads**: Limited cancellation support, manual cleanup
```rust,editable
// Thread cancellation is tricky
let handle = thread::spawn(|| {
    // No built-in cancellation mechanism
    loop { /* work */ }
});
// Must use external signaling
```

**Async**: Built-in cancellation when futures are dropped
```rust,editable
// Automatic cleanup when dropped
let task = spawn_task(async {
    some_operation().await; // Cancelled if task is dropped
});
drop(task); // Automatically cancels and cleans up
```

### Performance Guidelines

**Avoid**:
- Creating threads for each IO operation (thread-per-request anti-pattern)
- Async for CPU-bound work without yield points
- Mixing sync and async code unnecessarily

**Optimize**:
- Use `spawn_blocking` for sync code in async contexts
- Pool threads for repeated CPU work
- Batch async operations to reduce overhead
- Choose appropriate buffer sizes for channels

## Summary

**Threads**: OS-managed parallelism, higher overhead, preemptive scheduling
**Tasks**: Runtime-managed concurrency, lightweight, cooperative scheduling  
**Futures**: Building blocks for async state machines

The choice isn't exclusive - modern applications often use both approaches where each excels. Async provides better resource utilization for IO-bound work, while threads provide true parallelism for CPU-bound tasks.

**Best practice**: Start with async for concurrency needs, add threads for parallelism where beneficial. Use work-stealing runtimes that provide both models efficiently.

[ch16]: http://localhost:3000/ch16-00-concurrency.html
[combining-futures]: ch17-03-more-futures.html#building-our-own-async-abstractions
[streams]: ch17-04-streams.html#composing-streams
[ch21]: ch21-00-final-project-a-web-server.html

