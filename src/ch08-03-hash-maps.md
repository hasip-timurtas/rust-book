## Storing Keys with Associated Values in Hash Maps

`HashMap<K, V>` stores key-value pairs using a hashing function. Keys must implement `Eq` and `Hash` traits.

### Creating Hash Maps

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

Hash maps are not included in the prelude, require explicit `use`. No built-in macro like `vec!`.

### Accessing Values

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);

// Returns Option<&V>
let score = scores.get("Blue").copied().unwrap_or(0);

// Iterate over key-value pairs
for (key, value) in &scores {
    println!("{key}: {value}");
}
```

### Ownership Rules

```rust
use std::collections::HashMap;

let field_name = String::from("Favorite color");
let field_value = String::from("Blue");

let mut map = HashMap::new();
map.insert(field_name, field_value);
// field_name and field_value are moved, no longer valid

// For Copy types like i32, values are copied
// For references, values must live as long as the hash map
```

### Updating Hash Maps

**Overwriting values:**
```rust
scores.insert(String::from("Blue"), 25); // Replaces existing value
```

**Insert only if key doesn't exist:**
```rust
scores.entry(String::from("Yellow")).or_insert(50);
scores.entry(String::from("Blue")).or_insert(50); // Won't insert, Blue exists
```

**Update based on old value:**
```rust
let text = "hello world wonderful world";
let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1; // Dereference to modify the value
}
```

The `entry` method returns an `Entry` enum that represents a value that might or might not exist. `or_insert` returns a mutable reference to the value.

### Performance Notes

- Default hasher is SipHash (cryptographically secure, DoS resistant)
- For performance-critical code, consider alternative hashers via `BuildHasher` trait
- All keys must be same type, all values must be same type

### Common Patterns

```rust
// Check if key exists
if scores.contains_key("Blue") { /* ... */ }

// Remove key-value pair
scores.remove("Blue");

// Get mutable reference to value
if let Some(score) = scores.get_mut("Blue") {
    *score += 10;
}

// Merge two hash maps
for (k, v) in other_map {
    scores.insert(k, v);
}
```

Hash maps provide O(1) average-case performance for insertions, deletions, and lookups, making them suitable for caching, counting, and indexing operations common in web applications.

## Summary

Vectors, strings, and hash maps will provide a large amount of functionality
necessary in programs when you need to store, access, and modify data. Here are
some exercises you should now be equipped to solve:

1. Given a list of integers, use a vector and return the median (when sorted,
   the value in the middle position) and mode (the value that occurs most
   often; a hash map will be helpful here) of the list.
1. Convert strings to pig latin. The first consonant of each word is moved to
   the end of the word and _ay_ is added, so _first_ becomes _irst-fay_. Words
   that start with a vowel have _hay_ added to the end instead (_apple_ becomes
   _apple-hay_). Keep in mind the details about UTF-8 encoding!
1. Using a hash map and vectors, create a text interface to allow a user to add
   employee names to a department in a company; for example, "Add Sally to
   Engineering" or "Add Amir to Sales." Then let the user retrieve a list of all
   people in a department or all people in the company by department, sorted
   alphabetically.

The standard library API documentation describes methods that vectors, strings,
and hash maps have that will be helpful for these exercises!

We're getting into more complex programs in which operations can fail, so it's
a perfect time to discuss error handling. We'll do that next!

[validating-references-with-lifetimes]: ch10-03-lifetime-syntax.html#validating-references-with-lifetimes
[access]: #accessing-values-in-a-hash-map
[traits]: ch10-02-traits.html
