## Storing UTF-8 Encoded Text with Strings

Rust has two main string types: `str` (string slice, usually seen as `&str`) and `String` (owned, growable). Both are UTF-8 encoded.

- `&str` - Immutable reference to string data, often stored in binary
- `String` - Growable, heap-allocated, owned string type

### Creating Strings

```rust,editable
// Empty string
let mut s = String::new();

// From string literal
let s = "initial contents".to_string();
let s = String::from("initial contents");

// UTF-8 support
let hello = String::from("Здравствуйте");
let hello = String::from("नमस्ते");
```

### Updating Strings

```rust,editable
let mut s = String::from("foo");

// Append string slice
s.push_str("bar");

// Append single character  
s.push('l');

// Concatenation with + operator
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2; // s1 is moved, s2 can still be used

// Multiple concatenation with format! macro
let s = format!("{s1}-{s2}-{s3}");
```

The `+` operator uses the `add` method with signature `fn add(self, s: &str) -> String`, taking ownership of the left operand.

### String Indexing is Not Allowed

```rust,editable,ignore,does_not_compile
let s1 = String::from("hello");
let h = s1[0]; // Error: doesn't implement Index<{integer}>
```

**Reasons:**
1. **Variable byte encoding** - UTF-8 characters can be 1-4 bytes
2. **Performance** - Would require O(n) time to find nth character
3. **Ambiguity** - What should be returned? Byte, scalar value, or grapheme cluster?

### Internal Representation

Strings are `Vec<u8>` wrappers with UTF-8 guarantees:

```rust,editable
let hello = "Здравствуйте"; // 12 chars, 24 bytes
let hello = "नमस्ते";      // 4 graphemes, 6 scalar values, 18 bytes
```

### String Slicing

Use ranges carefully - slicing must occur at valid UTF-8 boundaries:

```rust,editable
let hello = "Здравствуйте";
let s = &hello[0..4]; // "Зд" (each char is 2 bytes)

// This would panic at runtime:
// let s = &hello[0..1]; // Invalid UTF-8 boundary
```

### Iterating Over Strings

```rust,editable
// By Unicode scalar values (char)
for c in "Зд".chars() {
    println!("{c}"); // З, д
}

// By bytes
for b in "Зд".bytes() {
    println!("{b}"); // 208, 151, 208, 180
}

// For grapheme clusters, use external crates
```

### String Methods

Key methods for string manipulation:

```rust,editable
let s = String::from("hello world");

// Search
s.contains("world");  // true

// Replace
s.replace("world", "rust"); // "hello rust"

// Split
s.split_whitespace(); // iterator over words
```

Unlike JavaScript strings, Rust strings are not indexed due to UTF-8 complexity. Use iteration methods or careful slicing instead.
