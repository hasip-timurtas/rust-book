## Variables and Mutability

Variables are immutable by default in Rust. This design choice enforces memory safety and prevents data races in concurrent contexts.

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/src/main.rs}}
```

This produces a compile-time error:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/output.txt}}
```

To allow mutation, use the `mut` keyword:

```rust,editable
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/src/main.rs}}
```

### Constants

Constants differ from immutable variables in several ways:

- Always immutable (no `mut` allowed)
- Declared with `const` keyword
- Type annotation required
- Must be set to compile-time constant expressions
- Can be declared in any scope, including global

```rust,editable
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

Constants use `SCREAMING_SNAKE_CASE` by convention and are valid for the entire program duration within their scope.

### Shadowing

Rust allows variable shadowing - declaring a new variable with the same name:

```rust,editable
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/src/main.rs}}
```

Output:
```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/output.txt}}
```

Shadowing differs from mutation:
- Creates a new variable (enables type changes)
- Requires `let` keyword
- Variable remains immutable after transformations

```rust,editable
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-04-shadowing-can-change-types/src/main.rs:here}}
```

This is invalid with `mut`:

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/src/main.rs:here}}
```

Error:
```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/output.txt}}
```

[comparing-the-guess-to-the-secret-number]: ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[data-types]: ch03-02-data-types.html#data-types
[storing-values-with-variables]: ch02-00-guessing-game-tutorial.html#storing-values-with-variables
[const-eval]: ../reference/const_eval.html
