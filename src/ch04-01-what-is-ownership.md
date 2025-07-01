## Ownership Nedir?

_Ownership_, Rust'ın bellek yönetimi kurallarının temelidir. Node.js/JavaScript'te garbage collector otomatik bellek yönetimi yaparken, Rust compile-time'da kontrol edilen ownership sistemi kullanır. Bu runtime overhead olmadan bellek güvenliği sağlar.

### Stack ve Heap

Stack: LIFO (Last In, First Out) yapısı. Sabit boyutlu veriler için hızlı erişim.
Heap: Dinamik boyutlu veriler için allocation gerekir. Pointer aracılığıyla erişim.

### Ownership Kuralları

1. Her değerin bir _owner_'ı vardır
2. Aynı anda sadece bir owner olabilir  
3. Owner scope dışına çıktığında değer drop edilir

### Variable Scope

```rust
let s = "hello";
```

String literal compile-time'da bilinen sabit boyutlu veridir.

### `String` Tipi

Heap'te allocation gerektiren dinamik string tipi:

```rust
let s = String::from("hello");
```

String literal'lardan farklı olarak mutable ve runtime'da büyüyebilir.

### Bellek ve Allocation

String literal'lar binary'ye hardcode edilir - hızlı ama immutable.
`String` heap allocation gerektirir:

1. `String::from` memory allocator'dan alan ister
2. Scope bitince `drop` function otomatik çağrılır

Rust'ta explicit `free` yoktur - RAII (Resource Acquisition Is Initialization) pattern.

#### Move Semantics

```rust
let s1 = String::from("hello");
let s2 = s1; // s1 moved to s2, s1 artık geçersiz
```

JavaScript'te shallow copy yapılırken, Rust move yapar. Bu double-free hatasını önler.

<Listing number="4-2" caption="Integer değer kopyalama">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-02/src/main.rs:here}}
```

</Listing>

Stack'teki sabit boyutlu değerler kopyalanır.

#### Clone ile Deep Copy

Heap verisini explicitly kopyalamak için:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-05-clone/src/main.rs:here}}
```

`clone()` performans maliyeti olan deep copy yapar.

#### Copy Trait

Stack-only tipler `Copy` trait implement eder:
- Tüm integer tipleri (`u32`, `i32`, vb.)
- `bool`
- Floating-point tipler (`f64`)
- `char`
- Copy implement eden tiplerden oluşan tuple'lar

`Copy` tipleri move olmaz, kopyalanır.

### Function'lar ve Ownership

<Listing number="4-3" file-name="src/main.rs" caption="Function ownership ve scope">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-03/src/main.rs}}
```

</Listing>

Function parameter'ı ownership transfer'i yapar (heap tipleri için).

### Return Values ve Scope

<Listing number="4-4" file-name="src/main.rs" caption="Return value ownership transfer">

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-04/src/main.rs}}
```

</Listing>

Her function call ownership alıp return etmek zahmetli. Bu sorunu references çözer.

[data-types]: ch03-02-data-types.html#data-types
[ch8]: ch08-02-strings.html
[trait-objects]: ch18-02-trait-objects.html#using-trait-objects-that-allow-for-values-of-different-types
[method-syntax]: ch05-03-method-syntax.html#method-syntax
[paths-module-tree]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
[drop]: ../std/ops/trait.Drop.html#tymethod.drop
[traits]: ch10-02-traits.html
[derivable-traits]: appendix-03-derivable-traits.html
