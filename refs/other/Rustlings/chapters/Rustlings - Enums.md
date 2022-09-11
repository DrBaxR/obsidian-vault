Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Enums
Rust enums can also encapsulate data, which makes them really powerful

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

The enum above is equivalent to the following sequence if you use structs

```rust
struct QuitMessage; // unit struct
struct MoveMessage {
    x: i32,
    y: i32,
}
struct WriteMessage(String); // tuple struct
struct ChangeColorMessage(i32, i32, i32); // tuple struct
```

It is also possible to define methods on enums, also by using the `impl` keyword

```rust
impl Message {
	fn call(&self) {
		// method body would be defined here
	}
}

let m = Message::Write(String::from("hello"));
m.call();
```

### `Option` enum
Rust does not have `null`, but is has another way of expressing that a value may be missing: the Option enum

```rust
enum Option<T> {
    None,
    Some(T),
}
```

### Pattern syntax
You can use the `match` operator kind of like a switch

```rust
let x = 1;

match x {
	1 => println!("one"),
	2 => println!("two"),
	3 => println!("three"),
	_ => println!("anything"),
}
```

#### Destructuring
Destructuring can be used on structs

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p = Point { x: 0, y: 7 };

    let Point { x, y } = p;
    assert_eq!(0, x);
    assert_eq!(7, y);
}
```

You can also use destructuring (pattern matching) in a `match` statement. This same mechanism can be used on enums

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p = Point { x: 0, y: 7 };

    match p {
        Point { x, y: 0 } => println!("On the x axis at {}", x),
        Point { x: 0, y } => println!("On the y axis at {}", y),
        Point { x, y } => println!("On neither axis: ({}, {})", x, y),
    }
}
```

It's also possible to add extra conditions in match structures with *match guards*

```rust
let num = Some(4);

match num {
	Some(x) if x % 2 == 0 => println!("The number {} is even", x),
	Some(x) => println!("The number {} is odd", x),
	None => (),
}
```

#### `if let`
It is also possible to extract values from enums or other destructurable things by usign the `if let` feature of rust. The two sequences of code below do the same thing

```rust
let coin = Coin::Penny;
let mut count = 0;
match coin {
	Coin::Quarter(state) => println!("State quarter from {:?}!", state),
	_ => count += 1,
}
```

```rust
let coin = Coin::Penny;
let mut count = 0;
if let Coin::Quarter(state) = coin {
	println!("State quarter from {:?}!", state);
} else {
	count += 1;
}
```

## Resources
[[Rustlings]]