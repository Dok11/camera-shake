# CameraShake

CameraShake is a behavior class for BabylonJS that adds camera shake effect with various parameters.

In this example you can see the camera shake effect when the camera is close to the red cube.

https://user-images.githubusercontent.com/2697890/226104576-baeed72e-7f05-4f1c-9c19-60b52358b1c3.mp4

## Installation

To install CameraShake, simply run:

```bash
npm install camera-shake
```

## Usage

Here's an example of how to use CameraShake:

```ts
import { SceneLoader } from '@babylonjs/core';
import { CameraShake, CameraShakeOptions } from 'camera-shake';

SceneLoader.Append('/assets/', 'myScene.gltf', this.scene, scene => {
  // Find a camera
  const myCam = scene.getCameraByName('myCam');

  if (myCam) {
    // Attach behavior to camera
    const cameraShakeOptions: CameraShakeOptions = {
      amplitude: new Vector3(0, 0.1, 0),
      frequency: new Vector3(0, 1, 0),
      rotation: new Vector3(0, 0.01, 0),
      rotationFrequency: new Vector3(0, 0.5, 0),
    };
    const cameraShake = new CameraShake(cameraShakeOptions);
    myCam.addBehavior(cameraShake);
  }
});
```

You can combine CameraShake with PropertyDistanceAttractor to make a camera shake when the camera is close to some shake attractor.

```ts
const cameraShakeOptions: CameraShakeOptions = {
  amplitude: new Vector3(0, 0, 0),
  frequency: new Vector3(0, 0, 0),
  rotation: new Vector3(0, 0, 0),
  rotationFrequency: new Vector3(0, 0, 0),
};
const cameraShake = new CameraShake(cameraShakeOptions);
myCam.addBehavior(cameraShake);

const shakeAttractor = scene.getMeshByName('shakeAttractor');
if (shakeAttractor) {
  const propertyDistanceAttractor = new PropertyDistanceAttractor<CameraShakeOptions>({
    distance: [1, 5],
    target: animatableNode as unknown as TransformNode,
    closest: {
      amplitude: new Vector3(0.05, 0.05, 0.05),
      frequency: new Vector3(0.1, 0.1, 0.1),
      rotation: new Vector3(0, 0.3, 0),
      rotationFrequency: new Vector3(0, 10, 0),
    },
    farthest: {
      amplitude: new Vector3(0, 0, 0),
      frequency: new Vector3(0, 0, 0),
      rotation: new Vector3(0, 0, 0),
      rotationFrequency: new Vector3(0, 0, 0),
    },
    updatableValue: cameraShakeOptions,
  });
  shakeAttractor.addBehavior(propertyDistanceAttractor);
}
```

And you can make a walking camera shake effect with the following code:

```ts
// Walking camera settings, but with zero influence by default
const cameraShakeOptions: CameraShakeOptions = {
  shakePattern: 'sin',
  amplitude: new Vector3(0.1, 0.1, 0),
  frequency: new Vector3(3, 9, 0),
  rotation: new Vector3(0.01, 0, 0),
  rotationFrequency: new Vector3(9, 1, 0),
  influence: 0,
};
const cameraShake = new CameraShake(cameraShakeOptions);
camera.addBehavior(cameraShake);

// Initialize variables to track camera movement
let previousPosition = camera.position.clone();
let previousTime = performance.now();

this.scene.onBeforeRenderObservable.add(function() {
  // Calculate time difference and distance moved since last frame
  const currentTime = performance.now();
  const timeDiff = currentTime - previousTime;
  const distanceMoved = Vector3.Distance(camera.position, previousPosition);

  // Calculate speed in units per second
  const speed = distanceMoved / (timeDiff / 1000);

  // Set cameraShakeOptions.influence relative to camera speed
  const cameraSpeedMax = 4;
  cameraShakeOptions.influence = Math.min(speed / cameraSpeedMax, 1);

  // Update the previous position and time
  previousPosition.copyFrom(camera.position);
  previousTime = currentTime;
});
```

## Options

- `shakePattern` - `'perlin'` or `'sin'`. Default is `'perlin'`.
- `amplitude` - Amplitude of the shake, number or Vector3. Default is `0`.
- `frequency` - Frequency of the shake, number or Vector3. Default is `0`.
- `frequencyOffset` - Frequency offset of the shake, number or Vector3. Default is `Math.random()`.
- `rotation` - Rotation of the shake, number or Vector3. Default is `0`.
- `rotationFrequency` - Rotation frequency of the shake, number or Vector3. Default is `0`.
- `rotationFrequencyOffset` - Rotation frequency offset of the shake, number or Vector3. Default is `Math.random()`.
- `speed` - Speed of the shake. Default is `1`.
- `influence` - Influence of the shake. Default is `1`.
- `onBeforeUpdate` - Callback function that is called before updating the shake. Default is `null`.

## Contributing

Contributions are welcome! If you'd like to contribute to this project, please
follow the standard Gitflow workflow and submit a pull request.

## Relative resources

- [Babylon.js](https://www.babylonjs.com/)
- [Behaviors](https://doc.babylonjs.com/features/featuresDeepDive/behaviors)
- [Interesting examples of BabylonJS usage](https://yuka.babylonpress.org/examples/)
