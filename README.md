# Minecraft Bedrock Edition - 3rd Dimension Space Utility module

This module is made with following modules

> @minecraft/server (stable API - version 1.10.0)

## Space
`Space` is a `class` which can express 3rd dimension grid. It can be used like `Vector3` interface.

See the Example below.

#### Spawning Particle in front of entity with its view direction.
```typescript
const targetLocation: Space = Space.getByValue(1, 0, 0).tiltByView(entity.getViewDirection()).add(entity.location);

entity.dimension.spawnParticle("minecraft:basic_flame_particle", targetLocation);
```
#### Spawning Particle around the entity
```typescript
const basis: Space = Space.getByValue(1, 0, 0);
for (let i = 0; i < 8; ++i) {
    entity.dimension.spawnParticle(
        "minecraft:basic_flame_particle", 
        basis.yTilt(Math.PI / 4).clone().add(entity.location),
    );
}
```

## Features
1. Tilting Basis with given `Vector3` or `Space`
2. Add, Subtract, Multiply, Cross with given other `Vector3` or `Space` or `Number`
3. Tilting Basis based on each axis basis (x, y, z).
4. Converting into `Yaw` and `Pitch` which can be used for entity rotation
5. Calculating length of `Space`
6. Verifying Grid if it's on available and script controlable position.
7. `Y` value increasing
8. Normalizing so that the `Space` has a length 1.

## Warning
When you try to tilt `Space` with given `Vector3` or `Space`,  `Space` (which is method `tiltByView()`) class doesn't follow the basis orientation of normal Minecraft. (like `^ ^ ^`)

### Minecraft Axis Orientation
> +x : right, -x : left
<br>
> +y : up, -y : down
<br>
> +z : behind, -z : front

### Space Module Axis Orientation
> +x : front, -x : behind
<br>
> +y : up, -y : down
<br>
> +z : right, -z : left

### Example of tilting
```typescript
const targetLocation: Space = Space.getByValue(1, 0, 0).tiltByView(entity.getViewDirection()).add(entity.location);

entity.dimension.spawnParticle("minecraft:basic_flame_particle", targetLocation);
```

Since the orignal `Space` has a value of {`x: 1, y: 0, z: 0`}, after tilting `Space` turns into 1 block front side of the entity's view direction.

If the orignal `Space` has a value of {`x: -2, y: 0, z: 0`}, after tilting `Space` turns into 2 block back side of the entity's view direction.


If the orignal `Space` has a value of {`x: 0, y: 0, z: 3`}, after tilting `Space` turns into 3 block right side of the entity's view direction.

And most of method change `the Instance's value`. If you want to get a `new Instance` with a calculation, use `clone()` method and then use other method you want.

### Example of cloning

```typescript
const basis: Space = Spcae.getByValue(1, 0, 0);
const basis2: Space = Spcae.getByValue(1, 0, 0);

const a = basis.up(1); // basis and a value : { x: 1, y: 1, z : 0 }
const b = basis.up(1); // basis and b value : { x: 1, y: 2, z : 0 }
const c = basis.up(1); // basis and c value : { x: 1, y: 3, z : 0 }

/**
 basis2 value : { x: 1, y: 0, z : 0 } -> not changed
 basis3 value : { x: 1, y: 1, z : 0 }
 **/
const basis3 = basis2.clone().up(1);
```