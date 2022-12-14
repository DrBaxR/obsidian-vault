Tags: #gamedev #rust 
Created: 2022-12-14 23:12
References: https://bevy-cheatbook.github.io/programming/commands.html https://github.com/bevyengine/bevy/blob/v0.9.1/examples/ecs/ecs_guide.rs

# Bevy Commands
Commands are the thing that you use in order to spawn/despawn entities, add/remove components from existing entities, and manage resources.

**Note:** These actions do not take effect immediately; they are queued to be performed later, when it is safe to do so (more about this on [[Bevy Stages]]).

You can spawn entities with components attached to them (as bundles) by using `.spawn` (`mut commands: Commands` in the system's parameters):

```rust
// create a new entity using `spawn`,
// providing the data for the components it should have
// (typically using a Bundle)
commands.spawn(PlayerBundle {
	name: PlayerName("Henry".into()),
	xp: PlayerXp(1000),
	health: Health {
		hp: 100.0, extra: 20.0
	},
	_p: Player,
	sprite: Default::default(),
});

// you can use a tuple if you need additional components or bundles
// (tuples of component and bundle types are considered bundles)
// (note the extra parentheses)
let my_entity_id = commands.spawn((
	// add some components
	ComponentA,
	ComponentB::default(),
	// add some bundles
	MyBundle::default(),
	TransformBundle::default(),
)).id(); // get the Entity (id) by calling `.id()` at the end
```

You can also add/remove resources using the commands:

```rust
// manage resources
commands.insert_resource(GoalsReached { main_goal: false, bonus: false });
commands.remove_resource::<MyResource>();
```

Finally, it is also possible to insert components to existing entities and also remove components from them:

```rust
// add/remove components of an existing entity
commands.entity(my_entity_id)
	.insert(ComponentC::default())
	.remove::<ComponentA>()
	.remove::<(ComponentB, MyBundle)>();
```

## Examples
This one takes all the players and attaches components to their entities:

```rust
fn make_all_players_hostile(
    mut commands: Commands,
    // we need the Entity id, to perform commands on specific entities
    query: Query<Entity, With<Player>>,
) {
    for entity in query.iter() {
        commands.entity(entity)
            // add an `Enemy` component to the entity
            .insert(Enemy)
            // remove the `Friendly` component
            .remove::<Friendly>();
    }
}
```

And finally, here is an example of despawning entities from the game:

```rust
fn despawn_all_enemies(
    mut commands: Commands,
    query: Query<Entity, With<Enemy>>,
) {
    for entity in query.iter() {
        commands.entity(entity).despawn();
    }
}
```