Tags: #gamedev #rust 
Created: 2022-12-16 22:12
References: https://bevy-cheatbook.github.io/programming/system-order.html

# Bevy System Order of Execution
If you need a [[Bevy Systems]] to be run before another system, you can define this type of constraints in the [[Bevy App Builder]]:

```rust
fn main() {
    App::new()
        .add_plugins(DefaultPlugins)

        // order doesn't matter for these systems:
        .add_system(particle_effects)
        .add_system(npc_behaviors)
        .add_system(enemy_movement)

        .add_system(input_handling)

        .add_system(
            player_movement
                // `player_movement` must always run before `enemy_movement`
                .before(enemy_movement)
                // `player_movement` must always run after `input_handling`
                .after(input_handling)
        )
        .run();
}
```

## Labels
It's also possible to use labels to group your systems and then reference those groups to specify the order of execution for systems. Labels can be either primitives or custom types (such as enums that implement a specific trait).

```rust

#[derive(Debug, Clone, Copy, PartialEq, Eq, Hash)]
#[derive(SystemLabel)]
enum MyLabel {
    /// everything that handles input
    Input,
    /// everything that updates player state
    Player,
    /// everything that moves things (works with transforms)
    Movement,
    /// systems that update the world map
    Map,
}

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)

        // use labels, because we want to have multiple affected systems
        .add_system(input_joystick.label(MyLabel::Input))
        .add_system(input_keyboard.label(MyLabel::Input))
        .add_system(input_touch.label(MyLabel::Input))

        .add_system(input_parameters.before(MyLabel::Input))

        .add_system(
            player_movement
                .before(MyLabel::Map)
                .after(MyLabel::Input)
                // we can have multiple labels on this system
                .label(MyLabel::Player)
                .label(MyLabel::Movement)
                // can also use loose strings as labels
                .label("player_movement")
        )

        // … and so on …

        .run();
}
```