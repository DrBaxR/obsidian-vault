Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Tests
Tests are rust functions that are annotated with the `#[test]` attribute. In these functions you can make assertions using things like `assert_eq!`.

```rust
#[cfg(test)]
mod tests {
    #[test]
    fn exploration() {
        assert_eq!(2 + 2, 4);
    }
}
```

In case the test should *panic*, then use the `#[should_panic]` attribute. If you don't want your tests to use assertions or to panic, you can make them return a `Result`.

```rust
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() -> Result<(), String> {
        if 2 + 2 == 4 {
            Ok(())
        } else {
            Err(String::from("two plus two does not equal four"))
        }
    }
}
```

## Resources
[[Rustlings]]