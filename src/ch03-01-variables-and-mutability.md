## Değişkenler ve Mutability

Varsayılan olarak değişkenler immutable'dır. Bu, Rust'ın güvenli ve eşzamanlı kod yazmanızı destekleyen tasarım kararlarından biridir.

Immutable değişkene atanan değeri değiştiremezsiniz:

<span class="filename">Dosya adı: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/src/main.rs}}
```

`cargo run` çıktısı:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/output.txt}}
```

Bu derleme hatası, kodun bir kısmının değerin hiç değişmeyeceği varsayımıyla çalışırken başka bir kısmının değeri değiştirmeye çalışması durumunda ortaya çıkabilecek bug'ları compile-time'da yakalar.

### Mutable Değişkenler

`mut` anahtar kelimesi ile değişkeni mutable yapabilirsiniz:

<span class="filename">Dosya adı: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/src/main.rs}}
```

Çıktı:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/output.txt}}
```

`mut` kullanımı kodun diğer kısımlarının bu değişkenin değerini değiştireceğini açıkça belirtir.

### Constants

Constants immutable değişkenlerden farklıdır:

1. `mut` kullanılamazlar - her zaman immutable
2. `const` anahtar kelimesi kullanılır
3. Tip annotasyonu zorunludur  
4. Sadece constant expression'larla atanabilirler
5. Herhangi bir scope'ta declare edilebilirler

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

Constants program boyunca declare edildikleri scope'ta geçerlidir. Hardcoded değerleri constant olarak tanımlamak maintainability için önemlidir.

### Shadowing

Aynı isimle yeni değişken tanımlayarak öncekini "shadow" edebilirsiniz:

<span class="filename">Dosya adı: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/src/main.rs}}
```

Çıktı:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/output.txt}}
```

Shadowing ile `mut` arasındaki farklar:

1. Shadowing `let` kullanmadan reassign etmeye çalışırsanız compile error
2. Transformation'lardan sonra değişken immutable kalabilir
3. Aynı ismi kullanarak tip değiştirebilirsiniz:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-04-shadowing-can-change-types/src/main.rs:here}}
```

`mut` ile tip değiştirmeye çalışırsanız hata alırsınız:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/src/main.rs:here}}
```

Error:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/output.txt}}
```

[comparing-the-guess-to-the-secret-number]: ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[data-types]: ch03-02-data-types.html#data-types
[storing-values-with-variables]: ch02-00-guessing-game-tutorial.html#storing-values-with-variables
[const-eval]: ../reference/const_eval.html
