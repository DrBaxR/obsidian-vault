Tags: #gamedev #rust 
Created: 2022-12-12 15:12
References: https://bevy-cheatbook.github.io/programming/queries.html

# Bevy Queries
Queries let you access components of entities:

```rust
fn check_zero_health(
    // access entities that have `Health` and `Transform` components
    // get read-only access to `Health` and mutable access to `Transform`
    // optional component: get access to `Player` if it exists
    mut query: Query<(&Health, &mut Transform, Option<&Player>)>,
) {
    // get all matching entities
    for (health, mut transform, player) in query.iter_mut() {
        eprintln!("Entity at {} has {} HP.", transform.translation, health.hp);

        // center if hp is zero
        if health.hp <= 0.0 {
            transform.translation = Vec3::ZERO;
        }

        if let Some(player) = player {
            // the current entity is the player!
            // do something special!
        }
    }
}
```

They also let you get the components associated with a specific entity:

```rust
    if let Ok((health, mut transform)) = query.get_mut(entity) {
        // do something with the components
    } else {
        // the entity does not have the components from the query
    }
```

In order to get IDs of the entities you access with your queries:

```rust
// add `Entity` to `Query` to get Entity IDs
fn query_entities(q: Query<(Entity, /* ... */)>) {
    for (e, /* ... */) in q.iter() {
        // `e` is the Entity ID of the entity we are accessing
    }
}
```

In case you know that the query **will only ever match a single entity**, you can use `single`/`single_mut`, instead of iterating (this will panic if the query matches more than one entity):

```rust
fn query_player(mut q: Query<(&Player, &mut Transform)>) {
    let (player, mut transform) = q.single_mut();

    // do something with the player and its transform
}
```

## Query filters
You can add these to narrow down the entities you get from a query. These consist of `With`/`Without` to only get entities that have specific components.

```rust
fn debug_player_hp(
    // access the health (and optionally the PlayerName, if present), only for friendly players
    query: Query<(&Health, Option<&PlayerName>), (With<Player>, Without<Enemy>)>,
) {
    // get all matching entities
    for (health, name) in query.iter() {
        if let Some(name) = name {
            eprintln!("Player {} has {} HP.", name.0, health.hp);
        } else {
            eprintln!("Unknown player has {} HP.", health.hp);
        }
    }
}
```

Multiple filters can be combined:
- in a tuple to apply all of them (*AND*)
- using the `Or<(...)>` wrapper to detect any of them (*OR*) - **note the tuple inside**