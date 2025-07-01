## Method Syntax

Methods are functions defined within a struct's context. The first parameter is always `self`, representing the instance.

### Defining Methods

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
    
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}

fn main() {
    let rect1 = Rectangle { width: 30, height: 50 };
    let rect2 = Rectangle { width: 10, height: 40 };
    
    println!("Area: {}", rect1.area());
    println!("Can hold: {}", rect1.can_hold(&rect2));
}
```

### `self` Parameter Variants

```rust
impl Rectangle {
    fn area(&self) -> u32 { ... }           // Immutable borrow
    fn set_width(&mut self, width: u32) { } // Mutable borrow  
    fn into_square(self) -> Rectangle { }   // Takes ownership (consumes)
}
```

- `&self`: Read-only access (most common)
- `&mut self`: Mutable access
- `self`: Takes ownership (rare, used for transformations)

### Method vs Field Access

```rust
impl Rectangle {
    fn width(&self) -> bool {
        self.width > 0
    }
}

// Usage
rect1.width()  // Method call (with parentheses)
rect1.width    // Field access (without parentheses)
```

### Automatic Referencing/Dereferencing

Rust automatically handles reference/dereference for method calls:

```rust
p1.distance(&p2);     // Rust automatically uses &p1
(&p1).distance(&p2);  // Equivalent
```

This works because methods have a clear receiver type (`self`).

### Associated Functions

Functions in `impl` blocks without `self` parameter:

```rust
impl Rectangle {
    // Associated function (constructor)
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}

// Called with :: syntax
let sq = Rectangle::square(3);
```

**Usage**: Often used for constructors (commonly named `new`).

### Multiple `impl` Blocks

```rust
impl Rectangle {
    fn area(&self) -> u32 { self.width * self.height }
}

impl Rectangle {
    fn can_hold(&self, other: &Rectangle) -> bool { /* ... */ }
}
```

Both blocks are valid. Multiple blocks are useful with generics and traits (Chapter 10).

**Key differences from functions**:
- Organized with the type
- Method syntax (`instance.method()`)
- Automatic referencing/dereferencing
- Access to struct internals

[enums]: ch06-00-enums.html
[trait-objects]: ch18-02-trait-objects.md
[public]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html#exposing-paths-with-the-pub-keyword
[modules]: ch07-02-defining-modules-to-control-scope-and-privacy.html
