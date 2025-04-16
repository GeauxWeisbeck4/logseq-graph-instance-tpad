tags:: phaserjs, game-dev, tutorial, programming, javascript, deno, typescript

- title:: [[Phaser Build A Game Tutorial]]
- link:: https://phaser.io/tutorials/making-your-first-phaser-3-game/part1
- # Build a Game With Phaser Tutorial
	- ## Introduction
		- Phaser is an HTML5 game framework which aims to help developers make powerful, cross-browser HTML5 games really quickly. It was created specifically to harness the benefits of modern browsers, both desktop and mobile. The only browser requirement is the support of the canvas tag.
	- ## Loading Assets
		- We have an assets folder with our assets. Use the following code to load them onto our HTML canvas
			- ```javascript
			  function preload ()
			  {
			      this.load.image('sky', 'assets/sky.png');
			      this.load.image('ground', 'assets/platform.png');
			      this.load.image('star', 'assets/star.png');
			      this.load.image('bomb', 'assets/bomb.png');
			      this.load.spritesheet('dude', 
			          'assets/dude.png',
			          { frameWidth: 32, frameHeight: 48 }
			      );
			  }
			  ```
			- This will load in 5 assets: 4 images and a sprite sheet. It may appear obvious to some of you, but I would like to point out the first parameter, also known as the asset key (i.e. 'sky', 'bomb'). This string is a link to the loaded asset and is what you'll use in your code when creating Game Objects. You're free to use any valid JavaScript string as the key.
	- ## Display an Image
		- In order to display one of the images we've loaded place the following code inside the `create` function:
			- `this.add.image(400, 300, 'sky');`
		- The values `400` and `300` are the x and y coordinates of the image. Why 400 and 300? It's because in Phaser 3 all Game Objects are positioned based on their center by default. The background image is 800 x 600 pixels in size, so if we were to display it centered at 0 x 0 you'd only see the bottom-right corner of it. If we display it at 400 x 300 you see the whole thing.
		- **Hint:** You can use `setOrigin` to change this. For example the code: `this.add.image(0, 0, 'sky').setOrigin(0, 0)` would reset the drawing position of the image to the top-left. In Phaser 2 this was achieved via the `anchor` property but in Phaser 3 it's the `originX` and `originY` properties instead.
		- The order in which game objects are displayed matches the order in which you create them. So if you wish to place a star sprite above the background, you would need to ensure that it was added as an image second, after the sky image: