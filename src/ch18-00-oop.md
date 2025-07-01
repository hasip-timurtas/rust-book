# Object-Oriented Programming Features

<!-- Old link, do not remove -->

<a id="object-oriented-programming-features-of-rust"></a>

Rust supports some OOP patterns through traits and structs, but emphasizes composition over inheritance and compile-time safety over runtime polymorphism.

## OOP in Rust vs Traditional OOP

**Encapsulation**: ✅ Structs with private fields and public methods
**Inheritance**: ❌ No class inheritance, use composition and traits
**Polymorphism**: ✅ Trait objects provide dynamic dispatch

## Rust's Approach

**Composition over Inheritance**: Build complex behavior from simpler components
**Trait-based Design**: Shared behavior through traits rather than inheritance hierarchies  
**Zero-cost Abstractions**: Static dispatch by default, dynamic dispatch when needed

## Compared to TypeScript Classes

**TypeScript**: Class-based inheritance
```typescript
class Animal {
    name: string;
    speak(): void { /* implementation */ }
}
class Dog extends Animal {
    speak(): void { console.log("Woof!"); }
}
```

**Rust**: Trait-based composition
```rust
trait Animal {
    fn name(&self) -> &str;
    fn speak(&self);
}

struct Dog { name: String }
impl Animal for Dog {
    fn name(&self) -> &str { &self.name }
    fn speak(&self) { println!("Woof!"); }
}
```

This chapter explores when OOP patterns are useful in Rust and when Rust's unique features provide better alternatives.
