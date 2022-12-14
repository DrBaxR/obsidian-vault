Tags: #gamedev #rust 
Created: 2022-12-14 23:12
References: https://bevy-cheatbook.github.io/programming/app-builder.html

# Bevy App Builder
To enter the bevy runtime you need to configure an `App`. This is the way you define the structure of all the things that make up your project.

```rust
fn main() {
    App::new()
        // Bevy itself:
        .add_plugins(DefaultPlugins)

        // resources:
        .insert_resource(StartingLevel(3))
        // if it implements `Default` or `FromWorld`
        .init_resource::<MyFancyResource>()

        // events:
        .add_event::<LevelUpEvent>()

        // systems to run once at startup:
        .add_startup_system(spawn_things)

        // systems to run each frame:
        .add_system(player_level_up)
        .add_system(debug_levelups)
        .add_system(debug_stats_change)
        // ...

        // launch the app!
        .run();
}
```