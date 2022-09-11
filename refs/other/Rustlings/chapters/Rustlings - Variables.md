Tags: #reference 
Created: 2022-09-11 14:09

# Rustlings - Variables
By default, they are immutable and declared using the `let` keyword. You can also make them mutable by using `let mut`.

Constants can be declared using `const` and their type MUST be annotated.

### Shadowing
You can shadow the names of variables in an inner scope by declaring a new variable with the same name as a previous one

```rust
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}

```

## Resources
[[Rustlings]]