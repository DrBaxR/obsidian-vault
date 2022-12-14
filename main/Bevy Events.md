Tags: #gamedev #rust 
Created: 2022-12-14 23:12
References: https://bevy-cheatbook.github.io/programming/events.html

# Bevy Events
Events are the way [[Bevy Systems]] communicate with each other. To send events you can use `EventWriter<T>` and to receive events you can use `EventReader<T>`.

Same events can be handled from multiple systems.

```rust
struct LevelUpEvent(Entity);

fn player_level_up(
    mut ev_levelup: EventWriter<LevelUpEvent>,
    query: Query<(Entity, &PlayerXp)>,
) {
    for (entity, xp) in query.iter() {
        if xp.0 > 1000 {
            ev_levelup.send(LevelUpEvent(entity));
        }
    }
}

fn debug_levelups(
    mut ev_levelup: EventReader<LevelUpEvent>,
) {
    for ev in ev_levelup.iter() {
        eprintln!("Entity {:?} leveled up!", ev.0);
    }
}
```

Besides using the reader and writer of the event, you also need to register your events using the [[Bevy App Builder]].

Events should be the go-to tool for data flow, since they can be sent form any system and received by multiple systems.

## Possilbe pitfalls
If the system that handles the event runs before the system that sends the event, the event will be handled a frame after it was sent. If you need it to be handled in the exact same frame, use explicit system ordering.

Events do not persist, they only exist until the end of the next frame. If systems do not handle events each frame, some may be lost.