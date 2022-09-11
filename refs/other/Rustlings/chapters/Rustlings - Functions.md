Tags: #reference 
Created: 2022-09-11 14:09

# Rustlings - Functions
Functions are defined with `fn`.

```rust
fn main() {
    print_labeled_measurement(5, 'h');
}

fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
```

### Statements and expressions
Statement are instructions that do some action and expressions evaluate to some resulting value.

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1 // notice there is no semicolon
    }; // expression

    println!("The value of y is: {y}"); // statement
}
```

If you add a `;` at the end of a expression, you transform it into a statement.

### Return values
The last expression in a function is the return value or you can use `return`.

```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {x}");
}

```

## Resources
[[Rustlings]]