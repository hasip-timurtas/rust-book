## Recoverable Errors with `Result`

The `Result<T, E>` enum handles recoverable errors:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

### Basic Usage

```rust
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");
    
    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => panic!("Problem opening file: {error:?}"),
    };
}
```

### Handling Different Error Types

```rust,ignore
use std::fs::File;
use std::io::ErrorKind;

let greeting_file = File::open("hello.txt").unwrap_or_else(|error| {
    if error.kind() == ErrorKind::NotFound {
        File::create("hello.txt").unwrap_or_else(|error| {
            panic!("Problem creating file: {error:?}");
        })
    } else {
        panic!("Problem opening file: {error:?}");
    }
});
```

### Shortcuts: `unwrap` and `expect`

```rust
// unwrap - panic with default message on error
let f = File::open("hello.txt").unwrap();

// expect - panic with custom message
let f = File::open("hello.txt")
    .expect("hello.txt should be included in this project");
```

Use `expect` over `unwrap` in production code for better debugging.

### Error Propagation

Manual propagation:
```rust
fn read_username_from_file() -> Result<String, io::Error> {
    let username_file_result = File::open("hello.txt");
    
    let mut username_file = match username_file_result {
        Ok(file) => file,
        Err(e) => return Err(e),
    };
    
    let mut username = String::new();
    
    match username_file.read_to_string(&mut username) {
        Ok(_) => Ok(username),
        Err(e) => Err(e),
    }
}
```

### The `?` Operator

The `?` operator simplifies error propagation:

```rust
fn read_username_from_file() -> Result<String, io::Error> {
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}

// Even more concise with method chaining
fn read_username_from_file() -> Result<String, io::Error> {
    let mut username = String::new();
    File::open("hello.txt")?.read_to_string(&mut username)?;
    Ok(username)
}

// Using standard library convenience function
fn read_username_from_file() -> Result<String, io::Error> {
    fs::read_to_string("hello.txt")
}
```

The `?` operator:
1. If `Ok(value)` - returns `value`
2. If `Err(e)` - returns early with `Err(e)` converted via `From` trait

### `?` Operator Constraints

`?` can only be used in functions returning compatible types:

```rust,ignore,does_not_compile
fn main() {
    let greeting_file = File::open("hello.txt")?; // Error: main returns ()
}
```

Fix by changing return type:
```rust,ignore
use std::error::Error;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;
    Ok(())
}
```

### Using `?` with `Option`

```rust
fn last_char_of_first_line(text: &str) -> Option<char> {
    text.lines().next()?.chars().last()
}
```

### Common Patterns

**Convert between Result and Option:**
```rust
// Result to Option
let opt: Option<String> = File::open("hello.txt").ok();

// Option to Result
let result: Result<String, &str> = some_option.ok_or("value was None");
```

**Chaining operations:**
```rust
let processed = input
    .parse::<i32>()?
    .checked_mul(2)
    .ok_or("overflow")?;
```

**Early returns in main:**
```rust
fn main() -> Result<(), Box<dyn Error>> {
    let config = load_config()?;
    let data = fetch_data(&config)?;
    process_data(data)?;
    Ok(())
}
```

The `?` operator makes error handling ergonomic while maintaining explicit control flow, similar to how async/await improves Promise handling in TypeScript.

[handle_failure]: ch02-00-guessing-game-tutorial.html#handling-potential-failure-with-result
[trait-objects]: ch18-02-trait-objects.html#using-trait-objects-that-allow-for-values-of-different-types
[termination]: ../std/process/trait.Termination.html
