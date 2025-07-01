## Traits: Paylaşılan Davranış Tanımlama

_Trait_, bir tip ile diğer tipler arasında paylaşılabilecek fonksiyonaliteyi tanımlar. TypeScript interface'lerine benzer, ancak bazı farklılıkları vardır.

### Trait Tanımlama

Bir tipin davranışı, o tip üzerinde çağrılabilen method'lardan oluşur. Farklı tipler aynı method'ları implement ederse aynı davranışı paylaşırlar.

Media aggregator library örneği: `NewsArticle` ve `SocialPost` struct'ları için summary özelliği:

<Listing number="10-12" file-name="src/lib.rs" caption="`summarize` method'u içeren `Summary` trait'i">

```rust,noplayground
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-12/src/lib.rs}}
```

</Listing>

`trait` anahtar kelimesiyle trait tanımları. Method signature'ları semicolon ile bitirilir. Her implement eden tip kendi method implementasyonunu sağlamalıdır.

### Trait Implementation

<Listing number="10-13" file-name="src/lib.rs" caption="`NewsArticle` ve `SocialPost` tiplerinde `Summary` trait implementasyonu">

```rust,noplayground
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-13/src/lib.rs:here}}
```

</Listing>

`impl TraitName for TypeName` syntax'ı kullanılır. Trait method'ları regular method'lar gibi çağrılabilir, ancak trait scope'a import edilmelidir:

```rust,ignore
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-01-calling-trait-method/src/main.rs}}
```

**Orphan Rule**: Trait ya da tip (ya da ikisi) crate'inize local olmalıdır. External trait'i external tip için implement edemezsiniz.

### Default Implementation'lar

Trait method'ları için default davranış tanımlayabilirsiniz:

<Listing number="10-14" file-name="src/lib.rs" caption="Default implementasyon ile `Summary` trait'i">

```rust,noplayground
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-14/src/lib.rs:here}}
```

</Listing>

Default implementasyon kullanmak için boş `impl` block:

```rust,ignore
impl Summary for NewsArticle {}
```

Default implementasyon'lar aynı trait'teki diğer method'ları çağırabilir:

```rust,noplayground
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-03-default-impl-calls-other-methods/src/lib.rs:here}}
```

### Trait Parameters

Function'lar trait implement eden tipleri kabul edebilir:

```rust,ignore
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-04-traits-as-parameters/src/lib.rs:here}}
```

#### Trait Bound Syntax

`impl Trait` syntax sugar'dır. Tam forma trait bound denir:

```rust,ignore
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

İki parametre farklı tipte olabilsin:
```rust,ignore
pub fn notify(item1: &impl Summary, item2: &impl Summary) {
```

Aynı tipte olması zorunluysa:
```rust,ignore
pub fn notify<T: Summary>(item1: &T, item2: &T) {
```

#### Multiple Trait Bounds

`+` syntax'ı ile birden fazla trait gereksinimi:

```rust,ignore
pub fn notify(item: &(impl Summary + Display)) {
pub fn notify<T: Summary + Display>(item: &T) {
```

#### `where` Clauses

Karmaşık trait bound'lar için `where` clause:

```rust,ignore
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-07-where-clause/src/lib.rs:here}}
```

### Trait Return Types

`impl Trait` return type'ı:

```rust,ignore
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/no-listing-05-returning-impl-trait/src/lib.rs:here}}
```

**Kısıtlama**: Sadece tek tip return edilebilir. Farklı tipler return etmek için trait objects gerekir.

### Conditional Method Implementation

Trait bound'lar ile conditional implementation:

<Listing number="10-15" file-name="src/lib.rs" caption="Trait bound'a bağlı conditional method implementasyonu">

```rust,noplayground
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-15/src/lib.rs}}
```

</Listing>

### Blanket Implementations

Başka trait implement eden tipler için otomatik implementasyon:

```rust,ignore
impl<T: Display> ToString for T {
    // --snip--
}
```

Bu sayede `Display` implement eden tüm tipler `ToString` özelliğini kazanır:

```rust
let s = 3.to_string();
```

Trait'ler generic'lerle kod tekrarını azaltırken compile-time type checking sağlar. Runtime overhead olmadan polymorphism sunar.

[using-trait-objects-that-allow-for-values-of-different-types]: ch18-02-trait-objects.html#using-trait-objects-that-allow-for-values-of-different-types
[methods]: ch05-03-method-syntax.html#defining-methods
