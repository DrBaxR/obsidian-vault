Tags: #gamedev #rust 
Created: 2022-12-15 14:12
References: https://bevy-cheatbook.github.io/programming/plugins.html

# Bevy Plugins
Plugins are a way to group your stuff ([[Bevy Systems]], [[Bevy Entities and Components]] and such) to make it more modular. Technically speaking, a plugin is an implementation of the `Plugin` trait:

```rust
struct MyPlugin;

impl Plugin for MyPlugin {
    fn build(&self, app: &mut App) {
        app
            .init_resource::<MyOtherResource>()
            .add_event::<MyEvent>()
            .add_startup_system(plugin_init)
            .add_system(my_system);
    }
}

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_plugin(MyPlugin)
        .run();
}
```

## Plugin groups
A plugin group is a convenient way to add multiple plugins at once. It is possible to disable plugins from the group, in case you don't need their functionality.

```rust
struct MyPluginGroup;

impl PluginGroup for MyPluginGroup {
    fn build(self) -> PluginGroupBuilder {
        PluginGroupBuilder::start::<Self>()
            .add(FooPlugin)
            .add(BarPlugin)
    }
}

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_plugins(MyPluginGroup)
        .run();
}
```

Disabling can be done with the `.disable::<T>()` method of the `PluginGroupBuilder`.

## Plugin configuration
It is also possible to add configuration for your plugins/plugin groups. The way you can do that is by adding properties to the struct that implements the `Plugin` trait.

**Note:** For configuration that can be changed at runtime it is recommended to use [[Bevy Resources]].

```rust

struct MyGameplayPlugin {
    /// Should we enable dev hacks?
    enable_dev_hacks: bool,
}

impl Plugin for MyGameplayPlugin {
    fn build(&self, app: &mut App) {
        // add our gameplay systems
        app.add_system(health_system);
        app.add_system(movement_system);
        // ...

        // if "dev mode" is enabled, add some hacks
        if self.enable_dev_hacks {
            app.add_system(player_invincibility);
            app.add_system(free_camera);
        }
    }
}

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_plugin(MyGameplayPlugin {
            enable_dev_hacks: false, // change to true for dev testing builds
        })
        .run();
}
```