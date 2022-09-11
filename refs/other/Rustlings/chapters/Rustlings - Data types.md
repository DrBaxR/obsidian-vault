Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Data types
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

## Resources
[[Rustlings]]