## References ve Borrowing

Reference'lar ownership transfer etmeden değerlere erişim sağlar. TypeScript'teki object references'a benzer, ancak compile-time'da memory safety garantisi vardır.

### Immutable References

```rust
fn calculate_length(s: &String) -> usize {
    s.len()
} // s drop edilmez, ownership yok

let s1 = String::from("hello");
let len = calculate_length(&s1); // s1 hala valid
```

`&s1` bir reference oluşturur, ownership'i transfer etmez. Function'ın `&String` parametresi sadece read access sağlar.

### Mutable References

```rust
fn change(s: &mut String) {
    s.push_str(", world");
}

let mut s = String::from("hello");
change(&mut s);
```

### Borrowing Rules (Borrow Checker)

Rust'ın bellek güvenliği için kritik kurallar:

1. **Mutual exclusion**: Bir değer için aynı anda ya tek bir `&mut` reference ya da birden fazla `&` reference olabilir
2. **No dangling**: Reference'lar her zaman valid olmalı

```rust
// ❌ Compile error: Multiple mutable borrows
let mut s = String::from("hello");
let r1 = &mut s;
let r2 = &mut s; // Error!

// ❌ Compile error: Immutable + mutable
let mut s = String::from("hello");
let r1 = &s;
let r2 = &mut s; // Error!

// ✅ Valid: Non-overlapping scopes
let mut s = String::from("hello");
{
    let r1 = &mut s;
} // r1 scope biter
let r2 = &mut s; // OK
```

### Reference Lifetimes

Reference'ların scope'u son kullanımlarına kadar:

```rust
let mut s = String::from("hello");
let r1 = &s;
let r2 = &s;
println!("{} {}", r1, r2); // r1, r2 burada son kullanım

let r3 = &mut s; // OK, r1/r2 artık scope'ta değil
```

### Dangling Prevention

```rust
// ❌ Compile error
fn dangle() -> &String {
    let s = String::from("hello");
    &s // s drop edilir, dangling reference
}

// ✅ Valid: Ownership transfer
fn no_dangle() -> String {
    String::from("hello") // Move ownership
}
```

### Borrow Checker vs Node.js

Node.js'de shared mutable state sorunları:
```javascript
let arr = [1, 2, 3];
let ref1 = arr;
let ref2 = arr;
ref1.push(4); // ref2 de etkilenir, race condition potansiyeli
```

Rust'ta compile-time'da yakalanır:
```rust
let mut arr = vec![1, 2, 3];
let ref1 = &mut arr;
let ref2 = &mut arr; // Compile error
```

Bu sistem concurrent programming'de data races'i önler ve TypeScript'in type safety'sini memory safety'ye extend eder.
