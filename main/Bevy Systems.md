Tags: #gamedev #rust 
Created: 2022-12-12 15:12
References: https://bevy-cheatbook.github.io/programming/systems.html

# Bevy Systems
Systems are basically functions that your write and are run by [[Bevy]]. These functions only take special parameters, to specify what you need access to. Using unsupported parameter types in a system function will lead to confusing compiler messages.:. These functions only take special parameters, to specify what you need access to.

***Using unsupported parameter types in a system function will lead to confusing compiler messages.***

Some of the things that you can do in systems:
- accessing [[Bevy Resources]] using `Res`/`ResMut`
- accessing components of entities using [[Bevy Queries]]
- creating/destroying entities, components and resources using [[Bevy Commands]]
- sending/receiving events using `EventWriter`/`EventReader`

```rust
fn debug_start(
    // access resource
    start: Res<StartingLevel>
) {
    eprintln!("Starting on level {:?}", *start);
}
```

System parameters can be grouped in tuples, which can also be nested. This is useful for organization:

```rust
fn complex_system(
    (a, mut b): (Res<ResourceA>, ResMut<ResourceB>),
    // this resource might not exist, so wrap it in an Option
    mut c: Option<ResMut<ResourceC>>,
) {
    if let Some(mut c) = c {
        // do something
    }
}
```

## Runtime
To run systems you also need to add them, using the [[Bevy App Builder]]:

```rust
fn main() {
    App::new()
        // ...

        // run it only once at launch
        .add_startup_system(init_menu)
        .add_startup_system(debug_start)

        // run it every frame update
        .add_system(move_player)
        .add_system(enemies_ai)

        // ...
        .run();
}
```