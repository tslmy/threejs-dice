# A 2024 fork of threejs-dice

[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)](https://github.com/pre-commit/pre-commit)
[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

Create dice for your Three.js scene and throw them on a specific side with Cannon.js.

![Dice types](https://github.com/user-attachments/assets/071b0b3c-affa-4d41-bad9-9c73525b3eac)

| Start rolling | Finished rolling                                                                                     |
| --- |------------------------------------------------------------------------------------------------------|
| ![Start rolling](https://github.com/user-attachments/assets/6dc07b34-cb68-45f1-9239-8dd037e38389) | ![Finished rolling](https://github.com/user-attachments/assets/7fbe4cc7-158c-4f7d-b380-33f21f527845) |

This is a fork from...
* [byWulf/threejs-dice](https://github.com/byWulf/threejs-dice): The original repository.
  * [BreadMoirai/threejs-dice](https://github.com/BreadMoirai/threejs-dice): depends on [cannon-es](https://github.com/pmndrs/cannon-es) instead of the original Cannon.js.
    * [tslmy/threejs-dice](https://github.com/tslmy/threejs-dice) (this repo): Formatted with the [JavaScript Standard Style](https://standardjs.com/). Fixed bugs the prevented shadows from properly rendering. Made material configurable.

## Features
* 4/6/8/10/12/20-sided dice available.
* Appearance of the dice is customizable (size, font color, back color, and material).
* Outcome is controllable. In other words, you can define the side/value that should be upside after the die settles.

## Demos
* [Types and sizes](./examples/types-and-sizes.html) - See the possible dice shapes and options (size / font color / back color)
* [Rolling](./examples/rolling.html) - See, how you can roll 5 dice, which will always land on the same sides

## Install

You can install this package via NPM:

```shell
npm install tslmy/threejs-dice
```

or you can simply use it statically:

```html
<head>
    <script type="importmap">
        {
            "imports": {
                "three": "https://unpkg.com/three@0.169.0/build/three.module.js",
                "cannon-es": "https://cdn.jsdelivr.net/npm/cannon-es@0.20.0/dist/cannon-es.min.js"
                "threejs-dice": "https://cdn.jsdelivr.net/gh/tslmy/threejs-dice@master/lib/dice.js",

            }
        }
    </script>
    <script type="module" src="./static/script.js"></script>
</head>
```

where `script.js` is your script that uses the dice:

```javascript
import { DiceD6, DiceManager } from 'threejs-dice';
```

Read on for usage.

## Usage

```javascript
// Setup your threejs scene
const scene = new THREE.Scene()
// ...

// Setup your cannonjs world
const world = new CANNON.World()
// ...

DiceManager.setWorld(world)

// Create a dice
const dice = new DiceD6({backColor: '#ff0000'}) // DiceD6 for six-sided dice; for options, see DiceObject.
scene.add(dice.getObject())

// If you want to place the mesh somewhere else, you have to update the body
dice.getObject().position.x = 150
dice.getObject().position.y = 100
dice.getObject().rotation.x = 20 * Math.PI / 180
dice.updateBodyFromMesh()

// Set the value of the side, which will be upside after the dice lands
DiceManager.prepareValues([{dice: dice, value: 6}])

//Animate everything
function animate () {
    world.step(1.0 / 60.0)
    // Call this after updating the physics world for rearranging the mesh according to the body:
    dice.updateMeshFromBody()
    renderer.render(scene, camera)
    requestAnimationFrame(animate)
}
requestAnimationFrame(animate)
```

## Credits

The very original repo, `byWulf/threejs-dice`, was based on the [Online 3D dice roller](http://a.teall.info/dice) (link broken) ([blog post](http://www.teall.info/2014/01/online-3d-dice-roller.htm)) by Anton Natarov, who published it under public domain.

![Online 3D dice roller](https://github.com/user-attachments/assets/b2fc204c-6e3e-4fb7-a930-9cbb99625a6f)

> "You can assume that it has the MIT license (or that else) if you wish so. I do not love any licenses at all and prefer to simply say that it is completely free =)"
> -- Anton Natarov
