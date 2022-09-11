Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Error handling
For recoverable errors, the `Result` enum exists. This result can be used with `match` or you can call the `unwrap_or_else` method or other unwrap methods on it.

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

The `?` operator returns with the received error (if there is an error) *or* it unwraps the value from the result if it was `Ok`. This operator can be used in functions that return either `Result` or `Option`.

```rust
use std::fs::File;
use std::io;
use std::io::Read;

fn read_username_from_file() -> Result<String, io::Error> {
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}
```


## Resources
[[Rustlings]]