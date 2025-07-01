## The Slice Type

Slices reference contiguous sequences of elements in collections without taking ownership.

**Problem:** Write a function returning the first word from a space-separated string.

Without slices, we might return an index:

<Listing number="4-7" file-name="src/main.rs" caption="The `first_word` function that returns a byte index value into the `String` parameter">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-07/src/main.rs:here}}
```

</Listing>

Issues with index-based approach:

<Listing number="4-8" file-name="src/main.rs" caption="Storing the result from calling the `first_word` function and then changing the `String` contents">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-08/src/main.rs:here}}
```

</Listing>

The index becomes invalid after `s.clear()`, but the compiler can't catch this error.

### String Slices

A string slice (`&str`) references a portion of a `String`:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-17-slice/src/main.rs:here}}
```

Slice syntax: `&s[start..end]` where `end` is exclusive.

Shortcuts:
```rust
let s = String::from("hello");

let slice = &s[0..2];
let slice = &s[..2];    // Same as above

let slice = &s[3..len];
let slice = &s[3..];    // Same as above

let slice = &s[0..len];
let slice = &s[..];     // Entire string
```

Improved `first_word` using slices:

<Listing number="04-4" file-name="src/main.rs" caption="Example code in src/main.rs">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-18-first-word-slice/src/main.rs:here}}
```

</Listing>

Slices prevent the previous bug by enforcing borrowing rules:

<Listing number="04-4" file-name="src/main.rs" caption="Example code in src/main.rs">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-19-slice-error/src/main.rs:here}}
```

</Listing>

Compile error: cannot borrow as mutable while immutable reference exists.

#### String Literals as Slices

String literals are `&str` types—slices pointing to binary data:

```rust
let s = "Hello, world!"; // s is &str
```

#### String Slices as Parameters

Better function signature accepts both `&String` and `&str`:

<Listing number="4-9" caption="Improving the `first_word` function by using a string slice for the type of the `s` parameter">

```rust,ignore
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-09/src/main.rs:here}}
```

</Listing>

Usage:

<Listing number="04-4" file-name="src/main.rs" caption="Example code in src/main.rs">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-09/src/main.rs:usage}}
```

</Listing>

### Other Slices

Array slices work similarly:

```rust
let a = [1, 2, 3, 4, 5];
let slice = &a[1..3];   // Type: &[i32]
assert_eq!(slice, &[2, 3]);
```

## Summary

Ownership, borrowing, and slices provide compile-time memory safety without runtime overhead. These concepts affect many other Rust features throughout the language.

[ch13]: ch13-02-iterators.html
[ch6]: ch06-02-match.html#patterns-that-bind-to-values
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[deref-coercions]: ch15-02-deref.html#implicit-deref-coercions-with-functions-and-methods
