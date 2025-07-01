# Fearless Concurrency

Rust's ownership and type systems provide compile-time guarantees for concurrent programming. Unlike runtime concurrency bugs common in other languages, Rust's approach moves many concurrency errors to compile time, eliminating data races and memory safety issues through static analysis.

Rust supports multiple concurrency paradigms:

- **Message-passing concurrency**: Channels for thread communication
- **Shared-state concurrency**: Mutexes and atomic types for shared data access
- **Thread creation**: OS threads with the `thread::spawn` API
- **Send/Sync traits**: Compile-time thread safety guarantees

The standard library provides a 1:1 threading model (one OS thread per language thread), with async/await as an alternative concurrency model covered in the next chapter.

This chapter covers:

- Thread creation and management
- Channel-based message passing
- Shared state with mutexes and atomic reference counting
- The `Send` and `Sync` traits for extensible concurrency
