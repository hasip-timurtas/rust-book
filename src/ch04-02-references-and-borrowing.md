## References and Borrowing

References allow using values without taking ownership - similar to pointers but guaranteed to be valid.

```rust
fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1); // borrow s1
    println!("The length of '{}' is {}.", s1, len); // s1 still valid
}

fn calculate_length(s: &String) -> usize {
    s.len()
} // s goes out of scope but doesn't drop the value (no ownership)
```

The `&` operator creates a reference. The value being referenced remains owned by the original variable.

### Borrowing Rules

**Immutable references** (default):
```rust
let s = String::from("hello");
let r1 = &s;
let r2 = &s; // multiple immutable references allowed
```

**Mutable references**:
```rust
let mut s = String::from("hello");
let r1 = &mut s;
// let r2 = &mut s; // ERROR: only one mutable reference allowed
```

**Core borrowing rules**:
1. Either one mutable reference OR any number of immutable references
2. References must always be valid (no dangling references)

### Why These Rules?

These rules prevent **data races** at compile time:
- Multiple writers to the same data
- Writer + reader accessing same data simultaneously  
- No synchronization mechanism

This is similar to race conditions you might handle with locks in Node.js, but Rust enforces safety at compile time.

### Reference Scopes

References are valid from introduction until last use:

```rust
let mut s = String::from("hello");

let r1 = &s;        // immutable reference
let r2 = &s;        // immutable reference  
println!("{} {}", r1, r2); // last use of immutable references

let r3 = &mut s;    // mutable reference (OK, immutable refs no longer used)
println!("{}", r3);
```

### Dangling References

Rust prevents dangling references at compile time:

```rust
fn dangle() -> &String {     // ERROR
    let s = String::from("hello");
    &s // s will be dropped, reference would be invalid
} // s goes out of scope

fn no_dangle() -> String {   // OK
    String::from("hello")    // ownership moved to caller
}
```

**Summary**: References provide safe, zero-cost access to data without ownership transfer. The borrowing rules eliminate entire categories of bugs that require runtime checking or sophisticated tooling in other languages.
