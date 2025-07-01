## Defining an Enum

Enums represent types with a finite set of possible values, enabling type-safe state modeling.

### Basic Enum Definition

```rust
enum IpAddrKind {
    V4,
    V6,
}

let four = IpAddrKind::V4;
let six = IpAddrKind::V6;

fn route(ip_kind: IpAddrKind) { }
```

### Enums with Data

Unlike structs, enum variants can hold different types and amounts of data:

```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);
let loopback = IpAddr::V6(String::from("::1"));
```

### Complex Enum Variants

```rust
enum Message {
    Quit,                       // unit variant
    Move { x: i32, y: i32 },    // struct-like variant  
    Write(String),              // tuple variant
    ChangeColor(i32, i32, i32), // tuple variant
}
```

This replaces needing separate structs while maintaining type safety and unified handling.

### Methods on Enums

```rust
impl Message {
    fn call(&self) {
        // method body
    }
}

let m = Message::Write(String::from("hello"));
m.call();
```

### The `Option<T>` Enum

Rust eliminates null pointer exceptions by encoding nullability in the type system:

```rust
enum Option<T> {
    None,
    Some(T),
}
```

**Key benefits over null**:
- Explicit nullability in type signatures
- Compile-time null safety
- Must handle `None` case explicitly

### Using `Option<T>`

```rust
let some_number = Some(5);              // Option<i32>
let some_char = Some('e');              // Option<char>
let absent_number: Option<i32> = None;  // Type annotation required
```

### Why `Option<T>` is Superior to Null

This won't compile - prevents null pointer errors:

```rust
let x: i8 = 5;
let y: Option<i8> = Some(5);
let sum = x + y; // ERROR: can't add i8 and Option<i8>
```

**Forcing explicit handling**:
- `Option<T>` and `T` are different types
- Cannot use `Option<T>` as if it contained a value
- Must extract value explicitly (via `match`, `unwrap`, etc.)

### Standard Library Integration

`Option<T>` is in the prelude - available everywhere without imports. Variants `Some` and `None` are also in scope.

**Real-world example** (similar to TypeScript's strict null checks):

```typescript
// TypeScript with strict null checks
function processUser(user: User | null) {
    if (user !== null) {
        // user is User here
        return user.name;
    }
    return "Anonymous";
}
```

```rust
// Rust equivalent
fn process_user(user: Option<User>) -> String {
    match user {
        Some(u) => u.name,
        None => String::from("Anonymous"),
    }
}
```

**Key insight**: `Option<T>` eliminates the billion-dollar mistake of null references by making nullability explicit and enforcing handling at compile time.

[IpAddr]: ../std/net/enum.IpAddr.html
[option]: ../std/option/enum.Option.html
[docs]: ../std/option/enum.Option.html
