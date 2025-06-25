# Setup
We'll discuss phaser.js setup with express.js in this tutorial.
In order to use phaser.js, we run `npm i phaser` and then add the path as a static delivery in our express app:
```javascript
app.use(
	'/phaser', 
	express.static(path.join(__dirname, 'node_modules/phaser/dist'))
);
```

Now, we can use the phaser.js code in our frontend html file like we use a CDN.
```html
<script src="/phaser/phaser.min.js"></script>  
<script type="module" src="./script.js"></script>
```

Now we can access the `Phaser` object in the script.js file.
We need to add some boiler plate code in the script.js:
```javascript
const config = {  
    type: Phaser.AUTO,  
    width: 800,  
    height: 600,  
    scene: {  
        preload: preload,  
        create: create,  
        update: update,  
    }}  
  
const game = new Phaser.Game(config);  
  
function preload() {}  
function create() {}  
function update() {}
```

The `type` property can be either `Phaser.CANVAS`, `Phaser.WEBGL`, or `Phaser.AUTO`. This is the rendering context that you want to use for your game. The recommended value is `Phaser.AUTO` which automatically tries to use WebGL, but if the browser or device doesn't support it it'll fall back to Canvas. The canvas element that Phaser creates will be simply be appended to the document at the point the script was called, but you can also specify a parent container in the game config should you wish.

We can use the `preload()` function to load assets for our game. We use the `this.load` object to do this.

| Method definition                                                                                                                                             | Description           |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| `this.load.image(str assetkey, str url)`<br><br>Ex:`this.load.image('bomb', 'assets/bomb.png')`                                                               | Loads the image asset |
| `this.load.spritesheet(str assetkey, str url, obj options)`<br><br>Example: `this.spritesheet('dude', 'assets/dude.png',{ frameWidth: 32, frameHeight: 48 })` | Loads the spritesheet |
