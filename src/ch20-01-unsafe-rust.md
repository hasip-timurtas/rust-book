## Unsafe Rust

Bypass Rust's compile-time safety checks for low-level operations, FFI, and performance-critical code.

### Unsafe Operations

Five operations require `unsafe` blocks:

```rust
unsafe {
    // Dereference raw pointers
    // Call unsafe functions
    // Access/modify mutable statics  
    // Implement unsafe traits
    // Access union fields
}
```

### Raw Pointers

```rust
let mut num = 5;
let r1 = &num as *const i32;
let r2 = &mut num as *mut i32;

unsafe {
    println!("r1: {}", *r1);  // Dereferencing requires unsafe
}
```

**Key differences from references:**
- No borrow checking (multiple mutable pointers allowed)
- Can be null or point to invalid memory
- No automatic cleanup

### Safe Abstractions

Wrap unsafe code in safe APIs:

```rust
fn split_at_mut(values: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
    assert!(mid <= values.len());
    unsafe {
        let ptr = values.as_mut_ptr();
        (
            slice::from_raw_parts_mut(ptr, mid),
            slice::from_raw_parts_mut(ptr.add(mid), values.len() - mid),
        )
    }
}
```

### FFI (Foreign Function Interface)

```rust
extern "C" {
    fn abs(input: i32) -> i32;
}

unsafe {
    println!("Absolute value: {}", abs(-3));
}
```

**Exporting to C:**
```rust
#[no_mangle]
pub extern "C" fn call_from_c() {
    println!("Called from C!");
}
```

### Static Variables

```rust
static mut COUNTER: usize = 0;

unsafe {
    COUNTER += 1;
    println!("COUNTER: {}", COUNTER);
}
```

### Unsafe Traits

```rust
unsafe trait Foo {
    // ...
}

unsafe impl Foo for i32 {
    // ...
}
```

### Unions

```rust
union MyUnion {
    f1: u32,
    f2: f32,
}

let u = MyUnion { f1: 1 };
unsafe {
    match u {
        MyUnion { f1: i } => println!("i32: {}", i),
    }
}
```

### Miri Validation

Detect undefined behavior in unsafe code:

```console
cargo +nightly miri run
cargo +nightly miri test
```

### Best Practices

1. **Minimize scope**: Keep unsafe blocks small and focused
2. **Document invariants**: Use `SAFETY:` comments explaining preconditions  
3. **Encapsulate**: Don't expose unsafe operations in public APIs
4. **Test thoroughly**: Use Miri and comprehensive test coverage
5. **Validate inputs**: Assert preconditions before unsafe operations

Unsafe Rust enables FFI, zero-cost abstractions, and performance optimizations where safety checks are insufficient.

[dangling-references]: ch04-02-references-and-borrowing.html#dangling-references
[ABI]: ../reference/items/external-blocks.html#abi
[differences-between-variables-and-constants]: ch03-01-variables-and-mutability.html#constants
[extensible-concurrency-with-the-sync-and-send-traits]: ch16-04-extensible-concurrency-sync-and-send.html#extensible-concurrency-with-the-sync-and-send-traits
[the-slice-type]: ch04-03-slices.html#the-slice-type
[unions]: ../reference/items/unions.html
[miri]: https://github.com/rust-lang/miri
[editions]: appendix-05-editions.html
[nightly]: appendix-07-nightly-rust.html
[nomicon]: https://doc.rust-lang.org/nomicon/
