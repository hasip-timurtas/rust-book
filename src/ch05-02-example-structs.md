## An Example Program Using Structs

Demonstrating the progression from loose parameters to structured data.

### Initial Implementation

```rust
fn main() {
    let width1 = 30;
    let height1 = 50;
    
    println!("The area of the rectangle is {} square pixels.", area(width1, height1));
}

fn area(width: u32, height: u32) -> u32 {
    width * height
}
```

**Problem**: Parameters are unrelated; function signature doesn't express the relationship between width and height.

### Refactoring with Tuples

```rust
fn main() {
    let rect1 = (30, 50);
    println!("The area of the rectangle is {} square pixels.", area(rect1));
}

fn area(dimensions: (u32, u32)) -> u32 {
    dimensions.0 * dimensions.1
}
```

**Improvement**: Related data grouped.  
**Problem**: Unclear which element is width/height; order-dependent.

### Refactoring with Structs

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };
    
    println!("The area of the rectangle is {} square pixels.", area(&rect1));
}

fn area(rectangle: &Rectangle) -> u32 {
    rectangle.width * rectangle.height
}
```

**Benefits**:
- Clear field names
- Function signature expresses intent
- Borrowing preserves ownership in `main`

### Debug Output

Structs don't implement `Display` by default. Use `Debug` trait:

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };
    
    println!("rect1 is {:?}", rect1);      // Single-line debug
    println!("rect1 is {:#?}", rect1);     // Pretty-printed debug
    
    // dbg! macro - prints to stderr with file/line info
    let scale = 2;
    let rect2 = Rectangle {
        width: dbg!(30 * scale),  // returns value, prints debug info
        height: 50,
    };
    dbg!(&rect2);
}
```

**Output**:
```
rect1 is Rectangle { width: 30, height: 50 }
rect1 is Rectangle {
    width: 30,
    height: 50,
}
```

**Key insight**: This progression shows how structs improve code organization and meaning. The next step is adding behavior through methods.

[the-tuple-type]: ch03-02-data-types.html#the-tuple-type
[app-c]: appendix-03-derivable-traits.md
[println]: ../std/macro.println.html
[dbg]: ../std/macro.dbg.html
[err]: ch12-06-writing-to-stderr-instead-of-stdout.html
[attributes]: ../reference/attributes.html
