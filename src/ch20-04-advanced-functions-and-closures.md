## Advanced Functions and Closures

### Function Pointers vs Closures

**Function pointers** (`fn`) are different from closure traits (`Fn`, `FnMut`, `FnOnce`):

```rust
fn add_one(x: i32) -> i32 { x + 1 }

fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
}

let answer = do_twice(add_one, 5);  // Function pointer
```

**Key differences**:
- `fn` is a type, not a trait
- Function pointers implement all closure traits
- Useful for FFI (C functions don't have closures)

### Practical Examples

**Map with function vs closure:**
```rust
let list = vec![1, 2, 3];
let list2: Vec<String> = list.iter().map(ToString::to_string).collect();
```

**Enum constructors as function pointers:**
```rust
enum Status { Value(u32) }
let list: Vec<Status> = (0u32..20).map(Status::Value).collect();
```

### Returning Closures

**Simple case** with `impl Trait`:
```rust
fn returns_closure() -> impl Fn(i32) -> i32 {
    |x| x + 1
}
```

**Multiple closures** require trait objects (each closure has unique type):
```rust
fn returns_closure(condition: bool) -> Box<dyn Fn(i32) -> i32> {
    if condition {
        Box::new(|x| x + 1)
    } else {
        Box::new(|x| x * 2)
    }
}
```

### JavaScript/TypeScript Comparison

**JavaScript**: Functions are first-class objects with uniform representation:
```javascript
function add(x) { return x + 1; }
const multiply = (x) => x * 2;
const funcs = [add, multiply];  // Same type
```

**Rust**: Functions and closures have distinct types based on captures:
```rust
fn add(x: i32) -> i32 { x + 1 }
let multiplier = 2;
let multiply = |x| x * multiplier;  // Different type due to capture

// Need trait objects for heterogeneous storage
let funcs: Vec<Box<dyn Fn(i32) -> i32>> = vec![
    Box::new(add),
    Box::new(multiply),
];
```

Rust's approach enables zero-cost abstractions - closures only pay for what they capture.

[advanced-traits]: ch20-02-advanced-traits.html#advanced-traits
[enum-values]: ch06-01-defining-an-enum.html#enum-values
[closure-types]: ch13-01-closures.html#closure-type-inference-and-annotation
[any-number-of-futures]: ch17-03-more-futures.html
[using-trait-objects-that-allow-for-values-of-different-types]: ch18-02-trait-objects.html#using-trait-objects-that-allow-for-values-of-different-types
