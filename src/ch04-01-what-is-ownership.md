## What Is Ownership?

Ownership is Rust's memory management system with compile-time rules that prevent memory safety issues without runtime overhead. Unlike garbage-collected languages, Rust determines memory cleanup at compile time.

> ### Stack vs Heap
>
> **Stack**: LIFO structure for fixed-size data. Fast allocation/deallocation, automatic cleanup.
> **Heap**: Runtime allocation for dynamic-size data. Returns pointer, requires explicit management.
>
> Stack access is faster due to locality. Heap access requires pointer dereferencing. Ownership primarily manages heap data.

### Ownership Rules

- Each value has exactly one owner
- Only one owner at a time
- Value is dropped when owner goes out of scope

### Variable Scope

```rust
let s = "hello";
```

String literals are immutable and stack-allocated. The variable `s` is valid from declaration until scope end.

<Listing number="4-1" caption="A variable and the scope in which it is valid">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-01/src/main.rs:here}}
```

</Listing>

### The `String` Type

`String` manages heap-allocated, mutable text data:

```rust
let s = String::from("hello");
```

Unlike string literals, `String` can be mutated and has dynamic size:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-01-can-mutate-string/src/main.rs:here}}
```

### Memory and Allocation

`String` requires heap allocation for dynamic content. Rust automatically calls `drop` when the owner goes out of scope—similar to RAII in C++.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-02-string-scope/src/main.rs:here}}
```

#### Variables and Data Interacting with Move

Simple stack values are copied:

<Listing number="4-2" caption="Assigning the integer value of variable `x` to `y`">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-02/src/main.rs:here}}
```

</Listing>

Heap-allocated values are moved:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-03-string-move/src/main.rs:here}}
```

`String` contains three parts on the stack: pointer, length, capacity. Assignment copies these metadata but not heap data. Rust invalidates the original variable to prevent double-free errors.

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/src/main.rs:here}}
```

Error: value used after move. This prevents the double-free memory safety issue.

#### Scope and Assignment

Assigning a new value immediately drops the previous value:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-04b-replacement-drop/src/main.rs:here}}
```

#### Variables and Data Interacting with Clone

For explicit deep copying:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-05-clone/src/main.rs:here}}
```

`clone()` is expensive—it copies heap data.

#### Stack-Only Data: Copy

Types implementing `Copy` trait are duplicated instead of moved:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-06-copy/src/main.rs:here}}
```

`Copy` types include:
- Integer types (`u32`, `i32`, etc.)
- Boolean (`bool`)
- Floating-point types (`f64`)
- Character (`char`)
- Tuples of `Copy` types

Types with `Drop` trait cannot implement `Copy`.

### Ownership and Functions

Function calls behave like assignment—values are moved or copied:

<Listing number="4-3" file-name="src/main.rs" caption="Functions with ownership and scope annotated">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-03/src/main.rs}}
```

</Listing>

### Return Values and Scope

Functions can transfer ownership via return values:

<Listing number="4-4" file-name="src/main.rs" caption="Transferring ownership of return values">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-04/src/main.rs}}
```

</Listing>

Returning ownership for every function parameter is verbose. Rust provides references for this use case:

<Listing number="4-5" file-name="src/main.rs" caption="Returning ownership of parameters">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-05/src/main.rs}}
```

</Listing>

[data-types]: ch03-02-data-types.html#data-types
[ch8]: ch08-02-strings.html
[traits]: ch10-02-traits.html
[derivable-traits]: appendix-03-derivable-traits.html
[method-syntax]: ch05-03-method-syntax.html#method-syntax
[paths-module-tree]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
[drop]: ../std/ops/trait.Drop.html#tymethod.drop
