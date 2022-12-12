Tags: #gamedev #rust 
Created: 2022-12-12 14:12
References: https://bevy-cheatbook.github.io/programming/res.html

# Bevy Resources
Resources are global data in the application. that is independent of entities.

To create a resource, you can use a [[Rustlings - Structs]] or [[Rustlings - Enums]] and derive the `Resource` trait, similar to components:

```rust
#[derive(Resource)]
struct GoalsReached {
    main_goal: bool,
    bonus: bool,
}
```

Resources can be accessed from [[Bevy Systems]] usign `Res`/`ResMut`.

## Resource initialization
For simple initialization, you can implement the `Default` trait:

```rust
#[derive(Resource, Default, Debug)]
struct StartingLevel(usize);
```

For resources that need complex initialization, implement the `FromWorld` trait:

```rust
#[derive(Resource)]
struct MyFancyResource { /* stuff */ }

impl FromWorld for MyFancyResource {
    fn from_world(world: &mut World) -> Self {
        // You have full access to anything in the ECS from here.
        // For instance, you can mutate other resources:
        let mut x = world.get_resource_mut::<MyOtherResource>().unwrap();
        x.do_mut_stuff();

        MyFancyResource { /* stuff */ }
    }
}
```

Resources can be initialized at app creation:

```rust
fn main() {
    App::new()
        // ...

        // if it implements `Default` or `FromWorld`
        .init_resource::<MyFancyResource>()
        // if not, or if you want to set a specific value
        .insert_resource(StartingLevel(3))

        // ...
        .run();
}
```

[[Bevy Commands]] can also be used to create/remove resource from a system:

```rust
commands.insert_resource(GoalsReached { main_goal: false, bonus: false });
commands.remove_resource::<MyResource>();
```


