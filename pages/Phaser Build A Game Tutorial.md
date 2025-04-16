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