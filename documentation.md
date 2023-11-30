# **Documentation Robbie Heaney B00418421**
There are many different elements that make up the 5th and final element which i will run through in this documentation.
Element 5 consists of all other elements combined from grounds to objects with gravity and different colours.

## Terrain
The First one being the Terrain.
Here we have the Terrain code used within element 5 which gets the terrain from a third party source in this case Babylon Enviroments and brings it in so that it can be used as the main ground for the Element Shown below: 

```javascript
largeGroundMat.diffuseTexture = new Texture("https://assets.babylonjs.com/environments/valleygrass.png");
```
After the texture is in the Height map must also be brought in to complete the Terrain for Element 5 this is done through the use of babylon resources again and like the texture done like so:

```javascript
const largeGround = 
MeshBuilder.CreateGroundFromHeightMap("largeGround", "https://assets.babylonjs.com/environmentsvillageheightmap.png", {width:150, height:150, subdivisions: 20, minHeight:0, maxHeight: 10});
```

## Importing Player Mesh
To get the Player within Element 5 this was also imported with the use of Babylon resources and done like so:

As you can see this is being loaded from another Scene from a models folder and being imported into the Element 5 

```javascript 
let item : any = SceneLoader.ImportMesh("", "./models/", "dummy3.babylon", scene, 
 function(newMeshes, particleSystems, skeletons) {
  let mesh = newMeshes[0];
```

Then all overrides are set for the Animations where we enable blending set the blender speed to 0.05 and the loop mode to 1

```javascript 
skeleton.animationPropertiesOverride = new AnimationPropertiesOverride();
  skeleton.animationPropertiesOverride.enableBlending = true; 
  skeleton.animationPropertiesOverride.blendingSpeed = 0.05; 
  skeleton.animationPropertiesOverride.loopMode = 1;
  ```
  After which we set which animation should be used for our player it is a walk animation shown below:
  ```javascript
  let walkRange: any = skeleton.getAnimationRange("YBot_Walk");
  ```

  Next is the keys that the user presses to make the player walk we code this so that when W is pressed the player walks forwards this is due to changing the z and y positions to allow for this to take place: 
  ```javascript
  let keydown: boolean = false;
 if (keyDownMap["w"] || keyDownMap["ArrowUp"]) 
{ 
mesh.position.z += 0.1; 
mesh.rotation.y = 0;
keydown=true; 
} 
  ```
Then when A is pressed this allows the player to move left this is due to changing the z and y positions to allow for this to take place:

```javascript
if (keyDownMap["a"] || keyDownMap["ArrowLeft"]) 
{
 mesh.position.x -= 0.1; 
 mesh.rotation.y = 3 * Math.PI / 2; 
 keydown=true;
} 
```

When S is pressed the player can move downwards this is due to changing the z and y positions to allow for this to take place:

```javascript
if (keyDownMap["s"] || keyDownMap["ArrowDown"]) 
{
 mesh.position.z -= 0.1; 
 mesh.rotation.y = 2 * Math.PI / 2; 
 keydown=true;
} 
```
Finally when D is pressed the player can move right this is due to changing the z and y positions to allow for this to take place:

```javascript
if (keyDownMap["d"] || keyDownMap["ArrowRight"]) 
{
 mesh.position.x += 0.1; 
 mesh.rotation.y = Math.PI / 2; 
 keydown=true;
} 
```
Next, if the player is not animating already then allow this to be activated by setting the animating to true which then begins the animation which has the walk range included.

```javascript
if (keydown) 
{
  if (!animating) 
  {
  animating = true; 
  scene.beginAnimation(skeleton, walkRange.from, walkRange.to, true);
  } 
} 
 else 
 { 
  animating = false; 
  scene.stopAnimation(skeleton);
 } 
 }); 
```
## Creating Skybox

This sky box takes the textures from folder textures where the images are located as well as setting the diffuse colour and specular colour aswell as setting the size of 150 to have in the scene.

