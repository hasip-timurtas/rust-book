## Storing Lists of Values with Vectors

`Vec<T>` is a growable array type that stores values contiguously in memory. All elements must be of the same type.

### Creating Vectors

```rust,editable
// Empty vector with type annotation
let v: Vec<i32> = Vec::new();

// Using the vec! macro with initial values
let v = vec![1, 2, 3];
```

### Updating Vectors

```rust,editable
let mut v = Vec::new();
v.push(5);
v.push(6);
v.push(7);
```

The `mut` keyword is required for modification. Rust infers the type from the data.

### Reading Elements

Two approaches for accessing elements:

```rust,editable
let v = vec![1, 2, 3, 4, 5];

// Index syntax - panics on invalid index
let third: &i32 = &v[2];

// get method - returns Option<&T>
let third: Option<&i32> = v.get(2);
```

Use indexing when you want the program to crash on invalid access, use `get()` when you want to handle the error gracefully.

### Borrowing Rules

```rust,editable,ignore,does_not_compile
let mut v = vec![1, 2, 3, 4, 5];
let first = &v[0];
v.push(6); // Error: cannot borrow as mutable
println!("The first element is: {first}");
```

Adding elements may require reallocation, invalidating existing references. The borrow checker prevents use-after-free errors.

### Iteration

```rust,editable
let v = vec![100, 32, 57];
for i in &v {
    println!("{i}");
}

// Mutable iteration
let mut v = vec![100, 32, 57];
for i in &mut v {
    *i += 50;
}
```

### Storing Multiple Types with Enums

Vectors can only store one type, but enums allow storing variants of different types:

```rust,editable
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```

For truly dynamic types unknown at compile time, use trait objects (covered in Chapter 18).

### Memory Management

Vectors are automatically freed when they go out of scope, along with their contents. The borrow checker ensures references remain valid.

```rust,editable
{
    let v = vec![1, 2, 3, 4];
    // v is valid here
} // v goes out of scope and is freed
```

[data-types]: ch03-02-data-types.html#data-types
[nomicon]: ../nomicon/vec/vec.html
[vec-api]: ../std/vec/struct.Vec.html
[deref]: ch15-02-deref.html#following-the-pointer-to-the-value-with-the-dereference-operator
