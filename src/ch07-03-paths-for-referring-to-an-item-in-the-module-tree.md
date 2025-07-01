## Paths for Referring to an Item in the Module Tree

Paths specify item locations within the module tree using `::` syntax:

- **Absolute paths**: Start from crate root with `crate::` (like absolute file paths)
- **Relative paths**: Start from current module using `self`, `super`, or module names

<Listing number="7-3" file-name="src/lib.rs" caption="Calling the `add_to_waitlist` function using absolute and relative paths">

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-03/src/lib.rs}}
```

</Listing>

This code fails to compile because `hosting` module is private:

<Listing number="7-4" caption="Compiler errors from building the code in Listing 7-3">

```console
{{#include ../listings/ch07-managing-growing-projects/listing-07-03/output.txt}}
```

</Listing>

### Privacy Rules

Rust's privacy model differs from JavaScript/TypeScript:
- **Default private**: All items are private to parent modules by default
- **Child access**: Child modules can access ancestor module items
- **Parent restriction**: Parent modules cannot access private child items

This enforces encapsulation—internal implementation details remain hidden unless explicitly exposed.

### Exposing Paths with the `pub` Keyword

Making the `hosting` module public isn't sufficient:

<Listing number="7-5" file-name="src/lib.rs" caption="Declaring the `hosting` module as `pub` to use it from `eat_at_restaurant`">

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-05/src/lib.rs:here}}
```

</Listing>

<Listing number="7-6" caption="Compiler errors from building the code in Listing 7-5">

```console
{{#include ../listings/ch07-managing-growing-projects/listing-07-05/output.txt}}
```

</Listing>

The `pub` keyword on a module only allows access to the module itself, not its contents. Both the module and the function must be public:

<Listing number="7-7" file-name="src/lib.rs" caption="Adding the `pub` keyword to `mod hosting` and `fn add_to_waitlist` lets us call the function from `eat_at_restaurant`">

```rust,editable,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-07/src/lib.rs:here}}
```

</Listing>

### Starting Relative Paths with `super`

Use `super` to access parent module items (like `../` in file paths):

<Listing number="7-8" file-name="src/lib.rs" caption="Calling a function using a relative path starting with `super`">

```rust,editable,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-08/src/lib.rs}}
```

</Listing>

### Making Structs and Enums Public

**Structs**: Fields remain private by default even when struct is public

<Listing number="7-9" file-name="src/lib.rs" caption="A struct with some public fields and some private fields">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-09/src/lib.rs}}
```

</Listing>

Structs with private fields require constructor functions since you cannot directly instantiate them.

**Enums**: All variants become public when enum is public

<Listing number="7-10" file-name="src/lib.rs" caption="Designating an enum as public makes all its variants public.">

```rust,editable,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-10/src/lib.rs}}
```

</Listing>

[pub]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html#exposing-paths-with-the-pub-keyword
[api-guidelines]: https://rust-lang.github.io/api-guidelines/
[ch12]: ch12-00-an-io-project.html