```javascript
function createSkyBox(scene:Scene)
{
      const skybox = MeshBuilder.CreateBox("skyBox", {size:150}, scene);
      const skyboxMaterial = new StandardMaterial("skyBox", scene);
      skyboxMaterial.backFaceCulling = false;
      skyboxMaterial.reflectionTexture = new CubeTexture("textures/skybox", scene);
      skyboxMaterial.reflectionTexture.coordinatesMode = Texture.SKYBOX_MODE;
      skyboxMaterial.diffuseColor = new Color3(0, 0, 0);
      skyboxMaterial.specularColor = new Color3(0, 0, 0);
      skybox.material = skyboxMaterial;
      return skybox;
}
```

## Creating Trees
Again the textures are took from the textures folder to allow these to be imported into the scene which sets the width and height 

```javascript
function createTrees(scene: Scene)
{
  const spriteManagerTrees = new SpriteManager("treesManager", "textures/palmtree.png", 2000, {width: 512, height: 1024}, scene);
}
```
Aswell as creating trees that will spawn in at random positions within the scene this is done with the Math.random position of the x,y and z coordinates all of which needs to get returned at the bottom to allow them to be displayed
 ```javacript 
  for (let i = 0; i < 500; i++) 
  {
      const tree = new Sprite("tree", spriteManagerTrees);
      tree.position.x = Math.random() * (-30);
      tree.position.z = Math.random() * 20 + 8;
      tree.position.y = 0.5;
  }
  return spriteManagerTrees;
  ```

  ## Creating House Box
  The textures where implemented using the babylon enviroments section online to bring in the diffedrent textures for the houses this was done by importing them in like so however only if the width of the box was equal to 2: 
  ```javascript
  const boxMat = new StandardMaterial("boxMat");
    if (width == 2) 
    {
      boxMat.diffuseTexture = new Texture("https://assets.babylonjs.com/environments/semihouse.png") 
    }
    else 
    {
       boxMat.diffuseTexture = new Texture("https://assets.babylonjs.com/environments/cubehouse.png");   
    }
  ```
  Then an option parameter was used to set different images on each sides of the box
  ```javascript
  const faceUV: Vector4[] = [];
    if (width == 2) {
      faceUV[0] = new Vector4(0.6, 0.0, 1.0, 1.0); //rear face
      faceUV[1] = new Vector4(0.0, 0.0, 0.4, 1.0); //front face
      faceUV[2] = new Vector4(0.4, 0, 0.6, 1.0); //right side
      faceUV[3] = new Vector4(0.4, 0, 0.6, 1.0); //left side
    }
    else {
      faceUV[0] = new Vector4(0.5, 0.0, 0.75, 1.0); //rear face
      faceUV[1] = new Vector4(0.0, 0.0, 0.25, 1.0); //front face
      faceUV[2] = new Vector4(0.25, 0, 0.5, 1.0); //right side
      faceUV[3] = new Vector4(0.75, 0, 1.0, 1.0); //left side
    }
  ```
  However i only need to set four faces as the top and bottom cannot be seen from the players view within the scene 

  ## Create Roofs
  Firstly the Texture image is imported from the babylon JS enviroments section which allows it to be used later on.

  ```javascript
    const roofMat = new StandardMaterial("roofMat");
    roofMat.diffuseTexture = new Texture("https://assets.babylonjs.com/environments/roof.jpg");
  ```

  Then all the properties of the roof are set to the correct height width and the nessesary roation applied to allow the shape of the roof to take place and the texture to be applied correctly.

  ```javascript
  const roof = MeshBuilder.CreateCylinder("roof", {diameter: 1.3, height: 1.2, tessellation: 3});
    roof.material = roofMat;
    roof.scaling.x = 0.75;
    roof.scaling.y = width;
    roof.rotation.z = Math.PI / 2;
    roof.position.y = 1.22;
    return roof;
  ```

  ## Creating the House By Merging
 
 Then we take the box that we have already created along with the Roof thats already been created and we merge them together using the MergeMeshes it has to go down in this section as undefinded as TypeScript does not accept the null value
  ```javascript
  function createHouse(scene: Scene, width: number) {
    const box = createBox(scene, width);
    const roof = createRoof(scene, width);
    const house: any = Mesh.MergeMeshes([box, roof], true, false, undefined, false, true);
    
    return house;
  ```

  ## Cloning The Houses
  First we define which house is which in terms of their position and the rotation these can be used later on in the code to define which house is needed where.
  
  ```javascript
    const detached_house = createHouse(scene, 1); //.clone("clonedHouse");
    detached_house.rotation.y = -Math.PI / 16;
    detached_house.position.x = -6.8;
    detached_house.position.z = 2.5;

    const semi_house = createHouse(scene, 2); //.clone("clonedHouse");
    semi_house.rotation.y = -Math.PI / 16;
    semi_house.position.x = -4.5;
    semi_house.position.z = 3;
  ```
  As you can see here each entry is an Array which allows you to chose the house type then the roation aswell as the x and z coordiantes for the position of the house.

  ```javascript
   
    const places: number[] [] = []; 
    places.push([1, -Math.PI / 16, -6.8, 2.5 ]);
    places.push([2, -Math.PI / 16, -4.5, 3 ]);
    places.push([2, -Math.PI / 16, -1.5, 4 ]);
    places.push([2, -Math.PI / 3, 1.5, 6 ]);
    places.push([2, 15 * Math.PI / 16, -6.4, -1.5 ]);
    places.push([1, 15 * Math.PI / 16, -4.1, -1 ]);
    places.push([2, 15 * Math.PI / 16, -2.1, -0.5 ]);
    places.push([1, 5 * Math.PI / 4, 0, -1 ]);
    places.push([1, Math.PI + Math.PI / 2.5, 0.5, -3 ]);
    places.push([2, Math.PI + Math.PI / 2.1, 0.75, -5 ]);
    places.push([1, Math.PI + Math.PI / 2.25, 0.75, -7 ]);
    places.push([2, Math.PI / 1.9, 4.75, -1 ]);
    places.push([1, Math.PI / 1.95, 4.5, -3 ]);
    places.push([2, Math.PI / 1.9, 4.75, -5 ]);
    places.push([1, Math.PI / 1.9, 4.75, -7 ]);
    places.push([2, -Math.PI / 3, 5.25, 2 ]);
    places.push([1, -Math.PI / 3, 6, 4 ]);
  ```
  Then a for loop is used to allow instances of the house to be created for both the semi house and detached house which then allows the postions to be mapped out. 

  ```javascript
  const houses: Mesh[] = [];
    for (let i = 0; i < places.length; i++) 
    {
      if (places[i][0] === 1) 
      {
          houses[i] = detached_house.createInstance("house" + i);
      }
      else 
      {
          houses[i] = semi_house.createInstance("house" + i);
      }
        houses[i].rotation.y = places[i][1];
        houses[i].position.x = places[i][2];
        houses[i].position.z = places[i][3];
    }
  ```
  ## Create a box with physics
 Next I have created a box that can be moved by the player this is because it has Physics attached to the box which allows it to me moved when touched this is done using PhysicsAggregate
 
  ```javascript
    function createBoxPhysics(scene, x, y, z, rotation: boolean){
    let box: Mesh = MeshBuilder.CreateBox("box", scene);
    box.position.x = x;
    box.position.y = y;
    box.position.z = z;
    
    if (rotation) {
      scene.registerAfterRender(function () {
        box.rotate(new Vector3(4, 8, 2), 0.02, Space.LOCAL);
      });

    const boxAggregate = new PhysicsAggregate(box, PhysicsShapeType.BOX, { mass: 1 }, scene);
    return box;
  }
}
  ```

  ## Creating Polyhedra with a colour of Yellow that rorates mid air
 This assigns the material/colour of yellow with the values 1,1,0 being Yellow
 
  ```Javascript
  const polyhedra = MeshBuilder.CreatePolyhedron("shape", {type: t, size: s}, scene);

  var yellowMat = new StandardMaterial("yellowMat");
  yellowMat.diffuseColor = new Color3(1, 1, 0);
  polyhedra.material = yellowMat;
  ```

  Next is the position of the polyhedra aswell as making it rotate round an axis with a specific angle of 0.02

  ```javascript
    polyhedra.position = new Vector3(px, py, pz);
    scene.registerAfterRender(function () {
    polyhedra.rotate(new Vector3(4, 8, 2)/*axis*/, 0.02/*angle*/, Space.LOCAL);
  ```

  ## Creating Arc Rotate Camera
