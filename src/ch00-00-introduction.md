# Introduction

> Note: This edition of the book is the same as [The Rust Programming
> Language][nsprust] available in print and ebook format from [No Starch
> Press][nsp].

[nsprust]: https://nostarch.com/rust-programming-language-2nd-edition
[nsp]: https://nostarch.com/

_The Rust Programming Language_ provides a systems programming approach that combines memory safety with zero-cost abstractions. Unlike TypeScript's runtime type checking or Node.js's garbage-collected memory management, Rust enforces safety at compile time without runtime overhead.

Rust addresses common pain points in systems programming: memory leaks, race conditions, and undefined behavior—issues that higher-level languages like TypeScript abstract away but become critical in systems-level development.

## Why Rust for Experienced Engineers

### Memory Safety Without Performance Cost

Rust's ownership system eliminates entire classes of bugs common in C/C++ while matching their performance. Think of it as compile-time guarantees for what you'd normally handle with careful code reviews and runtime checks in other languages.

### Modern Tooling Ecosystem

Rust's development experience rivals modern web development:

- **Cargo**: Dependency management and build system (similar to npm/yarn but for systems programming)
- **Rustfmt**: Code formatting (like Prettier)
- **rust-analyzer**: LSP implementation providing IDE features comparable to TypeScript's language server
- **Clippy**: Static analysis tool beyond basic linting

### Concurrency Without Data Races

Rust's ownership model prevents data races at compile time—eliminating a major source of production bugs in concurrent systems. This is particularly relevant coming from Node.js's event loop model where race conditions are less common but not impossible.

### Production Adoption

Major companies use Rust for performance-critical infrastructure: Discord (voice/video), Dropbox (storage), Facebook (source control), Microsoft (Windows components), and Mozilla (Firefox). These are systems where TypeScript/Node.js wouldn't be appropriate due to performance requirements.

## Target Use Cases

- **CLI tools**: Better performance than Node.js scripts with similar ergonomics
- **Web services**: Lower latency and memory usage than Node.js/TypeScript backends
- **Systems programming**: Device drivers, embedded systems, OS components
- **Performance-critical components**: Computational algorithms, real-time systems
- **Infrastructure tooling**: Networking tools, databases, container runtimes

## Book Structure

This book assumes software engineering experience. Chapters are divided into concept and project chapters:

**Core Concepts (Chapters 1-11):**
- Ch 1: Installation and tooling setup
- Ch 2: Hands-on number guessing game (quick practical intro)
- Ch 3: Basic syntax and types
- Ch 4: **Ownership system** (Rust's key differentiator—most important chapter)
- Ch 5-6: Structs and enums (similar to TypeScript interfaces/unions but with more capabilities)
- Ch 7: Module system (package organization)
- Ch 8: Collections (Vec, HashMap, etc.)
- Ch 9: Error handling (Result types—different from exceptions)
- Ch 10: Generics and traits (similar to TypeScript generics but more powerful)
- Ch 11: Testing

**Applied Concepts (Chapters 12-21):**
- Ch 12: CLI tool project (grep implementation)
- Ch 13: Functional programming features (closures, iterators)
- Ch 14: Cargo ecosystem and publishing
- Ch 15: Smart pointers (memory management primitives)
- Ch 16: Concurrency (threads, channels, async)
- Ch 17: Async/await (different from JavaScript promises but similar concepts)
- Ch 18: Object-oriented patterns in Rust
- Ch 19: Pattern matching (more powerful than switch statements)
- Ch 20: Advanced features (unsafe code, macros)
- Ch 21: Multithreaded web server project

**Recommended approach:** Read sequentially through Chapter 4 (ownership is fundamental), then adapt based on your specific interests.

## Error Messages and Learning Process

Rust's compiler provides detailed error messages with suggested fixes. Unlike TypeScript's sometimes cryptic errors, Rust errors are designed to be educational. The compiler often suggests exactly what to change.

Code examples use Ferris icons to indicate expected behavior:

| Icon | Meaning |
|------|---------|
| <img src="img/ferris/does_not_compile.svg" class="ferris-explain" alt="Ferris with a question mark"/> | Compilation failure (intentional for learning) |
| <img src="img/ferris/panics.svg" class="ferris-explain" alt="Ferris throwing up their hands"/> | Runtime panic (controlled failure) |
| <img src="img/ferris/not_desired_behavior.svg" class="ferris-explain" alt="Ferris with one claw up, shrugging"/> | Runs but produces incorrect results |

## Source Code

The source files from which this book is generated can be found on
[GitHub][book].

[book]: https://github.com/rust-lang/book/tree/main/src
