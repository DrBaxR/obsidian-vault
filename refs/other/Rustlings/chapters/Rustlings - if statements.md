Tags: #reference 
Created: 2022-09-11 14:09

# Rustlings - `if` statements
You can use an `if` in a `let` statement since it is an expression.

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```

## Resources
[[Rustlings]]