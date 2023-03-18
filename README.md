# CameraShake

CameraShake is a behavior class for BabylonJS that adds camera shake effect with
various parameters.

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

## Options

- `shakePattern` - `'perlin'` or `'sin'`. Default is `'perlin'`.
- `amplitude` - Amplitude of the shake, number or Vector3. Default is `0`.
- `frequency` - Frequency of the shake, number or Vector3. Default is `0`.
- `frequencyOffset` - Frequency offset of the shake, number or Vector3. Default is `Math.random()`.
- `rotation` - Rotation of the shake, number or Vector3. Default is `0`.
- `rotationFrequency` - Rotation frequency of the shake, number or Vector3. Default is `0`.
- `rotationFrequencyOffset` - Rotation frequency offset of the shake, number or Vector3. Default is `Math.random()`.
- `speed` - Speed of the shake. Default is `1`.
- `onBeforeUpdate` - Callback function that is called before updating the shake. Default is `null`.

## Contributing

Contributions are welcome! If you'd like to contribute to this project, please
follow the standard Gitflow workflow and submit a pull request.

## Relative resources

- [Babylon.js](https://www.babylonjs.com/)
- [Behaviors](https://doc.babylonjs.com/features/featuresDeepDive/behaviors)
- [Interesting examples of BabylonJS usage](https://yuka.babylonpress.org/examples/)
