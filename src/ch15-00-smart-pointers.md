# Smart Pointers

Pointer, memory adresini içeren değişkendir. Rust'ta en yaygın pointer'ı `&` reference'tır - sadece veriyi borrow eder, ek capability yoktur.

**Smart pointer'lar** ek metadata ve capability'leri olan veri yapılarıdır. Reference'lar sadece veriyi borrow ederken, smart pointer'lar çoğunlukla veriyi _own_ ederler.

TypeScript/Node.js'te garbage collection otomatik memory management yapar. Rust'ta smart pointer'lar reference counting ve automatic cleanup sağlar.

## Standard Library Smart Pointers

- **`Box<T>`**: Heap'te veri allocation için
- **`Rc<T>`**: Multiple ownership için reference counting  
- **`RefCell<T>`**: Interior mutability pattern'ı - runtime'da borrow rules

Bu chapter ayrıca **interior mutability** pattern'ını kapsar: Immutable tip içinde mutable data'yı değiştirme.

## Deref ve Drop Traits

Smart pointer functionality iki önemli trait ile sağlanır:

- **`Deref`**: Smart pointer'ı regular reference gibi davranmaya zorlar
- **`Drop`**: Smart pointer scope dışına çıktığında cleanup kodu çalıştırır

Bu trait'ler aynı zamanda kendi smart pointer'larınızı yazmak için de kullanılır.
