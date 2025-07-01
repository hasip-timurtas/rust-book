# Programming a Guessing Game

This chapter demonstrates core Rust concepts through a practical project: a CLI guessing game. Unlike Node.js scripts, this compiles to a self-contained binary with no runtime dependencies.

**Key concepts covered:**
- **Variables and mutability** (`let` vs `let mut`)
- **Pattern matching** (`match` expressions)
- **Error handling** (`Result` type vs exceptions)
- **External dependencies** (Cargo vs npm)
- **Type system** (compile-time safety vs runtime checks)

## Project Setup

```console
$ cargo new guessing_game
$ cd guessing_game
```

Generated structure matches standard Rust conventions:
```toml
# Cargo.toml
[package]
name = "guessing_game"
version = "0.1.0"
edition = "2024"

[dependencies]
```

## Input Handling

Unlike Node.js's `readline` or `process.stdin`, Rust uses explicit imports and error handling:

```rust,editable
use std::io;

fn main() {
    println!("Guess the number!");
    println!("Please input your guess.");

    let mut guess = String::new();  // Mutable variable (explicit)
    
    io::stdin()
        .read_line(&mut guess)      // Borrows mutable reference
        .expect("Failed to read line");  // Error handling (no try/catch)

    println!("You guessed: {guess}");
}
```

**Key differences from TypeScript/Node.js:**
- **Explicit mutability**: `let mut` vs `let` (immutable by default)
- **Borrowing**: `&mut guess` passes a mutable reference, not the value
- **No exceptions**: `Result` type forces explicit error handling
- **No garbage collection**: Memory managed through ownership system

## External Dependencies

Add the `rand` crate to `Cargo.toml`:

```toml
[dependencies]
rand = "0.8.5"
```

**vs npm/package.json:**
- Dependencies compile into the binary (no `node_modules` at runtime)
- Semantic versioning with automatic compatibility resolution
- `Cargo.lock` ensures reproducible builds (like package-lock.json)

```console
$ cargo build  # Downloads, compiles, and links dependencies
```

## Random Number Generation

```rust,editable
use rand::Rng;
use std::io;

fn main() {
    println!("Guess the number!");
    
    let secret_number = rand::thread_rng().gen_range(1..=100);
    // Unlike Math.random(), this is cryptographically secure by default
    
    println!("Please input your guess.");
    
    let mut guess = String::new();
    io::stdin().read_line(&mut guess).expect("Failed to read line");
    
    println!("You guessed: {guess}");
    println!("The secret number was: {secret_number}");
}
```

## Pattern Matching and Comparison

Rust's `match` is more powerful than TypeScript's `switch`:

```rust,editable
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    let secret_number = rand::thread_rng().gen_range(1..=100);
    
    println!("Please input your guess.");
    let mut guess = String::new();
    io::stdin().read_line(&mut guess).expect("Failed to read line");
    
    // Type conversion with error handling
    let guess: u32 = guess.trim().parse().expect("Please type a number!");
    
    println!("You guessed: {guess}");
    
    // Pattern matching (exhaustive)
    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```

**Key concepts:**
- **Type conversion**: `parse()` returns `Result<T, E>`, not `T | undefined`
- **Exhaustive matching**: Compiler ensures all cases are handled
- **No type coercion**: Must explicitly convert `String` to `u32`

## Game Loop with Robust Error Handling

```rust,editable
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");
    let secret_number = rand::thread_rng().gen_range(1..=100);
    
    loop {  // Infinite loop (like while(true) but more idiomatic)
        println!("Please input your guess.");
        
        let mut guess = String::new();
        io::stdin().read_line(&mut guess).expect("Failed to read line");
        
        // Graceful error handling instead of throwing exceptions
        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => {
                println!("Please type a number!");
                continue;  // Skip to next iteration
            }
        };
        
        println!("You guessed: {guess}");
        
        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;  // Exit loop
            }
        }
    }
}
```

## Final Implementation

```rust,editable
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");
    let secret_number = rand::thread_rng().gen_range(1..=100);
    
    loop {
        println!("Please input your guess.");
        
        let mut guess = String::new();
        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");
        
        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };
        
        println!("You guessed: {guess}");
        
        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

## Key Rust Concepts Demonstrated

**Memory Safety:**
- No null pointer dereferences (no `null` or `undefined`)
- Borrowing prevents data races at compile time
- No manual memory management needed

**Type System:**
- Static typing with inference (like TypeScript but stricter)
- `Result<T, E>` for error handling (instead of exceptions)
- Pattern matching ensures exhaustive case handling

**Performance:**
- Zero-cost abstractions (no runtime overhead)
- Compiled binary runs without runtime environment
- No garbage collection pauses

**Error Handling:**
- Explicit error handling with `Result` and `Option` types
- `match` expressions for control flow
- Compile-time guarantees about error handling

This approach differs significantly from TypeScript/Node.js patterns and demonstrates Rust's emphasis on safety, performance, and explicit error handling. 