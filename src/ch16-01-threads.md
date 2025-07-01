## Thread'ler ile Eşzamanlı Kod

Modern işletim sistemleri multiple process'leri manage eder. Program içinde de independent thread'ler eşzamanlı çalışabilir.

Thread kullanımı performance artırır ama complexity ekler:
- **Race conditions**: Inconsistent data access
- **Deadlock**: Thread'lerin birbirini beklemesi
- **Reproducible olmayan bug'lar**

Rust 1:1 model kullanır: Her language thread bir OS thread'e karşılık gelir.

### `spawn` ile Thread Oluşturma

<Listing number="16-1" file-name="src/main.rs" caption="Yeni thread oluşturma">

```rust
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-01/src/main.rs}}
```

</Listing>

Main thread bitince tüm spawned thread'ler terminate edilir.

### `join` Handle'lar

Thread'in tamamlanmasını beklemek için:

<Listing number="16-2" file-name="src/main.rs" caption="`JoinHandle<T>` ile thread completion garantisi">

```rust
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-02/src/main.rs}}
```

</Listing>

`join()` blocking operation'dır - thread terminate olana kadar bekler.

### `move` Closures

Thread'ler arası data transfer için `move` keyword gerekir:

<Listing number="16-3" file-name="src/main.rs" caption="Compile error - borrow checker">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-03/src/main.rs}}
```

</Listing>

Çözüm `move` closure:

<Listing number="16-5" file-name="src/main.rs" caption="`move` ile ownership transfer">

```rust
{{#rustdoc_include ../listings/ch16-fearless-concurrency/listing-16-05/src/main.rs}}
```

</Listing>

`move` keyword closure'un capture ettiği değerlerin ownership'ini alır. Bu borrow checker'ın conservative yaklaşımını override eder.

[capture]: ch13-01-closures.html#capturing-the-environment-with-closures
