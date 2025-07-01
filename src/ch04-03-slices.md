## Slice Tipi

Slices, collection'ların contiguous sequence'larına references'dır. Ownership almazlar, view sağlarlar.

### String Slices

```rust
let s = String::from("hello world");
let hello = &s[0..5];   // &str
let world = &s[6..11];  // &str
let slice = &s[..];     // Tüm string
```

String literal'ler zaten slice'dır:
```rust
let s = "Hello, world!"; // s: &str
```

### Function Parameters

```rust
// Bu kullanımı tercih et:
fn first_word(s: &str) -> &str {
    // String slice veya String reference alabilir
}

// Bunun yerine:
fn first_word(s: &String) -> &str {
    // Sadece String reference alabilir
}
```

`&str` daha flexible - hem `String` hem de string literal'lerle çalışır.

### Array Slices

```rust
let a = [1, 2, 3, 4, 5];
let slice = &a[1..3]; // &[i32], [2, 3] içerir
```

### Memory Layout

Slice iki word içerir:
- **ptr**: Data'ya pointer
- **len**: Slice uzunluğu

```rust
let s = String::from("hello");
let slice = &s[0..2]; // ptr: s.ptr, len: 2
```

### Slice Patterns

```rust
match &s[..] {
    "hello" => println!("greeting"),
    slice if slice.len() > 5 => println!("long"),
    _ => println!("other"),
}
```

### Iterator Integration

```rust
let s = "hello world";
let words: Vec<&str> = s.split_whitespace().collect();
```

Slices ownership olmadan memory-efficient view'lar sağlar, JavaScript'teki array.slice()'ın aksine zero-copy operation'dır.

## Özet

Ownership, borrowing ve slices Rust'ın compile-time memory safety garantisinin temelini oluşturur. Bu kavramlar manual memory management'ın karmaşıklığını ve garbage collection'ın runtime overhead'ını ortadan kaldırır.

Bu sistem TypeScript/Node.js'deki memory abstractions'ın üzerine sistem seviyesi kontrol ekler ve concurrent programming'de data races'i önler.
