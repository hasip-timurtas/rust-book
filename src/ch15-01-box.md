## `Box<T>` ile Heap'e Veri Yerleştirme

`Box<T>` en basit smart pointer'dır. Stack yerine heap'te veri saklar. Stack'te sadece heap'teki veriyi gösteren pointer kalır.

Performance overhead'i minimal - sadece heap allocation maliyeti. Kullanım durumları:

- Compile-time'da boyutu bilinmeyen tipler için exact size gerektiğinde
- Büyük veriyi ownership transfer etmek ama copy'dan kaçınmak için  
- Trait object'leri için (Chapter 18)

### Temel Kullanım

<Listing number="15-1" file-name="src/main.rs" caption="Heap'te `i32` değer saklama">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-01/src/main.rs}}
```

</Listing>

`Box` scope dışına çıktığında hem box (stack) hem de işaret ettiği veri (heap) deallocate edilir.

### Recursive Types

Recursive type'ların boyutu compile-time'da bilinmez. `Box` ile çözülür.

#### Cons List Örneği

Lisp'ten gelen veri yapısı - nested pair'ler:

```text
(1, (2, (3, Nil)))
```

<Listing number="15-2" file-name="src/main.rs" caption="Infinite size hatası verecek cons list">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-02/src/main.rs:here}}
```

</Listing>

Bu kod infinite size hatası verir çünkü `List` kendi tipini doğrudan içerir.

#### Box ile Çözüm

<Listing number="15-5" file-name="src/main.rs" caption="`Box<T>` ile known size cons list">

```rust
{{#rustdoc_include ../listings/ch15-smart-pointers/listing-15-05/src/main.rs}}
```

</Listing>

`Box<T>` pointer size'ı sabittir, bu yüzden recursive cycle kırılır.

`Box<T>` smart pointer'dır çünkü:
- `Deref` trait implement eder (reference gibi davranır)
- `Drop` trait implement eder (automatic cleanup)

[trait-objects]: ch18-02-trait-objects.html#using-trait-objects-that-allow-for-values-of-different-types
