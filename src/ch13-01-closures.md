<!-- Old heading. Do not remove or links may break. -->

<a id="closures-anonymous-functions-that-can-capture-their-environment"></a>

## Closures: Anonymous Functions That Capture Their Environment

Closures are anonymous functions that capture values from their enclosing scope. Unlike regular functions, closures can access variables from the environment where they're defined.

<!-- Old headings. Do not remove or links may break. -->

<a id="creating-an-abstraction-of-behavior-with-closures"></a>
<a id="refactoring-using-functions"></a>
<a id="refactoring-with-closures-to-store-code"></a>

### Environment Capture

Consider this scenario: a shirt giveaway system that assigns colors based on user preferences or inventory levels.

<Listing number="13-1" file-name="src/main.rs" caption="Shirt company giveaway situation">

```rust,noplayground
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-01/src/main.rs}}
```

</Listing>

The closure `|| self.most_stocked()` captures an immutable reference to `self`. The `unwrap_or_else` method calls this closure only when the `Option` is `None`, demonstrating lazy evaluation.

```console
{{#include ../listings/ch13-functional-features/listing-13-01/output.txt}}
```

### Type Inference and Annotations

Closures don't require explicit type annotations in most cases—the compiler infers types from usage:

<Listing number="13-2" file-name="src/main.rs" caption="Optional type annotations in closures">

```rust
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-02/src/main.rs:here}}
```

</Listing>

Syntax comparison:
```rust,ignore
fn  add_one_v1   (x: u32) -> u32 { x + 1 }
let add_one_v2 = |x: u32| -> u32 { x + 1 };
let add_one_v3 = |x|             { x + 1 };
let add_one_v4 = |x|               x + 1  ;
```

Once a closure's types are inferred from first usage, they're locked in:

<Listing number="13-3" file-name="src/main.rs" caption="Type inference locks closure parameter types">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-03/src/main.rs:here}}
```

</Listing>

```console
{{#include ../listings/ch13-functional-features/listing-13-03/output.txt}}
```

### Capture Modes

Closures automatically choose the minimal capture mode needed:

**Immutable borrow:**
<Listing number="13-4" file-name="src/main.rs" caption="Immutable reference capture">

```rust
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-04/src/main.rs}}
```

</Listing>

**Mutable borrow:**
<Listing number="13-5" file-name="src/main.rs" caption="Mutable reference capture">

```rust
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-05/src/main.rs}}
```

</Listing>

**Move semantics:**
Use `move` to force ownership transfer, commonly needed for thread boundaries:

<Listing number="13-6" file-name="src/main.rs" caption="Force ownership with `move`">

```rust
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-06/src/main.rs}}
```

</Listing>

<!-- Old headings. Do not remove or links may break. -->

<a id="storing-closures-using-generic-parameters-and-the-fn-traits"></a>
<a id="limitations-of-the-cacher-implementation"></a>
<a id="moving-captured-values-out-of-the-closure-and-the-fn-traits"></a>

### Fn Traits

Closures implement one or more of these traits based on how they handle captured values:

- **`FnOnce`**: Callable once, consumes captured values. All closures implement this.
- **`FnMut`**: Callable multiple times, may mutate captured values.
- **`Fn`**: Callable multiple times without mutation, safe for concurrent access.

Example from `Option<T>`:
```rust,ignore
impl<T> Option<T> {
    pub fn unwrap_or_else<F>(self, f: F) -> T
    where
        F: FnOnce() -> T
    {
        match self {
            Some(x) => x,
            None => f(),
        }
    }
}
```

`sort_by_key` requires `FnMut` since it calls the closure multiple times:

<Listing number="13-7" file-name="src/main.rs" caption="Using `sort_by_key` with `FnMut`">

```rust
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-07/src/main.rs}}
```

</Listing>

This won't compile because the closure moves `value` out, implementing only `FnOnce`:

<Listing number="13-8" file-name="src/main.rs" caption="FnOnce closure incompatible with `sort_by_key`">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-08/src/main.rs}}
```

</Listing>

```console
{{#include ../listings/ch13-functional-features/listing-13-08/output.txt}}
```

Correct approach using mutable reference:

<Listing number="13-9" file-name="src/main.rs" caption="FnMut closure with mutable reference">

```rust
{{#rustdoc_include ../listings/ch13-functional-features/listing-13-09/src/main.rs}}
```

</Listing>

[unwrap-or-else]: ../std/option/enum.Option.html#method.unwrap_or_else
