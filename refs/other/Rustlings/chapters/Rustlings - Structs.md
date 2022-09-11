Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Structs
This is how you define and instantiate structs

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main() {
    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };
}
```

You can also use `..` lust like the spread operator in [[JavaScript]] and you can also use field shorthand notation if field and value that you assign it to are the same.

### Method syntax
Methods are simitar to functions, the only difference being that they are defined in the context of a struct.

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```

You can also create *associated functions* (they are not methods, because they don't take `&self` as a parameter), functions that you can call like `String::from`.

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}

fn main() {
    let sq = Rectangle::square(3);
}
```

It is also valid syntax to have multiple `impl` blocks.

## Resources
[[Rustlings]]