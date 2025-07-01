## Ownership Nedir?

Ownership, Rust'ın bellek yönetimi için kullandığı compile-time rule setidir. Garbage collection'lı dillerin runtime overhead'ı ve manual memory management'ın güvenlik risklerinin aksine, Rust statik analizle her iki problemi de çözer.

### Stack vs Heap

TypeScript/Node.js'de gizlenen bu kavramlar Rust'ta kritiktir:

- **Stack**: LIFO, fixed-size değerler, hızlı access
- **Heap**: Dynamic allocation, pointer indirection, runtime allocation overhead

Rust'ta compile-time'da hangi değerlerin stack/heap'te olacağı belirlenir.

### Ownership Kuralları

1. Her değerin tek bir **owner**'ı vardır
2. Aynı anda sadece bir owner olabilir  
3. Owner scope dışına çıktığında değer drop edilir

### Variable Scope

```rust
{
    let s = "hello"; // s scope'a girer
    // s burada valid
} // s scope'tan çıkar, drop edilir
```

### String Tipi

String literal'ler (`"hello"`) stack'te, fixed-size. `String` tipi heap allocation kullanır:

```rust
let s = String::from("hello"); // heap allocation
```

`String` yapısı:
- **ptr**: Heap data'ya pointer
- **len**: Mevcut uzunluk  
- **capacity**: Allocated kapasite

### Move Semantics

```rust
let s1 = String::from("hello");
let s2 = s1; // s1 moved to s2, s1 artık invalid
```

TypeScript'teki reference assignment'ın aksine, Rust `s1`'i invalidate eder. Bu **move** operation'dır, shallow copy değil.

Move olma sebebi: Double free prevention. İki variable aynı heap data'yı point ederse, scope'tan çıkışta ikisi de drop() çağırır.

### Clone vs Copy

**Explicit deep copy**:
```rust
let s1 = String::from("hello");
let s2 = s1.clone(); // Heap data da kopyalanır
```

**Stack-only Copy**:
```rust
let x = 5;
let y = x; // x hala valid, Copy trait
```

`Copy` trait'i olan tipler move olmaz:
- Primitives: `i32`, `bool`, `f64`, `char`
- Tuples (sadece Copy tiplerden oluşuyorsa)

### Function Calls

```rust
fn takes_ownership(s: String) {
    // s burada kullanılır
} // s drop edilir

fn makes_copy(x: i32) {
    // x kopyalanır
}

let s = String::from("hello");
takes_ownership(s); // s moved, artık kullanılamaz

let x = 5;
makes_copy(x); // x hala kullanılabilir
```

### Return Values

```rust
fn gives_ownership() -> String {
    String::from("hello") // Caller'a transfer
}

fn takes_and_gives_back(s: String) -> String {
    s // s caller'a geri döner
}
```

Ownership pattern'ı: assign = move, function parameter = move, return = move.

Bu sistem JavaScript'teki shared references'ın aksine, exactly-once deallocation garantisi sağlar.
