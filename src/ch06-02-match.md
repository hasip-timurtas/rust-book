<!-- Old heading. Do not remove or links may break. -->

<a id="the-match-control-flow-operator"></a>

## The `match` Control Flow Construct

`match` provides exhaustive pattern matching, similar to switch statements but with compile-time completeness guarantees.

### Basic Match Syntax

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

### Pattern Binding

Extract data from enum variants:

```rust
#[derive(Debug)]
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {state:?}!");
            25
        }
    }
}
```

### Matching `Option<T>`

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}

let five = Some(5);
let six = plus_one(five);   // Some(6)
let none = plus_one(None);  // None
```

### Exhaustive Matching

Rust enforces handling all cases:

```rust
// This won't compile - missing None case
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        Some(i) => Some(i + 1),
    }
}
// ERROR: pattern `None` not covered
```

### Catch-All Patterns

```rust
let dice_roll = 9;
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    other => move_player(other),  // binds value
}

// Or ignore the value:
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(), 
    _ => reroll(),  // don't bind value
}

// Or do nothing:
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    _ => (),  // unit value - no operation
}
```

### Multi-line Arms

```rust
match coin {
    Coin::Penny => {
        println!("Lucky penny!");
        1
    }
    Coin::Nickel => 5,
    // ...
}
```

**Key advantages over switch statements**:
- **Exhaustiveness**: Compiler ensures all cases handled
- **Pattern destructuring**: Extract data from complex types
- **Expression-based**: Returns values
- **No fallthrough**: Each arm is isolated

**Performance**: `match` compiles to efficient jump tables or decision trees - zero runtime overhead.

[tuples]: ch03-02-data-types.html#the-tuple-type
[ch19-00-patterns]: ch19-00-patterns.html
