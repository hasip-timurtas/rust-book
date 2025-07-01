## Advanced Types

### Newtype Pattern for Type Safety

Compile-time type safety and abstraction:

```rust,editable
struct UserId(u32);
struct ProductId(u32);

fn get_user(id: UserId) -> User { /* ... */ }
// get_user(ProductId(123)); // Compile error
```

**Use cases**: Units (`Kilometers`, `Miles`), IDs, domain modeling, API abstraction.

### Type Aliases

Convenience syntax for complex types:

```rust,editable
type Kilometers = i32;  // No type safety
type Thunk = Box<dyn Fn() + Send + 'static>;

// Result alias pattern (std::io uses this)
type Result<T> = std::result::Result<T, std::io::Error>;

fn write_data() -> Result<()> { /* ... */ }
```

### Never Type (`!`)

Represents computations that never return:

```rust,editable
fn never_returns() -> ! {
    panic!("This function never returns!");
}

// Key property: ! coerces to any type
let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,  // continue has type !, coerces to u32
};
```

**Other `!` expressions**: `panic!`, `loop {}`, `process::exit()`.

### Dynamically Sized Types (DSTs)

Types with unknown compile-time size:

```rust,editable
// str is a DST (not &str)
let s1: &str = "hello";      // OK: reference with size info
let s2: Box<str> = "hello".into();  // OK: smart pointer

// Won't compile - unknown size
// let s3: str = "hello";
```

**Common DSTs**: `str`, `[T]`, `dyn Trait`.

### `Sized` Trait

Generic functions implicitly require `Sized`:

```rust,editable
fn generic<T>(t: T) {}           // Actually: fn generic<T: Sized>(t: T)
fn flexible<T: ?Sized>(t: &T) {} // ?Sized relaxes the bound
```

**`?Sized`** means "maybe sized" - requires using `T` behind a pointer (`&T`, `Box<T>`).

[encapsulation-that-hides-implementation-details]: ch18-01-what-is-oo.html#encapsulation-that-hides-implementation-details
[string-slices]: ch04-03-slices.html#string-slices
[the-match-control-flow-operator]: ch06-02-match.html#the-match-control-flow-operator
[using-trait-objects-that-allow-for-values-of-different-types]: ch18-02-trait-objects.html#using-trait-objects-that-allow-for-values-of-different-types
[using-the-newtype-pattern]: ch20-02-advanced-traits.html#using-the-newtype-pattern-to-implement-external-traits-on-external-types
