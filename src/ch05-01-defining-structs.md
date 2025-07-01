## Struct Tanımlama ve Instantiation

### Named Field Structs

```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}

// Instantiation
let user1 = User {
    email: String::from("user@example.com"),
    username: String::from("user123"),
    active: true,
    sign_in_count: 1,
};

// Mutable access
let mut user1 = User { /* ... */ };
user1.email = String::from("new@example.com");
```

### Field Init Shorthand

```rust
fn build_user(email: String, username: String) -> User {
    User {
        email,    // email: email yerine
        username, // username: username yerine
        active: true,
        sign_in_count: 1,
    }
}
```

### Struct Update Syntax

```rust
let user2 = User {
    email: String::from("another@example.com"),
    ..user1 // Diğer alanları user1'den kopyala
};
```

**Dikkat**: `..user1` move semantics kullanır. Non-Copy alanlar move edilir, user1 partial olarak invalid olur.

### Tuple Structs

Type aliasing için kullanışlı:

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);

// black != origin (farklı tipler)
// Destructuring: let Color(r, g, b) = black;
```

### Unit-Like Structs

Marker types için:

```rust
struct AlwaysEqual;
let subject = AlwaysEqual;
```

### Ownership Patterns

**Owned data**: Struct kendi datasını ownar
```rust
struct User {
    username: String, // Owned
    email: String,    // Owned
}
```

**References**: Lifetime annotations gerekir
```rust
struct User<'a> {
    username: &'a str, // Borrowed
    email: &'a str,    // Borrowed
}
```

### TypeScript Comparison

TypeScript:
```typescript
interface User {
    username: string;
    email: string;
    active: boolean;
    signInCount: number;
}
```

Rust struct'ları compile-time memory layout optimization ve zero-cost abstractions sağlar, runtime'da sadece raw memory access'i vardır.
