## Defining an Enum

Enums model data that can be one of several distinct variants. Each variant can carry different types and amounts of data.

### Basic Syntax

```rust,editable
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:def}}
```

Enum values are created using namespace syntax:

```rust,editable
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:instance}}
```

Both values have type `IpAddrKind`, enabling uniform function parameters:

```rust,editable
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:fn}}
```

### Data in Variants

Variants can carry associated data directly:

```rust,editable
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-02-enum-with-data/src/main.rs:here}}
```

Variant names become constructor functions: `IpAddr::V4()` takes a `String` and returns an `IpAddr`.

Each variant can have different data types:

```rust,editable
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-03-variants-with-different-data/src/main.rs:here}}
```

The standard library's `IpAddr` uses structs within variants:

```rust,editable
struct Ipv4Addr {
    // --snip--
}

struct Ipv6Addr {
    // --snip--
}

enum IpAddr {
    V4(Ipv4Addr),
    V6(Ipv6Addr),
}
```

### Complex Variants

<Listing number="6-2" caption="A `Message` enum whose variants each store different amounts and types of values">

```rust,editable
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-02/src/main.rs:here}}
```

</Listing>

This enum replaces what would require multiple struct types:

```rust,editable
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-04-structs-similar-to-message-enum/src/main.rs:here}}
```

Enums enable functions accepting any variant as a single type parameter, unlike separate structs.

### Methods on Enums

Enums support methods via `impl` blocks:

```rust,editable
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-05-methods-on-enums/src/main.rs:here}}
```

### The `Option<T>` Enum

`Option<T>` encodes nullable values safely in the type system:

```rust,editable
enum Option<T> {
    None,
    Some(T),
}
```

`Option<T>` is included in the prelude. Variants `Some` and `None` are directly accessible without the `Option::` prefix.

Examples:
```rust,editable
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-06-option-examples/src/main.rs:here}}
```

### Option vs Null Safety

Unlike TypeScript's `null` and `undefined`, Rust prevents direct operations on `Option<T>` values:

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-07-cant-use-option-directly/src/main.rs:here}}
```

Compiler error:
```console
{{#include ../listings/ch06-enums-and-pattern-matching/no-listing-07-cant-use-option-directly/output.txt}}
```

This compile-time check eliminates null pointer exceptions. You must explicitly handle both `Some(T)` and `None` cases before accessing the inner value.

**Key Benefits:**
- Explicit opt-in for nullable types via `Option<T>`
- Compile-time guarantee that non-`Option` types are never null
- Forces explicit null checking at usage sites

To extract values from `Option<T>`, use pattern matching (`match`) or `Option<T>` methods. Pattern matching provides exhaustive case handling and value extraction from variants.

[IpAddr]: ../std/net/enum.IpAddr.html
[option]: ../std/option/enum.Option.html
[docs]: ../std/option/enum.Option.html
