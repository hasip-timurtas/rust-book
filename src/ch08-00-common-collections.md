# Common Collections

Rust's standard library provides heap-allocated collections with different performance characteristics:

- **`Vec<T>`**: Growable array (like JavaScript Array)
- **`String`**: UTF-8 string buffer (mutable, unlike string literals)  
- **`HashMap<K, V>`**: Hash table (like JavaScript Map/Object)

## Key Differences from JavaScript

**Memory Management**: Collections manage their own heap allocation. No garbage collector means predictable performance.

**Type Safety**: Homogeneous collections with compile-time type checking.

**Performance**: Zero-cost abstractions with explicit complexity guarantees.

This chapter covers practical usage patterns, performance characteristics, and API design principles for each collection type.

[collections]: ../std/collections/index.html
