# Prismarine Physics 26.2

Independent Complexity-ML distribution of the Prismarine player physics engine
with Minecraft Java 26.2 feature detection for GAME LAB.

| Package | Minecraft Java | Release |
| --- | --- | --- |
| `prismarine-physics@1.11.0+complexity.26.2.0` | 26.2 | [`complexity-26.2.0`](https://github.com/Complexity-ML/prismarine-physics-26.2/releases/tag/complexity-26.2.0) |

The 26.2 compatibility layer enables proportional liquid gravity and
jump-assisted climbing for the current game version while retaining the
historical physics behavior inherited from upstream.

## Installation

```bash
npm install "https://github.com/Complexity-ML/prismarine-physics-26.2/releases/download/complexity-26.2.0/prismarine-physics-complexity-26.2.0.tgz"
```

## Usage

```js
const { Physics, PlayerState } = require('prismarine-physics')
const { Vec3 } = require('vec3')

const mcData = require('minecraft-data')('26.2')
const Block = require('prismarine-block')('26.2')

const physics = Physics(mcData, world)
const controls = {
  forward: false,
  back: false,
  left: false,
  right: false,
  jump: false,
  sprint: false,
  sneak: false
}
const player = {
    entity: {
      position: pos,
      velocity: new Vec3(0, 0, 0),
      onGround: false,
      isInWater: false,
      isInLava: false,
      isInWeb: false,
      isCollidedHorizontally: false,
      isCollidedVertically: false,
      elytraFlying: false,
      yaw: 0,
      pitch: 0
    },
    jumpTicks: 0,
    jumpQueued: false,
    fireworkRocketDuration: 0
  }
const playerState = new PlayerState(player, controls)

while (!player.entity.onGround) {
  // simulate 1 tick of player physic, then apply the result to the player
  physics.simulatePlayer(playerState, world).apply(player)
}
```

See `examples/` for more.


## API

### Physics

#### simulatePlayer(playerState, world)
- playerState : instance of the PlayerState class
- world : interface with a function `getBlock(position)` returning the prismarine-block at the given position

### PlayerState

A player state is an object containing the properties:

Read / Write properties:
- pos : position (vec3) of the player entity
- vel : velocity (vec3) of the player entity
- onGround : (boolean) is the player touching ground ?
- isInWater : (boolean) is the player in water ?
- isInLava : (boolean) is the player in lava ?
- isInWeb : (boolean) is the player in a web ?
- isCollidedHorizontally : (boolean) is the player collided horizontally with a solid block ?
- isCollidedVertically : (boolean) is the player collided vertically with a solid block ?
- elytraFlying : (boolean) is the player elytra flying ?
- jumpTicks : (integer) number of ticks before the player can auto-jump again
- jumpQueued : (boolean) true if the jump control state was true between the last tick and the current one
- fireworkRocketDuration : (number) how many ticks of firework boost are remaining ?

Read only properties:
- yaw : (float) the yaw angle, in radians, of the player entity
- pitch: (float) the pitch angle, in radians, of the player entity
- control : (object) control states vector with properties:
  - forward
  - back
  - left
  - right
  - jump
  - sprint
  - sneak

## Validation

The complete physics suite passes 19 tests. The released package is also part
of the live GAME LAB test against the official Minecraft Java 26.2 server.

## Maintenance

Open Minecraft 26.2 and GAME LAB issues or pull requests in this repository.

## Credits and license

Derived from
[PrismarineJS prismarine-physics](https://github.com/PrismarineJS/prismarine-physics).
The upstream history and contributor attribution are retained. Complexity-ML
maintains this 26.2 distribution independently and is not affiliated with
Mojang or Microsoft.

Licensed under the [MIT License](LICENSE).
