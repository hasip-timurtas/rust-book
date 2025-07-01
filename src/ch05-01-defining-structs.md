## Defining and Instantiating Structs

Structs group related data with named fields, providing better semantics than tuples.

### Basic Struct Definition

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

### Instantiation and Access

```rust
let mut user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};

user1.email = String::from("anotheremail@example.com");
```

**Note**: The entire instance must be mutable; individual fields cannot be selectively mutable.

### Field Init Shorthand

When parameter names match field names:

```rust
fn build_user(email: String, username: String) -> User {
    User {
        email,          // equivalent to email: email
        username,       // equivalent to username: username
        active: true,
        sign_in_count: 1,
    }
}
```

### Struct Update Syntax

Create instances from existing ones:

```rust
let user2 = User {
    email: String::from("another@example.com"),
    ..user1     // use remaining values from user1
};
```

**Ownership note**: This moves data from `user1`. After this, `user1` can no longer be used because `username` (a `String`) was moved. If only `Copy` types were used from `user1`, it would remain valid.

### Tuple Structs

Structs without named fields:

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

Each tuple struct is a distinct type, even with identical field types.

### Unit-Like Structs

Structs with no fields, useful for implementing traits:

```rust
struct AlwaysEqual;
let subject = AlwaysEqual;
```

### Ownership in Structs

```rust
// Preferred: owned data
struct User {
    username: String,  // owned
    email: String,     // owned
}

// Requires lifetimes (covered in Chapter 10)
struct User<'a> {
    username: &'a str,  // borrowed
    email: &'a str,     // borrowed
}
```

For most cases, use owned types (`String`) rather than references (`&str`) in struct fields to avoid lifetime complexity.

[tuples]: ch03-02-data-types.html#the-tuple-type
[move]: ch04-01-what-is-ownership.html#variables-and-data-interacting-with-move
[copy]: ch04-01-what-is-ownership.html#stack-only-data-copy
