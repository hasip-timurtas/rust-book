## Advanced Traits

### Associated Types vs Generics

**Associated types** enforce single implementation per type:

```rust,editable
trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
}

impl Iterator for Counter {
    type Item = u32;  // Only one Item type allowed
    // ...
}
```

**Generics** allow multiple implementations:
```rust,editable
trait Iterator<T> {
    fn next(&mut self) -> Option<T>;
}
// Could implement Iterator<u32>, Iterator<String>, etc.
```

### Default Generic Type Parameters

```rust,editable
trait Add<Rhs = Self> {
    type Output;
    fn add(self, rhs: Rhs) -> Self::Output;
}

// Default: Point + Point
impl Add for Point {
    type Output = Point;
    fn add(self, other: Point) -> Point { /* ... */ }
}

// Custom: Point + Meters
impl Add<Meters> for Point {
    type Output = Point;
    fn add(self, other: Meters) -> Point { /* ... */ }
}
```

**Use cases**: Extending traits without breaking existing code, mixed-type operations.

### Method Disambiguation

**Multiple traits, same method names:**
```rust,editable
trait Pilot {
    fn fly(&self);
}

trait Wizard {
    fn fly(&self);
}

struct Human;
impl Pilot for Human {
    fn fly(&self) { println!("Flying a plane"); }
}
impl Wizard for Human {
    fn fly(&self) { println!("Flying with magic"); }
}

let person = Human;
Pilot::fly(&person);   // Explicit trait
Wizard::fly(&person);  // Explicit trait
```

**Associated functions** (no `self`):
```rust,editable
println!("{}", <Dog as Animal>::baby_name());  // Fully qualified syntax
```

### Supertraits

Require implementing one trait before another:

```rust,editable
trait OutlinePrint: fmt::Display {
    fn outline_print(&self) {
        println!("* {} *", self);  // Can use Display::fmt
    }
}

// Must implement Display before OutlinePrint
impl fmt::Display for Point {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "({}, {})", self.x, self.y)
    }
}
```

### Newtype Pattern

Circumvent orphan rule (implement external traits on external types):

```rust,editable
struct Wrapper(Vec<String>);

impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "[{}]", self.0.join(", "))
    }
}

let w = Wrapper(vec![String::from("hello"), String::from("world")]);
println!("w = {}", w);
```

**Additional benefits:**
- Type safety: `UserId(u32)` vs `ProductId(u32)`
- Zero runtime cost
- Controlled API surface

**Access wrapped value** with `Deref`:
```rust,editable
impl Deref for Wrapper {
    type Target = Vec<String>;
    fn deref(&self) -> &Self::Target {
        &self.0
    }
}
```
