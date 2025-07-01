# Fearless Concurrency

Rust's ownership system eliminates data races and memory safety issues in concurrent code at compile time, enabling high-performance parallel programming without runtime overhead.

## Rust's Concurrency Advantage

**Compile-time Safety**: Ownership and borrowing rules prevent:
- Data races
- Use-after-free in multi-threaded contexts  
- Memory corruption from concurrent access

**Zero-cost Abstractions**: Thread safety without runtime performance penalty.

## Concurrency Models

**Message Passing**: Channels for communication between threads
- Similar to Go channels or Node.js worker threads
- "Don't communicate by sharing memory; share memory by communicating"

**Shared State**: Mutexes and atomic types for shared data
- Compile-time prevention of race conditions
- Lock-free programming with atomics

**Async/Await**: Cooperative concurrency (Chapter 17)
- Similar to JavaScript async/await but with different runtime model

## Compared to Node.js

**Node.js**: Single-threaded event loop with worker threads
```javascript
// Shared state requires careful coordination
const { Worker, isMainThread, parentPort } = require('worker_threads');
// Complex setup for true parallelism
```

**Rust**: Native threading with compile-time safety
```rust
// Safe parallel computation
use std::thread;
let handles: Vec<_> = (0..4).map(|i| {
    thread::spawn(move || compute_chunk(i))
}).collect();
```

## Key Safety Traits

**`Send`**: Types safe to transfer between threads
**`Sync`**: Types safe to share references between threads

These traits are automatically implemented based on type composition, ensuring thread safety by construction.

This chapter demonstrates building concurrent systems with guaranteed safety and optimal performance.

Here are the topics we'll cover in this chapter:

- How to create threads to run multiple pieces of code at the same time
- _Message-passing_ concurrency, where channels send messages between threads
- _Shared-state_ concurrency, where multiple threads have access to some piece
  of data
- The `Sync` and `Send` traits, which extend Rust's concurrency guarantees to
  user-defined types as well as types provided by the standard library
