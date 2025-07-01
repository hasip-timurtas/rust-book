## What Is Ownership?

Ownership is Rust's compile-time memory management system. Unlike garbage collection (JavaScript/Node.js) or manual allocation (C++), Rust enforces memory safety through ownership rules checked at compile time with zero runtime overhead.

### Stack vs Heap

**Stack**: LIFO storage for fixed-size data. Fast allocation/deallocation, automatic cleanup when scope ends.

**Heap**: Dynamic allocation for variable-size data. Requires explicit management, slower access due to pointer indirection.

In Node.js, the garbage collector handles heap cleanup. In Rust, ownership rules determine when heap data is freed.

### Ownership Rules

1. Each value has exactly one owner
2. Only one owner can exist at any time  
3. Value is dropped when owner goes out of scope

### Variable Scope

```rust
{                      // s is not valid here, it's not yet declared
    let s = "hello";   // s is valid from this point forward
    // do stuff with s
}                      // scope ends, s is no longer valid
```

### The `String` Type

String literals (`"hello"`) are immutable and stored in the binary. `String` is heap-allocated and mutable:

```rust
let mut s = String::from("hello");
s.push_str(", world!"); // append to string
```

`String` structure (stack data pointing to heap):
- `ptr`: pointer to heap data
- `len`: current length in bytes
- `capacity`: allocated capacity

### Move Semantics

```rust
let s1 = String::from("hello");
let s2 = s1; // s1 is moved to s2, s1 is now invalid
```

This is a **move**, not a copy. The heap data isn't duplicated; ownership transfers from `s1` to `s2`. This prevents double-free errors.

Unlike JavaScript where assignment creates references, Rust moves ownership to maintain memory safety.

### Clone vs Copy

**Deep copy** (expensive):
```rust
let s1 = String::from("hello");
let s2 = s1.clone(); // both s1 and s2 are valid
```

**Copy trait** (cheap, for stack-only types):
```rust
let x = 5;
let y = x; // both x and y are valid (i32 implements Copy)
```

Types implementing `Copy`: integers, booleans, floats, chars, tuples of `Copy` types.

### Functions and Ownership

Passing to functions follows the same rules:

```rust
fn main() {
    let s = String::from("hello");
    takes_ownership(s);    // s is moved, no longer valid
    
    let x = 5;
    makes_copy(x);         // x is copied, still valid
}

fn takes_ownership(some_string: String) {
    println!("{}", some_string);
} // some_string goes out of scope and is dropped

fn makes_copy(some_integer: i32) {
    println!("{}", some_integer);
} // some_integer goes out of scope, nothing special happens
```

**Returning values** also transfers ownership:

```rust
fn gives_ownership() -> String {
    String::from("hello") // moved to caller
}

fn takes_and_gives_back(a_string: String) -> String {
    a_string // moved to caller
}
```

**Key insight**: This ownership system eliminates entire classes of bugs (use-after-free, double-free, memory leaks) at compile time, providing the control of systems languages with the safety of high-level languages.

[data-types]: ch03-02-data-types.html
[method-syntax]: ch05-03-method-syntax.html
[paths-module-tree]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
[drop]: ../std/ops/trait.Drop.html#tymethod.drop
[traits]: ch10-02-traits.html
[derivable-traits]: appendix-03-derivable-traits.html
