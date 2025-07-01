## Enum Tanımlama

### Temel Enum Syntax

```rust
enum IpAddrKind {
    V4,
    V6,
}

// Usage
let four = IpAddrKind::V4;
let six = IpAddrKind::V6;

fn route(ip_kind: IpAddrKind) { }
```

### Data ile Enum Variants

```rust
enum IpAddr {
    V4(String),
    V6(String),
}

// Different data types per variant
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);
let loopback = IpAddr::V6(String::from("::1"));
```

### Complex Enum Patterns

```rust
enum Message {
    Quit,                       // Unit variant
    Move { x: i32, y: i32 },   // Struct-like
    Write(String),              // Tuple-like
    ChangeColor(i32, i32, i32), // Multiple values
}
```

TypeScript union types ile karşılaştırma:
```typescript
type Message = 
  | { type: "Quit" }
  | { type: "Move", x: number, y: number }
  | { type: "Write", value: string }
  | { type: "ChangeColor", r: number, g: number, b: number }
```

Rust enum'ları memory efficient: tagged union olarak implement edilir.

### Enum Methods

```rust
impl Message {
    fn call(&self) {
        match self {
            Message::Quit => println!("quit"),
            Message::Move { x, y } => println!("move to {}, {}", x, y),
            Message::Write(text) => println!("write: {}", text),
            Message::ChangeColor(r, g, b) => println!("color: {}, {}, {}", r, g, b),
        }
    }
}

let m = Message::Write(String::from("hello"));
m.call();
```

### Option<T> Enum

Null safety için kritik tür:

```rust
enum Option<T> {
    None,
    Some(T),
}

// Automatic import, direkt kullanılabilir
let some_number = Some(5);
let some_char = Some('e');
let absent_number: Option<i32> = None;
```

### Null Safety Guarantee

```rust
// ❌ Compile error
let x: i8 = 5;
let y: Option<i8> = Some(5);
let sum = x + y; // Error: can't add i8 + Option<i8>

// ✅ Valid: Explicit handling required
let sum = x + y.unwrap_or(0);
```

### TypeScript vs Rust Null Handling

TypeScript (runtime error risk):
```typescript
function getLength(s: string | null): number {
    return s.length; // Runtime error if s is null
}
```

Rust (compile-time safety):
```rust
fn get_length(s: Option<String>) -> usize {
    match s {
        Some(string) => string.len(),
        None => 0,
    }
}
```

Option<T>'yi handle etme patterns'ları `match`, `if let`, `unwrap()`, `map()`, `and_then()` gibi methods ile sağlanır.
