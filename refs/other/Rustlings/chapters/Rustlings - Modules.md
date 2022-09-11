Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Modules
- **Packages**: a [[Cargo]] feature that lets you build, test and share crates
- **Crates**: a tree of modules that produces a *library* or *executable*
- **Modules**: let you control organization
- **Paths**: a way of naming an item such as a struct, function or module

Only modules that are public (`pub`) can be accessed from outside. Without the `use` keyword, you would have to always write the full name of things, which can get repetitve. The `use` keyword brings the module name **ONLY** into the scope where `use` was used.

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

In order to split the modules in different files you need to specify the module name and also create a fiel with that path. An example for the code above would be:

src/lib.rs
```rust
mod front_of_house;

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

src/front_of_house.rs
```rust
pub mod hosting;
```

src/front_of_house/hosting.rs
```rust
pub fn add_to_waitlist() {}
```

## Resources
[[Rustlings]]