This allows the camera to be rotated around the scene as it as a control attached to it with the specific maths needed to move
  ```javascript
    const camera = new ArcRotateCamera("Camera", 3 * Math.PI / 4, Math.PI / 4, 4, Vector3.Zero(), scene);
    camera.attachControl(true);
  ```

  ## Creating Hemispheric Light
This creates an Hemispheric Light with a position and an intensity of 0.8 then returns the light so it can be used

  ```javascript
  const lightPoint = new HemisphericLight("HemisphericLight", new Vector3(2, 2, 0.5), scene);
      lightPoint.intensity = 0.8;
      return lightPoint;
  ```

  # **Menu Elements**
  This allows text to be created within the scene with the options to chose the font size and colour
  ```javascript
  function createText(scene: Scene, theText: string, x: string, y: string, s: string, c: string, advtex) {
    let text = new GUI.TextBlock();
    text.text = theText;
    text.color = c;
    text.fontSize = s;
    text.fontWeight = "bold";
    text.left = x;
    text.top = y;
    advtex.addControl(text);
    return text;
  }
  ```
This allows a rectangle to be created within the scene with the options to change the properties 
  ```javascript
  function createRectangle(scene: Scene, w: string, h: string, x: string, y: string, cr: number, c: string, t: number, bg: string, advtext) {
    let rectangle = new GUI.Rectangle();
    rectangle.width = w;
    rectangle.height = h;
    rectangle.left = x;
    rectangle.top = y;
    rectangle.cornerRadius = cr;
    rectangle.color = c;
    rectangle.thickness = t;
    rectangle.background = bg;
    advtext.addControl(rectangle);
    return rectangle;
  }
  ```
