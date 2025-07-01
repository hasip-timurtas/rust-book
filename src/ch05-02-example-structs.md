## An Example Program Using Structs

This example demonstrates the progression from loose parameters to structured data using a rectangle area calculator.

### Initial Implementation

<Listing number="5-8" file-name="src/main.rs" caption="Calculating the area of a rectangle specified by separate width and height variables">

```rust,editable
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-08/src/main.rs:all}}
```

</Listing>

Output:
```console
{{#include ../listings/ch05-using-structs-to-structure-related-data/listing-05-08/output.txt}}
```

The function signature `fn area(width: u32, height: u32) -> u32` doesn't express the relationship between parameters. This creates maintenance issues and potential parameter ordering mistakes.

### Refactoring with Tuples

<Listing number="5-9" file-name="src/main.rs" caption="Specifying the width and height of the rectangle with a tuple">

```rust,editable
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-09/src/main.rs}}
```

</Listing>

Tuples group related data but lose semantic meaning. Index-based access (`dimensions.0`, `dimensions.1`) reduces code clarity and introduces potential errors.

### Refactoring with Structs

Structs provide both grouping and semantic meaning:

<Listing number="5-10" file-name="src/main.rs" caption="Defining a `Rectangle` struct">

```rust,editable
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-10/src/main.rs}}
```

</Listing>

Benefits:
- Clear function signature expressing intent
- Named field access (`rectangle.width`, `rectangle.height`)
- Borrowing instead of ownership transfer
- Type safety preventing parameter confusion

### Debug Output

Structs don't implement `Display` by default. Use `Debug` trait for development output:

<Listing number="5-11" file-name="src/main.rs" caption="Attempting to print a `Rectangle` instance">

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-11/src/main.rs}}
```

</Listing>

Error:
```text
{{#include ../listings/ch05-using-structs-to-structure-related-data/listing-05-11/output.txt:3}}
```

Enable debug output with `#[derive(Debug)]`:

<Listing number="5-12" file-name="src/main.rs" caption="Adding the attribute to derive the `Debug` trait and printing the `Rectangle` instance using debug formatting">

```rust,editable
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-12/src/main.rs}}
```

</Listing>

Output:
```console
{{#include ../listings/ch05-using-structs-to-structure-related-data/listing-05-12/output.txt}}
```

Pretty-print with `{:#?}`:
```console
{{#include ../listings/ch05-using-structs-to-structure-related-data/output-only-02-pretty-debug/output.txt}}
```

### Debug Macro

The `dbg!` macro provides file/line information and returns ownership:

```rust,editable
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-05-dbg-macro/src/main.rs}}
```

Output:
```console
{{#include ../listings/ch05-using-structs-to-structure-related-data/no-listing-05-dbg-macro/output.txt}}
```

**Key Differences:**
- `println!` takes references, outputs to stdout
- `dbg!` takes ownership (returns it), outputs to stderr with location info

Other derivable traits are listed in [Appendix C][app-c]. Custom trait implementation is covered in Chapter 10.

[the-tuple-type]: ch03-02-data-types.html#the-tuple-type
[app-c]: appendix-03-derivable-traits.md
[println]: ../std/macro.println.html
[dbg]: ../std/macro.dbg.html
[err]: ch12-06-writing-to-stderr-instead-of-stdout.html
[attributes]: ../reference/attributes.html
