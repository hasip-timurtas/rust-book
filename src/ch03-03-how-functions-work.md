## Functions

Functions in Rust use snake_case convention and require explicit type annotations for parameters (unlike TypeScript's optional inference).

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-16-functions/src/main.rs}}
```

Functions are defined with `fn` and can be declared in any order within scope. The compiler doesn't care about declaration order, similar to hoisting in JavaScript.

### Parameters

Unlike TypeScript, parameter types are mandatory:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/src/main.rs}}
```

Multiple parameters:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/src/main.rs}}
```

### Statements vs Expressions

Critical distinction for Rust (unlike TypeScript where nearly everything is an expression):

- **Statements**: Perform actions, return no value (`let y = 6;`)
- **Expressions**: Evaluate to values (`5 + 6`, function calls, blocks)

<Listing number="3-1" file-name="src/main.rs" caption="Statement example">

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-01/src/main.rs}}
```

</Listing>

This won't compile (unlike JavaScript/TypeScript):

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/src/main.rs}}
```

Blocks are expressions:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-20-blocks-are-expressions/src/main.rs}}
```

The block evaluates to `4` because `x + 1` lacks a semicolon. **Adding a semicolon converts an expression to a statement**.

### Return Values

Return type annotation required after `->`. Functions return the last expression implicitly (no `return` keyword needed):

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/src/main.rs}}
```

Parameter and return example:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-22-function-parameter-and-return/src/main.rs}}
```

**Critical**: Adding a semicolon to the return expression breaks the return:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/src/main.rs}}
```

This fails because statements return `()` (unit type), not the expected `i32`. The semicolon behavior is fundamentally different from TypeScript where semicolons are purely syntactic.
