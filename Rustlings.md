Tags: #reference 
Created: 2022-09-09 22:09

# Rustlings
## Intro
In [[Rust]], you can print stuff to the console with the `println!` command. You can also format the stuff you want to print using this command.

```rust
println!("Hello {}!", "World");
```

## Variables
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

## Functions
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

## `if` statements
You can use an `if` in a `let` statement since it is an expression.

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```

## Data types
You have the classic types like `int`, `float`,... but they also include the amount of bits they have.

### Tuples
```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

It is also possible to destructure tuples using this syntax

```rust
let cat = ("Furry McFurson", 3.5);
let (name, age) = cat;
```

### Arrays
```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```

You can initialize an array with the same element repeated a set amount of times with this syntax

```rust
let a = [3; 5];
```

## Slice Type
A slice lets you reference a contiguous sequence of elements in a collection rather than the whole collection.

### String slices
You can get a section of the string using string slices (which have a type of `&str`).

```rust
fn main() {
	let s = String::from("hello");
	
	let len = s.len();
	
	let slice = &s[3..len];
	let slice = &s[3..];
}
```



## Resources
https://github.com/rust-lang/rustlings
