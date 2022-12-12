Tags: #gamedev #rust 
Created: 2022-12-12 14:12
References: https://bevy-cheatbook.github.io/programming/ec.html

# Bevy Entities and Components
Entities and Components are part of the [[ECS]] paradigm.

## Entities 
Entities are just simple integer values in [[Bevy]] that are used to identify a particular set of component values.

## Components
Components are the data that is associated with entities.

To create a component type, create a [[Rustlings - Structs]] or [[Rustlings - Enums]] and derive the **Component** trait.

```rust
#[derive(Component)]
struct Health {
    hp: f32,
    extra: f32,
}
```

Components ca be accessed from [[Bevy Systems]] using [[Bevy Queries]]. Components can be added/removed from enisting entities usign [[Bevy Commands]].

## Component Bundles
Bundles are a way to conveniently group a bunch of components:

```rust
#[derive(Bundle)]
struct PlayerBundle {
    xp: PlayerXp,
    name: PlayerName,
    health: Health,
    _p: Player,

    // We can nest/include another bundle.
    // Add the components for a standard Bevy Sprite:
    #[bundle]
    sprite: SpriteSheetBundle,
}
```

[[Bevy]] also considers arbitraty tuples as bundles:

```rust
(ComponentA, ComponentB, ComponentC)
```

**Note:** You cannot query for a whole bundle. Bundles are just a convenience when creating entities.