## Generic Data Types

Generics allow you to write code that works with multiple types while maintaining type safety. You can use generics in function signatures, structs, enums, and methods.

### Function Definitions

Generic functions use type parameters in angle brackets. Consider these two functions that find the largest value:

<Listing number="10-4" file-name="src/main.rs" caption="Two functions that differ only in their types">

```rust,editable
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-04/src/main.rs:here}}
```

</Listing>

Combine them using a generic type parameter `T`:

```rust,editable,ignore
fn largest<T>(list: &[T]) -> &T {
```

This declares a function generic over type `T`, taking a slice of `T` and returning a reference to `T`.

<Listing number="10-5" file-name="src/main.rs" caption="Generic `largest` function (doesn't compile yet)">

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-05/src/main.rs}}
```

</Listing>

This fails to compile because not all types support comparison:

```console
{{#include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-05/output.txt}}
```

The error indicates you need trait bounds (covered in the next section) to constrain `T` to comparable types.

### Struct Definitions

Structs can be generic over one or more types:

<Listing number="10-6" file-name="src/main.rs" caption="Generic `Point<T>` struct">

```rust,editable
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-06/src/main.rs}}
```

</Listing>

Both fields must be the same type `T`. This won't compile:

<Listing number="10-7" file-name="src/main.rs" caption="Fields must have the same generic type">

```rust,editable,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-07/src/main.rs}}
```

</Listing>

For different types, use multiple type parameters:

<Listing number="10-8" file-name="src/main.rs" caption="Generic over two types">

```rust,editable
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-08/src/main.rs}}
```

</Listing>

### Enum Definitions

Enums can hold generic data types:

```rust,editable
enum Option<T> {
    Some(T),
    None,
}
```

Multiple type parameters are common:

```rust,editable
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

### Method Definitions

Implement methods on generic structs by declaring the type parameter after `impl`:

<Listing number="10-9" file-name="src/main.rs" caption="Method on generic struct">

```rust,editable
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-09/src/main.rs}}
```

</Listing>

You can implement methods only for specific concrete types:

<Listing number="10-10" file-name="src/main.rs" caption="Implementation for specific type">

```rust,editable
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-10/src/main.rs:here}}
```

</Listing>

Method type parameters can differ from struct type parameters:

<Listing number="10-11" file-name="src/main.rs" caption="Method with different generic types">

```rust,editable
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-11/src/main.rs}}
```

</Listing>

### Performance

Generics have zero runtime cost due to **monomorphization**—the compiler generates specific versions for each concrete type used:

```rust,editable
let integer = Some(5);
let float = Some(5.0);
```

The compiler generates specialized versions:

```rust,editable
enum Option_i32 {
    Some(i32),
    None,
}

enum Option_f64 {
    Some(f64),
    None,
}

fn main() {
    let integer = Option_i32::Some(5);
    let float = Option_f64::Some(5.0);
}
```

This eliminates runtime overhead while maintaining type safety.
