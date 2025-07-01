## Working with Any Number of Futures

### Variable-Arity Joins

The `join!` macro handles variable numbers of futures:

<Listing number="17-14" caption="Using join! for multiple futures" file-name="src/main.rs">

```rust,editable
{{#rustdoc_include ../listings/ch17-async-await/listing-17-14/src/main.rs:here}}
```

</Listing>

For dynamic collections of futures, use `join_all`:

<Listing number="17-15" caption="Attempting to store futures in a vector (doesn't compile)">

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch17-async-await/listing-17-15/src/main.rs:here}}
```

</Listing>

**Error**: Each async block creates a unique anonymous type. Even identical blocks have different types.

### Trait Objects for Dynamic Collections

Use trait objects to unify future types:

<Listing number="17-16" caption="Using Box for type alignment (still doesn't compile)" file-name="src/main.rs">

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch17-async-await/listing-17-16/src/main.rs:here}}
```

</Listing>

<Listing number="17-17" caption="Explicit type annotation for trait objects" file-name="src/main.rs">

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch17-async-await/listing-17-17/src/main.rs:here}}
```

</Listing>

**Pin Requirements**: `join_all` requires futures to implement `Unpin`. Most async blocks don't implement `Unpin` automatically.

### Pinning Futures

Use `Pin<Box<T>>` to satisfy the `Unpin` requirement:

<Listing number="17-18" caption="Using Pin and Box::pin" file-name="src/main.rs">

```rust,editable
{{#rustdoc_include ../listings/ch17-async-await/listing-17-18/src/main.rs:here}}
```

</Listing>

**Performance optimization**: Use `pin!` macro to avoid heap allocation:

<Listing number="17-19" caption="Using pin! macro for stack allocation" file-name="src/main.rs">

```rust,editable
{{#rustdoc_include ../listings/ch17-async-await/listing-17-19/src/main.rs:here}}
```

</Listing>

### Tradeoffs

**Same types + dynamic count**: Use `join_all` with trait objects and pinning
**Different types + known count**: Use `join!` macro or specific `join` functions
**Performance**: Stack pinning (`pin!`) vs heap allocation (`Box::pin`)

<Listing number="17-20" caption="Mixing different future output types" file-name="src/main.rs">

```rust,editable
{{#rustdoc_include ../listings/ch17-async-await/listing-17-20/src/main.rs:here}}
```

</Listing>

The `join!` macro handles heterogeneous types but requires compile-time knowledge of count and types.

### Racing Futures

`race` returns the first completed future, discarding others:

<Listing number="17-21" caption="Racing futures for first completion" file-name="src/main.rs">

```rust,editable
{{#rustdoc_include ../listings/ch17-async-await/listing-17-21/src/main.rs:here}}
```

</Listing>

**Key point**: Futures only yield at await points. CPU-intensive work without await points will block other futures (cooperative multitasking).

### Yielding Control

Force yield points with `yield_now()`:

<Listing number="17-22" caption="Simulating long-running operations" file-name="src/main.rs">

```rust,editable
{{#rustdoc_include ../listings/ch17-async-await/listing-17-22/src/main.rs:slow}}
```

</Listing>

<Listing number="17-23" caption="Blocking behavior without yield points" file-name="src/main.rs">

```rust,editable
{{#rustdoc_include ../listings/ch17-async-await/listing-17-23/src/main.rs:slow-futures}}
```

</Listing>

<Listing number="17-24" caption="Adding yield points between operations" file-name="src/main.rs">

```rust,editable
{{#rustdoc_include ../listings/ch17-async-await/listing-17-24/src/main.rs:here}}
```

</Listing>

<Listing number="17-25" caption="Using yield_now for explicit control yielding" file-name="src/main.rs">

```rust,editable
{{#rustdoc_include ../listings/ch17-async-await/listing-17-25/src/main.rs:yields}}
```

</Listing>

**Performance comparison**: `yield_now()` is significantly faster than `sleep()` for cooperation points.

<Listing number="17-26" caption="Benchmarking yield strategies" file-name="src/main.rs">

```rust,editable
{{#rustdoc_include ../listings/ch17-async-await/listing-17-26/src/main.rs:here}}
```

</Listing>

### Building Async Abstractions

Compose futures to create higher-level abstractions:

<Listing number="17-27" caption="Using a timeout abstraction" file-name="src/main.rs">

```rust,editable,ignore
{{#rustdoc_include ../listings/ch17-async-await/listing-17-27/src/main.rs:here}}
```

</Listing>

<Listing number="17-28" caption="Timeout function signature" file-name="src/main.rs">

```rust,editable,ignore
{{#rustdoc_include ../listings/ch17-async-await/listing-17-28/src/main.rs:declaration}}
```

</Listing>

<Listing number="17-29" caption="Implementing timeout with race and sleep" file-name="src/main.rs">

```rust,editable
{{#rustdoc_include ../listings/ch17-async-await/listing-17-29/src/main.rs:implementation}}
```

</Listing>

**Pattern**: Use `race` to implement timeout logic. First argument gets priority due to non-fair scheduling.

## Key Patterns

- **Dynamic collections**: Trait objects + pinning for runtime-determined future sets
- **Static collections**: `join!` macro for compile-time known futures
- **Cooperation**: Insert `yield_now()` in CPU-intensive loops
- **Composition**: Build complex async behavior from simpler primitives
- **Performance**: Choose between stack (`pin!`) and heap (`Box::pin`) allocation

[dyn]: ch12-03-improving-error-handling-and-modularity.html
[enum-alt]: ch08-01-vectors.html#using-an-enum-to-store-multiple-types
[async-program]: ch17-01-futures-and-syntax.html#our-first-async-program
[iterator-trait]: ch13-02-iterators.html#the-iterator-trait-and-the-next-method
