# threejs-dice-es

[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)](https://github.com/pre-commit/pre-commit)
[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

Fork of https://github.com/byWulf/threejs-dice that depends on cannon-es instead of cannon

Create dice for your threejs scene and throw them on a specific side with cannonjs.

![Dice types](https://github.com/user-attachments/assets/071b0b3c-affa-4d41-bad9-9c73525b3eac)

![Start rolling](https://github.com/user-attachments/assets/6dc07b34-cb68-45f1-9239-8dd037e38389)

![Finished rolling](https://github.com/user-attachments/assets/7fbe4cc7-158c-4f7d-b380-33f21f527845)

## Features
* 4/6/8/10/12/20 sided dice available
* Customize the appearance of the dice
* You can define the side/value, which should be upside after the die has fallen

## Demos
* [Types and sizes](./examples/types-and-sizes.html) - See the possible dice shapes and options (size / font color / back color)
* [Rolling](./examples/rolling.html) - See, how you can roll 5 dice, which will always land on the same sides

## Install

    npm install threejs-dice

## Usage

    <script src="lib/dice.js"></script>

    <script>
        // Setup your threejs scene
        var scene = new THREE.Scene();
        // ...

        // Setup your cannonjs world
        var world = new CANNON.World();
        // ...

        DiceManager.setWorld(world);

        // Create a dice
        var dice = new DiceD6({backColor: '#ff0000}); //DiceD6 for six-sided dice; for options see DiceObject
        scene.add(dice.getObject());

        // If you want to place the mesh somewhere else, you have to update the body
        dice.getObject().position.x = 150;
        dice.getObject().position.y = 100;
        dice.getObject().rotation.x = 20 * Math.PI / 180;
        dice.updateBodyFromMesh();

        // Set the value of the side, which will be upside after the dice lands
        DiceManager.prepareValues([{dice: dice, value: 6}]);

        //Animate everything
        function animate() {
            world.step(1.0 / 60.0);

            dice.updateMeshFromBody(); // Call this after updating the physics world for rearranging the mesh according to the body

            renderer.render(scene, camera);

            requestAnimationFrame(animate);
        }
        requestAnimationFrame(animate);

    </script>

## Credits
Based on the "Online 3D dice roller" from http://a.teall.info/dice (http://www.teall.info/2014/01/online-3d-dice-roller.htm). Credits go to Anton Natarov, who published it under public domain.

![Online 3D dice roller](https://github.com/user-attachments/assets/b2fc204c-6e3e-4fb7-a930-9cbb99625a6f)

"You can assume that it has the MIT license (or that else) if you wish so. I do not love any licenses at all and prefer to simply say that it is completely free =)" - Anton Natarov
