<!-- Old heading. Do not remove or links may break. -->

<a id="the-match-control-flow-operator"></a>

## The `match` Control Flow Construct

`match` enables pattern matching against enum variants with compile-time exhaustiveness checking. Each pattern can extract data from variants and execute corresponding code.

### Basic Syntax

<Listing number="6-3" caption="An enum and a `match` expression that has the variants of the enum as its patterns">

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-03/src/main.rs:here}}
```

</Listing>

**Structure:**
- `match` keyword followed by expression to match
- Arms with pattern `=>` code syntax
- Arms evaluated in order until match found
- Matching arm's code becomes the expression's return value

For multi-line arm code, use curly braces:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-08-match-arm-multiple-lines/src/main.rs:here}}
```

### Pattern Binding

Match arms can bind to data within enum variants:

<Listing number="6-4" caption="A `Coin` enum in which the `Quarter` variant also holds a `UsState` value">

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-04/src/main.rs:here}}
```

</Listing>

Extract data using variable binding in patterns:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-09-variable-in-pattern/src/main.rs:here}}
```

The `state` variable binds to the inner value from `Coin::Quarter(UsState::Alaska)`, making it accessible within the arm.

### Matching `Option<T>`

Common pattern for safe null handling:

<Listing number="6-5" caption="A function that uses a `match` expression on an `Option<i32>`">

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-05/src/main.rs:here}}
```

</Listing>

**Execution flow:**
- `Some(5)` matches `Some(i)`, binding `i = 5`
- `None` matches `None` pattern directly
- Each variant must be handled explicitly

### Exhaustiveness

Match expressions must handle all possible variants:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-10-non-exhaustive-match/src/main.rs:here}}
```

Compiler error:
```console
{{#include ../listings/ch06-enums-and-pattern-matching/no-listing-10-non-exhaustive-match/output.txt}}
```

Exhaustiveness prevents runtime errors from unhandled cases, especially critical for `Option<T>` null safety.

### Catch-All Patterns

Handle specific values with catch-all for remaining cases:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-15-binding-catchall/src/main.rs:here}}
```

Use `_` placeholder when catch-all value isn't needed:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-16-underscore-catchall/src/main.rs:here}}
```

Use unit value `()` for no-op catch-all:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-17-underscore-unit/src/main.rs:here}}
```

**Key Points:**
- Catch-all patterns must be last (evaluated in order)
- `_` ignores the value without binding
- `()` performs no operation
- Catch-all ensures exhaustiveness for large value spaces

[tuples]: ch03-02-data-types.html#the-tuple-type
[ch19-00-patterns]: ch19-00-patterns.html
