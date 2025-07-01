## The Slice Type

Slices are references to contiguous sequences of elements in collections. They don't have ownership.

### String Slices

**Problem**: Returning indices creates disconnected state that can become invalid:

```rust
fn first_word(s: &String) -> usize {
    let bytes = s.as_bytes();
    
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return i;
        }
    }
    s.len()
}

// Usage issue:
let mut s = String::from("hello world");
let word = first_word(&s); // word = 5
s.clear(); // s is now empty, but word still contains 5 (bug!)
```

**Solution**: String slices (`&str`) maintain connection to source data:

```rust
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();
    
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }
    &s[..]
}
```

### Slice Syntax

```rust
let s = String::from("hello world");

let hello = &s[0..5];   // or &s[..5]
let world = &s[6..11];  // or &s[6..]
let whole = &s[0..s.len()]; // or &s[..]
```

Slices store a pointer to starting element and length.

### Compile-time Safety

```rust
let mut s = String::from("hello world");
let word = first_word(&s);
s.clear(); // ERROR: cannot borrow `s` as mutable while immutable borrow exists
println!("the first word is: {}", word);
```

The compiler enforces that the slice remains valid, preventing the class of bugs seen in the index-based approach.

### String Literals are Slices

```rust
let s = "Hello, world!"; // type is &str
```

String literals are slices pointing to binary data - immutable references.

### Improved Function Signatures

Accept `&str` instead of `&String` for flexibility:

```rust
fn first_word(s: &str) -> &str { // works with both &String and &str
    // implementation
}

// Usage:
let my_string = String::from("hello world");
let word = first_word(&my_string[..]);     // slice of String
let word = first_word(&my_string);         // &String (deref coercion)

let my_string_literal = "hello world";
let word = first_word(&my_string_literal[..]); // slice of literal
let word = first_word(my_string_literal);       // &str directly
```

### Array Slices

Works with any collection:

```rust
let a = [1, 2, 3, 4, 5];
let slice = &a[1..3]; // type: &[i32], value: [2, 3]
```

**Key benefit**: Slices provide safe, efficient access to subsequences without copying data, with compile-time guarantees that the underlying data remains valid.

[ch8]: ch08-00-common-collections.html
[ch13]: ch13-02-iterators.html
[ch6]: ch06-02-match.html#patterns-that-bind-to-values
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[deref-coercions]: ch15-02-deref.html#implicit-deref-coercions-with-functions-and-methods
