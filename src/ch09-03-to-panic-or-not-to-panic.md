## To `panic!` or Not to `panic!`

**Default choice: Return `Result`** - gives calling code flexibility to decide how to handle errors.

**Use `panic!` when:**
- **Examples/prototypes** - `unwrap()` and `expect()` are acceptable placeholders
- **Tests** - failed assertions should terminate the test
- **Impossible states** - when you have more information than the compiler
- **Contract violations** - invalid inputs that should never happen
- **Security vulnerabilities** - continuing could be dangerous

### Examples and Prototyping

```rust,editable
// Acceptable in examples and prototypes
let config: Config = std::env::args().collect().parse().unwrap();
let file = File::open("config.toml").expect("config file must exist");
```

Use `expect()` over `unwrap()` to provide context for debugging.

### When You Know More Than the Compiler

```rust,editable
use std::net::IpAddr;

let home: IpAddr = "127.0.0.1"
    .parse()
    .expect("Hardcoded IP address should be valid");
```

The string is hardcoded and valid, but the compiler can't guarantee this.

### Guidelines for Library Design

**Panic for programming errors:**
```rust,editable
pub fn get_element(slice: &[i32], index: usize) -> i32 {
    if index >= slice.len() {
        panic!("Index {} out of bounds for slice of length {}", index, slice.len());
    }
    slice[index]
}
```

**Return Result for expected failures:**
```rust,editable
pub fn parse_config(data: &str) -> Result<Config, ConfigError> {
    // Handle malformed data gracefully
}

pub fn fetch_url(url: &str) -> Result<Response, NetworkError> {
    // Network calls can fail in normal operation
}
```

### Type-Based Validation

Create custom types to encode invariants:

```rust,editable
pub struct Guess {
    value: i32,
}

impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess must be between 1 and 100, got {}", value);
        }
        
        Guess { value }
    }
    
    pub fn value(&self) -> i32 {
        self.value
    }
}

// Functions can now safely assume valid ranges
fn process_guess(guess: Guess) {
    // No need to validate - type system guarantees validity
}
```

### Decision Matrix

| Scenario | Choice | Rationale |
|----------|--------|-----------|
| User input validation | `Result` | Expected to be invalid sometimes |
| Network requests | `Result` | Can fail due to external factors |
| File operations | `Result` | Files may not exist or lack permissions |
| Array bounds checking | `panic!` | Programming error, should never happen |
| Parsing hardcoded strings | `expect()` | Invalid data indicates programming error |
| Library contract violations | `panic!` | Caller passed invalid arguments |

### Modern Error Handling Patterns

**Layered error handling:**
```rust,editable
// Low-level: specific errors
fn read_config_file() -> Result<String, io::Error> { /* ... */ }

// Mid-level: domain errors
fn parse_config(content: String) -> Result<Config, ConfigError> { /* ... */ }

// High-level: application errors
fn initialize() -> Result<App, Box<dyn Error>> {
    let content = read_config_file()?;
    let config = parse_config(content)?;
    Ok(App::new(config))
}
```

**Error context chaining:**
```rust,editable
use anyhow::{Context, Result};

fn load_settings() -> Result<Settings> {
    let content = fs::read_to_string("settings.toml")
        .context("Failed to read settings file")?;
    
    toml::from_str(&content)
        .context("Failed to parse settings TOML")
}
```

The goal is to make error handling predictable and maintainable, similar to how TypeScript's strict null checks prevent runtime errors by making nullability explicit.

[encoding]: ch18-03-oo-design-patterns.html#encoding-states-and-behavior-as-types
