# Three Js Envirnonment
This project creates a 3D environment using threee. It involves creating 3d objects and also including textures. A Monkey head is also imported from blender and used in the project.

## Scene
![scene](./assets/three%20js.png)

## Preparing the environment
```js
//create the renderer to render objects
const renderer = new THREE.WebGLRenderer()
//Enable shadows from objects in the scene
renderer.shadowMap.enabled = true
//set  renderer size to the size of the window
renderer.setSize(window.innerWidth, window.innerHeight)

document.body.appendChild(renderer.domElement)
//add scene where objects will be rendered
const scene = new THREE.Scene()
//add the camera 
const camera = new THREE.PerspectiveCamera(
    45,
    window.innerWidth/window.innerHeight,
    0.1,
    1000
)
// Enable movement through the scene using the mouse
const orbit = new OrbitControls(camera, renderer.domElement)
```

## Create objects
The process of object creation involves adding the geometry for the object and adding the material that covers the geometry. 
```js
//Here you create a box
//Add a box geomery
const boxGeometry = new THREE.BoxGeometry()
//Add the material for the box
const boxMaterial = new THREE.MeshBasicMaterial({color:0x00FF00})
//create the box using the mesg
const box = new THREE.Mesh(boxGeometry, boxMaterial)
//add the box to the mesh
scene.add(box)
```

## Lighting
This process involves adding light to the scene, you can have direct light, spot light and ambient light
```js
//add ambient light. This light affect the whole scene
const ambientLight = new THREE.AmbientLight(0x333333)
scene.add(ambientLight)

//add directional light, affects a certain point in the secne
const directionalLight = new THREE.DirectionalLight(0xFFFFFF, 0.8)
scene.add(directionalLight)
directionalLight.position.set(-30, 50, 0)
//Add capability on this light to cast shadows on an object
directionalLight.castShadow = true

// add a spot light on a small area in the scene
const spotLight = new THREE.SpotLight(0xFFFFFF)
scene.add(spotLight)
spotLight.position.set(-100, 100, 0)
spotLight.castShadow = true
spotLight.angle = 0.2

```

## Add Texture to objects
```js
// add texture loader
const textureLoader = new THREE.TextureLoader()
const box2Geometry = new THREE.BoxGeometry(4, 4, 4)
const box2Material = new THREE.MeshBasicMaterial({
    map: textureLoader.load(stars) // add stars texture to all sides of the box
})

//Add different texture to all sides of an object.
const box2MultiMaterial = [
    new THREE.MeshBasicMaterial({map: textureLoader.load(stars)}),
    new THREE.MeshBasicMaterial({map: textureLoader.load(nebula)}),
    new THREE.MeshBasicMaterial({map: textureLoader.load(stars)}),
    new THREE.MeshBasicMaterial({map: textureLoader.load(nebula)}),
    new THREE.MeshBasicMaterial({map: textureLoader.load(stars)}),
    new THREE.MeshBasicMaterial({map: textureLoader.load(nebula)})
]

```

## Use vertex and fragment shaders 
To add custom shaders to an onject, use vertex and fragment shaders. The vertex shader add the points of an object in the 3D space, while the fragment shader adds the color to the points in the object.
```js
//vertex shader
const vShader = `
    void main(){
        gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
    }
`
//frgment shader
const fShader = `
    void main(){
        gl_FragColor = vec4(0.5, 0.5, 1.0, 1.0);
    }
`
const sphere2Geometry = new THREE.SphereGeometry(4)
const sphere2Material = new THREE.ShaderMaterial({
    vertexShader: vShader,
    fragmentShader: fShader
})
```

## Add Blender Model
```js
import {GLTFLoader} from 'three/examples/jsm/loaders/GLTFLoader'
//get the url to the blender model
const blendUrl = new URL('./assets/monkey.glb', import.meta.url)

//create a new GLTF model
const assetLoader = new GLTFLoader()
assetLoader.load(blendUrl.href, function(gltf){
    const model = gltf.scene
    scene.add(model)
    model.position.set(-12, 4, 10)
}, undefined, function(error){
    console.error(error);
})
```