This allows a button to be created within the scene with the options to change the properties 
  ```javascript
  function createSceneButton(scene: Scene, name: string, index: string, x: string, y: string, advtex/*, val: number*/){
    let button = GUI.Button.CreateSimpleButton(name, index);
        button.left = x;
        button.top  =  y;
        button.width = "160px"
        button.height = "60px";
        button.color = "white";
        button.cornerRadius = 20;
        button.background = "purple";
        const buttonClick = new Sound("MenuClickSFX", "./audio/menu-click.wav", scene, null, {
          loop: false,
          autoplay: false,
        });
        button.onPointerUpObservable.add(function() {
            buttonClick.play();
            setSceneIndex(1);
        });
        advtex.addControl(button); 
        return button;
  }
  ```
  The button has also been made to crate a sound when clicked as seen here: 
  ```javascript
 buttonClick.play(); 
  ```
These elements are now being added to the scene to create the menu
  ```javascript
    let advancedTexture = GUI.AdvancedDynamicTexture.CreateFullscreenUI("myUI",true);
    let textBG = createRectangle(that.scene, "300px", "100px", "0px", "-200px", 20, "white", 4, "purple", advancedTexture);
    let titleText = createText(that.scene, "THE GAME", "0px", "-200px", "45", "white", advancedTexture);
    let button1 = createSceneButton(that.scene, "but1", "Start Game","0px", "-75px", advancedTexture);
    let button2 = createSceneButton(that.scene, "but2", "Options","0px", "0px", advancedTexture);

    that.camera = createArcRotateCamera(that.scene);
    that.skybox = createSky(that.scene);
    that.hemisphericLight = createHemiLight(that.scene);

    return that;
  ```

  ## Displaying Elements
  First, the properties have to be defined  

  ```javascript
  interface SceneData 
  {
      scene: Scene;
      terrain?: Mesh;
      ground?: Mesh;
      skybox?: Mesh;
      trees?: SpriteManager;
      box?: Mesh;
      house?: any;
      polyhedra?: Mesh;
      actionManager?: any;
      importMesh?: any;
      light?: Light;
      hemisphericLight?: HemisphericLight;
      camera?: Camera;
  }
  ```
This allows the elements to be displayed within the scene
  ```javascript
  that.actionManager = actionManager(that.scene)
    that.house = cloneHouse(that.scene);
    that.trees = createTrees(that.scene);
    that.skybox = createSkyBox(that.scene);
    that.ground = createGround(that.scene);
    that.box = createBoxPhysics(that.scene, 2, 2, 2, true);
    that.polyhedra = createPolyhedra(that.scene, 4, 1, 0, 5, 0);
    that.light = createLight(that.scene);
    that.terrain = createTerrain(that.scene);
    that.camera = createArcRotateCamera(that.scene);
    that.importMesh = importPlayerMesh(that.scene, 0, 0);
  ```

