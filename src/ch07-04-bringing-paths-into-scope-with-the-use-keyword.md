## Bringing Paths into Scope with the `use` Keyword

The `use` keyword creates shortcuts to paths, reducing repetition. Think of it as creating local aliases, similar to import statements in JavaScript/TypeScript.

<Listing number="7-11" file-name="src/lib.rs" caption="Bringing a module into scope with `use`">

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-11/src/lib.rs}}
```

</Listing>

The `use` statement creates a shortcut valid only within its scope:

<Listing number="7-12" file-name="src/lib.rs" caption="A `use` statement only applies in the scope it's in.">

```rust,noplayground,test_harness,does_not_compile,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-12/src/lib.rs}}
```

</Listing>

### Idiomatic `use` Patterns

**Functions**: Import the parent module, not the function directly

```rust,noplayground,test_harness
// Idiomatic
use crate::front_of_house::hosting;
hosting::add_to_waitlist();

// Less clear where function is defined
use crate::front_of_house::hosting::add_to_waitlist;
add_to_waitlist();
```

**Structs/Enums/Types**: Import the full path

<Listing number="7-14" file-name="src/main.rs" caption="Bringing `HashMap` into scope in an idiomatic way">

```rust
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-14/src/main.rs}}
```

</Listing>

**Name conflicts**: Use parent modules to disambiguate

<Listing number="7-15" file-name="src/lib.rs" caption="Bringing two types with the same name into the same scope requires using their parent modules.">

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-15/src/lib.rs:here}}
```

</Listing>

### Providing New Names with the `as` Keyword

Create aliases to resolve naming conflicts:

<Listing number="7-16" file-name="src/lib.rs" caption="Renaming a type when it's brought into scope with the `as` keyword">

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-16/src/lib.rs:here}}
```

</Listing>

### Re-exporting Names with `pub use`

Make imported items available to external code (similar to TypeScript's `export { ... } from ...`):

<Listing number="7-17" file-name="src/lib.rs" caption="Making a name available for any code to use from a new scope with `pub use`">

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-17/src/lib.rs}}
```

</Listing>

Re-exporting allows exposing a different public API structure than your internal organization.

### Using External Packages

Add dependencies to `Cargo.toml`:

<Listing file-name="Cargo.toml">

```toml
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-02/Cargo.toml:9:}}
```

</Listing>

Then import and use:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-03/src/main.rs:ch07-04}}
```

Standard library (`std`) is available automatically but still requires explicit `use` statements.

### Nested Paths

Combine multiple imports from the same crate/module:

<Listing number="7-18" file-name="src/main.rs" caption="Specifying a nested path to bring multiple items with the same prefix into scope">

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-18/src/main.rs:here}}
```

</Listing>

Use `self` to import both the parent and child:

<Listing number="7-20" file-name="src/lib.rs" caption="Combining the paths in Listing 7-19 into one `use` statement">

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-20/src/lib.rs}}
```

</Listing>

### The Glob Operator

Import all public items (use sparingly):

```rust
use std::collections::*;
```

Commonly used in test modules and when implementing prelude patterns. Can make code less clear and cause naming conflicts.

[ch14-pub-use]: ch14-02-publishing-to-crates-io.html#exporting-a-convenient-public-api-with-pub-use
[rand]: ch02-00-guessing-game-tutorial.html#generating-a-random-number
[writing-tests]: ch11-01-writing-tests.html#how-to-write-tests
