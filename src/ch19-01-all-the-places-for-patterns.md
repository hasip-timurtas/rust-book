## Pattern Usage Contexts

### `match` Arms

```
match VALUE {
    PATTERN => EXPRESSION,
    PATTERN => EXPRESSION,
}
```

`match` expressions must be exhaustive. Use `_` as a catchall pattern:

```rust,ignore
match x {
    None => None,
    Some(i) => Some(i + 1),
}
```

### Conditional `if let` Expressions

More flexible than `match` for single-pattern matching:

<Listing number="19-1" file-name="src/main.rs" caption="Mixing `if let`, `else if`, `else if let`, and `else`">

```rust
{{#rustdoc_include ../listings/ch19-patterns-and-matching/listing-19-01/src/main.rs}}
```

</Listing>

`if let` doesn't check exhaustiveness, unlike `match`. Can introduce shadowed variables within the conditional scope.

### `while let` Conditional Loops

<Listing number="19-2" caption="Using a `while let` loop to print values for as long as `rx.recv()` returns `Ok`">

```rust
{{#rustdoc_include ../listings/ch19-patterns-and-matching/listing-19-2/src/main.rs:here}}
```

</Listing>

Continues until the pattern fails to match.

### `for` Loops

The pattern follows `for`:

<Listing number="19-3" caption="Using a pattern in a `for` loop to destructure a tuple">

```rust
{{#rustdoc_include ../listings/ch19-patterns-and-matching/listing-19-3/src/main.rs:here}}
```

</Listing>

Output:
```console
{{#include ../listings/ch19-patterns-and-matching/listing-19-3/output.txt}}
```

### `let` Statements

Every `let` statement uses a pattern:

```rust
let x = 5;  // x is a pattern
```

Formal syntax: `let PATTERN = EXPRESSION;`

Destructuring example:

<Listing number="19-4" caption="Using a pattern to destructure a tuple and create three variables at once">

```rust
{{#rustdoc_include ../listings/ch19-patterns-and-matching/listing-19-4/src/main.rs:here}}
```

</Listing>

Pattern elements must match exactly:

<Listing number="19-5" caption="Incorrectly constructing a pattern whose variables don't match the number of elements in the tuple">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch19-patterns-and-matching/listing-19-5/src/main.rs:here}}
```

</Listing>

Error:
```console
{{#include ../listings/ch19-patterns-and-matching/listing-19-5/output.txt}}
```

### Function Parameters

Function parameters are patterns:

<Listing number="19-6" caption="A function signature uses patterns in the parameters">

```rust
{{#rustdoc_include ../listings/ch19-patterns-and-matching/listing-19-6/src/main.rs:here}}
```

</Listing>

Destructuring in function signatures:

<Listing number="19-7" file-name="src/main.rs" caption="A function with parameters that destructure a tuple">

```rust
{{#rustdoc_include ../listings/ch19-patterns-and-matching/listing-19-07/src/main.rs}}
```

</Listing>

Prints: `Current location: (3, 5)`

Closure parameters work the same way as function parameters.

[ignoring-values-in-a-pattern]: ch19-03-pattern-syntax.html#ignoring-values-in-a-pattern
