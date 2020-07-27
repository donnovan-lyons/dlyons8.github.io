---
layout: post
title:      "Tutorial: Remaking My Favorite Classic Snake Game with ES6"
date:       2020-06-10 12:40:04 +0300
permalink:  tutorial_remaking_my_favorite_classic_snake_game_with_es6
---

Whenever you take on a new project, it can be daunting. None of the coursework I took in the Flatiron School taught me specifically how to create a snake that grows every time it eats an apple. Even more so, similar to my earlier Sinatra project, there was no real file structure or scaffold that was created for me like there was for my first Ruby Gem, or the Rails project I did much later. Nevertheless, if you find yourself stuck, my advice is just to go forward and code away. It can be overwhelming when you're starting out on new terrain, and you don't always know how you're going to build what you want to build. It can take a lot of research, experimentation, and cleaning up after you've made a mess of the code in your project. But the purpose of these projects is not only to show yourself and others what you're capable of, but I've found each and every single one of them useful in helping me to solidify the concepts I've learned throughout the curriculum. In essence, you're still very much learning.

I for one, found it amazing what one is able to learn by following walkthroughs. With a little help, starting off with Mozilla's own tutorial on how to build the game [2D breakout](https://developer.mozilla.org/en-US/docs/Games/Tutorials/2D_Breakout_game_pure_JavaScript), I found that I was able to integrate that knowledge with my knowledge of JS, and accomplish my goal of making my own remake of the classic game, Snake. Here is how I did it, the tutorialized version:

### Creating the User Interface

The first thing to do is create an HTML file, and in it, to include the HTML necessities. 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

In the body of the HTML, we're going to start by placing three divs inside one div. The first inner div will be where we input the player's name. The second inner div will be the option menu. The third div will be the high score screen. We'll give them the ids of `name-screen`, `start-screen`, and `high-score-screen` respectively. We'll give the div that everything is inside of the id, `game`. Let's also add a canvas element in there, with an id of `canvas`. We'll give it height and width attributes of 400px.

It should like below:

```
    <div id="game">

        <div id="name-screen">
        </div>

        <div id="start-screen">
        </div>

        <canvas id="canvas" height="400px" width="400px"></canvas>

        <div id="high-score-screen">
        </div>
        
    </div>
```

You're probably wondering, which of these divs are going to contain the actual game? The  answer for that is  the `canvas`  element, which relies on the canvas API to be drawn on by JavaScript functions within the region set by the height and  width  parameters which we set in the canvas element above.

Next, we're  going to fill out our inner divs. These divs have nothing to do with  the actual  functionality of  Snake, but instead the functionality of  our game. We want a User to be  able to input their  name. We then  want a User to be able to select whether they want to restore an old game, or a new game. Then we want that User to be able to see the list of high scores after the game is completed, and whether or not he is on there.

So for our `name-screen` div, we're going to put a  form inside of it, with a text field, and a submit button. The form should have an id of `input-form` like so:

```
<form id="input-form">
    <label>Enter your name:</label> <br>
    <input type="text" id="name"><br>
    <input type="submit">
    <p>Press ENTER To Continue</p>
</form>
```
For the `start-screen` div, we'll put the two options for "Start New Game" and "Restore Game" inside `h1` headers with id's of `start` and `restore` respectively.

```
<h1 id="start"><em>Start New Game</em></h1>
<h1 id="restore"><em>Restore Game</em></h1>
```

Finally, for the `high-score-screen`, we're going to put a h1 header, table with id of `high-scores`, and two buttons with ids of `play-again` and `choose-new-name`.

The inside of the div should have something like the following elements

```
<h1>High Score</h1>
<table id="high-scores">
</table>
<button id="play-again">Play Again</button><br>
<button id="choose-new-name">Choose New User Name</button>
```

After all of the above coding, the body of the HTML should look like what's in the repo [here](https://github.com/dlyons8/snapple/blob/master/snapple-frontend/index.html).

### From Here on Out, JavaScript Only

The idea behind the three inner divs came to me, when I thought about the user experience, and how the game should start up. Each div represents a point in time for when the user has to make a choice. For example, what name do they want to give? Then, do they want to start a new game, or restore an old game? Finally, once they're taking a look at the list of high scores, do they want to play again, or choose a new user name?

The reason we gave all of these divs and the elements inside of them IDs, is so that they can easily be called upon by JavaScript later and assigned to a variable of our choosing.

Let's do that now, and then declare every div element that is not the `name-screen` div invisible by changing it's css display to "none". We'll also make the restore button element invisible, because as you'll see later, we only want it to display when a user has a previously unfinished game.

```
const canvas = document.getElementById('canvas'); 
canvas.style.display = "none"
const ctx = canvas.getContext("2d"); 
const nameScreen = document.getElementById('name-screen')
const startScreen = document.getElementById('start-screen')
startScreen.style.display = "none"
const startScreenInput = document.getElementById('input-form')
const startButton = document.getElementById('start')
const restoreButton = document.getElementById('restore')
restoreButton.style.display = "none"
const highScoreScreen = document.getElementById('high-score-screen')
highScoreScreen.style.display = "none"
const playAgainButton = document.getElementById('play-again')
const newNameButton = document.getElementById('choose-new-name')
```

Above, we have declared our canvas, and called `getContext()` on it to return a context for us to be able to draw on the canvas using JavaScript. We also declared the variables for each inner div, and their buttons.

Next, we'll want to declare variables that we assign values later. We'll need to capture the `name` once input by the User. We'll also need the data for any restored game, so we'll just call that `restoredGameData`, and an `interval`. We'll add them below the previously assigned values.

```
let name
let interval
let restoredGameData
```

So now, we're going to create our objects. We need a `Game`, a `Snake`, and `Consumables`. Using ES6, we'll begin the construction of each object below our declared and assigned/unassigned variables.

### The Game Class

First, let's start with the `Game` variable. ES6 declaration for a new class looks like the following:

```
class Game {
    constructor() {
    }
       
}
```

Each `Game` will have one `Snake`, two `Consumables`, an id, a player, a score, and whether or not it was completed. So we'll need to feed it that information once a new `Game` is constructed with some default values. Note that our `Consumables` will be the `apple` and `skull`.

```
class Game {
    constructor(id = null, player, snake, score = 0, apple, skull, completed = false) {
        this.id = id;
        this.player = player;
        this.snake = snake;
        this.score = score;
        this.apple = apple;
        this.skull = skull;
        this.completed = completed;

        if (this.id == null) {
            this.init()
        }
        Game.backGroundMusic.play();
    }
```

Above, you'll also find the `this.init()` function and `Game.backGroundMusic.play()`. These are part of the functions we'll define later as we build out the functionality of the game.

First, we'll add `init()` which saves the game to our database.

```
init() {
        const game = {id: this.id, name: this.player, snake: this.snake, score: this.score, apple: this.apple, skull: this.skull, completed: this.completed}
        return fetch("http://localhost:3000/api/v1/games", {
        method: "POST",
        headers: {
            "Content-Type": "application/json"},
            body: JSON.stringify(game)
        })
        .then(resp => resp.json())
        .then(json => this.id = json.id) //we want to return the id that was created and make our id it
    }
```

The `checkContact` function is one of the most important functions in how we differentiate our game between the other classic Snake games. It's where we define the `Game`, `Snake`, and `Consumable` interactions that occur, and how each of those components react to one another. For example, when the `Snake` touches the skull `Consumable`, the `Snake` get a speed boost, but there's no growth, but when the Snake touches an apple `Consumable`, it grows by one segment. If I were to add any more features, this is certainly a function that would need to change to reflect that.

```
    checkContact() {
        for (let index = 1; index < this.snake.body.length; index++) {
            if (this.snake.x == this.snake.body[index].x && this.snake.y == this.snake.body[index].y) {
                this.gameOver();
            }        
        }
        if (this.snake.x == this.apple.x && this.snake.y == this.apple.y) {
            Game.eatAppleSound.play();
            this.score += 10
            this.snake.grow();
            this.apple.spawn()
            this.save()
        }
        if (this.snake.x == this.skull.x && this.snake.y == this.skull.y) {
            Game.skullBoostSound.play()
            this.score += 25
            clearInterval(interval);
            this.skull.spawn()
            interval = setInterval(this.draw.bind(this), 80);
            setTimeout(this.pause.bind(this), 8000);
            setTimeout(this.resume.bind(this), 8001);
        }
    }
```

Then we have a couple of functions that draws the snake, starts and ends the game, and then retrieves the high scores from the database once the game has ended.

```
    draw() {
        this.snake.slither()
        this.checkContact()
    
        //paint background
        ctx.fillStyle = "black";
        ctx.fillRect(0, 0, canvas.width, canvas.height); 
    
        this.snake.draw()
    
        this.apple.draw()
        this.skull.draw()
        this.drawScore()
    }

    start() {
        document.addEventListener("keydown", keyDownEvent); 
        let x = 8;
        interval = setInterval(this.draw.bind(this), 1000 / x);
    }

    gameOver() {
        this.completed = true
        this.pause()
        canvas.style.display = "none"
        highScoreScreen.style.display = ""
    }

    static appendHighScores(data) {
        const highScoreTable = document.getElementById('high-scores')
        highScoreTable.innerHTML = ""
        for (let i = 0; i < data.length; i++) {
            const element = data[i];
            const tr = document.createElement('tr');
            const rank = document.createElement('td')
            const name = document.createElement('td')
            const score = document.createElement('td')
            rank.innerText = `${i + 1}.`
            name.innerText = element.user.name
            score.innerText = element.score
            tr.appendChild(rank)
            tr.appendChild(name)
            tr.appendChild(score)
            highScoreTable.appendChild(tr)
        } 
    }

```

You'll notice that unlike the rest of the functions, the `appendHighScores()` has the word "static" before it's declaration. This is to show in ES6 that regardless of how many objects of this class are created, this function belongs to all objects of that class, i.e. it is shared by all object types. So, when it's called, it's called not as a function of the instant, but as a function of the class, e.g. `Game.appendHighScores(data)`.

We'll declare another class function below that.

```
    static backGroundMusic = (function() {
        const audio = new Audio("src/audio/skull-snake-game-music.m4a")
        audio.loop = true
        return audio
    })()
    static eatAppleSound = new Audio("src/audio/eat-apple.ogg");
    static ratDeathSound = new Audio("src/audio/rat-death.mp3");
    static skullBoostSound = new Audio("src/audio/skull-boost.wav");
    static lightningStrikeSound = new Audio("src/audio/lightning-strike.wav");
```

And then create the `pause` and `resume` functions that save by ceasing all movement in the game and updating the Game's data in the database.

```
    pause() {
        Game.backGroundMusic.pause();
        clearInterval(interval);
        this.save()
    }

    resume() {
        Game.backGroundMusic.play();
        let x = 8;
        interval = setInterval(this.draw.bind(this), 1000 / x);
    }

    save() {
        const game = {id: this.id, name: this.player, snake: this.snake, score: this.score, apple: this.apple, skull: this.skull, completed: this.completed}
        return fetch(`http://localhost:3000/api/v1/games/${this.id}`, {
        method: "PATCH",
        headers: {
            "Content-Type": "application/json"},
            body: JSON.stringify(game)
        })
        .then(resp => resp.json())
        .then(json => {
            if (game.completed === true) {
                Game.appendHighScores(json)
            }
        })
    }
```
Lastly, we need to keep track of the score, and our `Game` will do just that.

```
    drawScore() {
        ctx.font = "16px Arial";
        ctx.fillStyle = "brown";
        ctx.fillText("Score: "+this.score, 8, 20);
        // ctx.fillText("High Score: "+, canvas.width-65, 20);
    }
```

When you're done. The `Game` class should look like[this](https://github.com/dlyons8/snapple/blob/master/snapple-frontend/src/javascripts/game.js).

### The Snake Class

So our `Snake` object is going to need an image, head coordinates, a body with each segment being a coordinate, a speed, and a direction. Before we do that though, we'll need a grid size, so let's declare and assign a grid size of 20, considering that our canvas is 400x400 pixels, a pixel size of 20x20 px is only 1/20th of that which is perfect for our game.

```
let gridSize = 20;
```

Then let's begin defining Snake. We'll need a default value for each of these if we're constructing a new `Snake` for a new `Game`. So let's randomize the initial coordinates, and direction. If the body is empty, then we'll add two segments to it, giving it a total length of 3 segments, each 20x20 pixels.

```
class Snake {
    constructor(imageFile, x = Math.floor(Math.random() * gridSize), y = Math.floor(Math.random() * gridSize), direction = ["left", "up", "right", "down"][Math.floor(Math.random() * 4)], body = [], speed = 0) {
            this.x = x;
            this.y = y;
            this.direction = direction;
            this.speed = speed;
            this.directions = {"left": {x: -1,y: 0}, "up": {x:0, y:-1}, "right": {x: 1, y:0}, "down": {x:0, y:1}}
            this.image = this.loadImage(imageFile)

            this.body = body;
            
            if (this.body.length == 0) {
                for (let index = 0; index < 3; index++) {
                    this.body.push({x: this.x - index * this.directions[direction].x, y: this.y - index * this.directions[direction].y})
                }
            }
    } 
}
```

After that we'll need a function that is able to load the imageFile so that we can assign the object's image.

```
  loadImage(imageFile) {
        const image = new Image();
        image.onload = function() {}
        image.src = imageFile;
    
        return image;
    }

```

We'll need the `Snake` to be able to move in his direction, with his body to follow,

```
    slither() {
        this.x += this.directions[this.direction].x
        this.y += this.directions[this.direction].y
        if (this.x < 0) {
            this.x = gridSize - 1;
        }
        if (this.x > gridSize - 1) {
            this.x = 0;
        }
    
        if (this.y < 0) {
            this.y = gridSize - 1;
        }
        if (this.y > gridSize - 1) {
            this.y = 0;
        }
        for (let i = this.body.length - 1; i > 0; i--) {
            this.body[i].x = this.body[i-1].x
            this.body[i].y = this.body[i-1].y
        }
        this.body[0].x = this.x
        this.body[0].y = this.y
    }
```

and for the `Snake` to grow once it's made contact with a `Consumable`, and then to be drawn.

```

    grow() {
        this.x += this.directions[this.direction].x
        this.y += this.directions[this.direction].y
        this.body.unshift({x: this.x, y: this.y})
    }
    

    draw() {
        for (let index = 0; index < this.body.length; index++) {
            // draw head
            const snakePart = this.body[index];
            if (index == 0) {
                if (this.direction == "left") {
                    ctx.drawImage(this.image, 192, 64, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                } else if (this.direction == "up") {
                    ctx.drawImage(this.image, 192, 0, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                } else if (this.direction == "right") {
                    ctx.drawImage(this.image, 256, 0, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                } else {
                    ctx.drawImage(this.image, 256, 64, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                }
            } else if (index == this.body.length - 1) {
                // draw tail
                const adj = this.body[index - 1]
                if (snakePart.x < adj.x) {
                    ctx.drawImage(this.image, 256, 128, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                } else if (snakePart.x > adj.x) {
                    ctx.drawImage(this.image, 192, 192, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                } else if (snakePart.y < adj.y) {
                    ctx.drawImage(this.image, 256, 192, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                } else {
                    ctx.drawImage(this.image, 192, 128, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                }
            } else {
                // draw in between
                const toTail = this.body[index + 1]
                const toHead = this.body[index - 1]
                if (toHead.x > snakePart.x && toTail.y > snakePart.y || toTail.x > snakePart.x && toHead.y > snakePart.y) {
                    ctx.drawImage(this.image, 0, 0, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                } else if (toHead.y < snakePart.y && toTail.x > snakePart.x || toTail.y < snakePart.y && toHead.x > snakePart.x) {
                    ctx.drawImage(this.image, 0, 64, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                } else if (snakePart.y == toTail.y && snakePart.y == toHead.y) {
                    ctx.drawImage(this.image, 64, 0, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                } else if (toHead.x < snakePart.x && toTail.y > snakePart.y || toTail.x < snakePart.x && toHead.y > snakePart.y) {
                    ctx.drawImage(this.image, 128, 0, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                } else if (snakePart.x == toTail.x && snakePart.x == toHead.x) {
                    ctx.drawImage(this.image, 128, 64, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                } else {
                    ctx.drawImage(this.image, 128, 128, 64, 64, this.body[index].x * gridSize, this.body[index].y * gridSize, gridSize, gridSize);
                }
            }
        }
    }
```

### The Consumable

This class will be the easiest to define, as it's the smallest with very few changing variables. We will only want coordinates, the ability to change those coordinates once the `Snake` makes contact, and  to  load  the image that will be representing the consumable.

To do that, we'll create a `loadImage` function that we'll call upon constructing the instance of the class, and randomize the initial `x` and `y` values of the  `Consumable`. We'll copy that randomization of the `x` and `y` values into a `spawn` function that we'll call every time we need  the location of the `Consumable` to change.

```
class Consumable {
    constructor(imageFile, x = Math.floor(Math.random() * 20), y = Math.floor(Math.random() * 20)){
        this.image = this.loadImage(imageFile)
        this.x = x
        this.y = y
    }

    loadImage(imageFile) {
        const image = new Image();
        image.onload = function() {}
        image.src = imageFile;
    
        return image;
    }

    draw() {
        ctx.drawImage(this.image, this.x * gridSize, this.y * gridSize, gridSize, gridSize)
    }

    spawn() {
      this.x = Math.floor(Math.random() * 20)
      this.y = Math.floor(Math.random() * 20)
    }
}
```

### Putting it all Together

Now that we have our `Game`, `Snake`, and `Consumable` classes define, what we'll want to do is connect all of them.

A `Game` has one `Snake`, and two `Consumables`. One of which is a skull, and the next which is an apple. We'll then go back to the top where we initially declared the variables, and beneath those, we'll declare some new ones.

```
let snake = new Snake("src/images/snapple.png")
let apple = new Consumable("src/images/apple.png")
let skull = new Consumable("src/images/skull.png")
let newGame
```

If you remember, the first thing a user is going to see when he loads up our game is the div with the form on the screen for him/her to put their name. So the next thing we'll do is create a function called `getName()`. Because we're creating a single page application (SPA) and we don't want the page to be refreshed once our form is submitted, we're going to use `preventDefault()` to prevent the form from refreshing the page. We'll then get the value and assign it's value to our name variable, search for any previous game data, and change which div we display all in this function.

```
function getName(e) {
    e.preventDefault()
    name = document.getElementById("name").value
    restore()
    nameScreen.style.display = "none"
    startScreen.style.display = ""
}
```

The `restore` function however isn't defined. So we'll need to define that next. It will us just returning a fetch request that then calls a function once it's json promise is returned.

```
function restore() {
    return fetch(`http://localhost:3000/api/v1/games/restore/${name}`)
    .then(resp => resp.json())
    .then(json => addGameData(json[0]))
}
```

We'll need to create that function, which is named above, `addGameData`. What we want this function to do for us is assigned our variable `restoredGameData` any data that belongs to our user via their name, and then display the `restoreButton` if there is any incomplete game data.

```
function addGameData(data) {
    restoredGameData = data
    if (restoredGameData) {
        restoreButton.style.display = ""
    } else {
        restoreButton.style.display = "none"
    }
}
```
At this point, we should be showing the `startScreen` div, which displays two `h1` headers, "Start New Game", or "Restore Game." The latter only if there are any unfinished games by that user.

So, because we're not going into a tutorial on how to build the database and APIs, we're just going to focus on what happens when a new game is started.

### Let the Games Begin

We want to add an event listener, so that when the "Start New Game" header is clicked, a new game begins.

So, we'll do that like this:

```
startButton.addEventListener("click", init)
```
We'll also create an `init` function now to initialize the game.

```
function init() {
    startScreen.style.display = "none"
    canvas.style.display = "block"
    newGame = new Game(null, name, snake, 0, apple, skull)
    newGame.start()
}
```

The `init` function displays the canvas and hides the display of the `startScreen` div. It then calls the `start` function on the instance of the newGame that we assign the variables we defined earlier.

So, the game is playing, but wait. We can't move our snake. What's going on?

The answer is, we need to create another event listener for our keys!

So, we'll create a function that takes care of the event when a key is pressed down. If you look above in the code we created in the `Game` class. There's a function called `start` where we add an event listener with a function called `keyDownEvent`. We'll define that now. What we want is if any of our arrow buttons are pressed, the game's snake's direction will reflect that.

```
function keyDownEvent(e) {
    switch (e.keyCode) {
      case 37:
          if (newGame.snake.direction != "right") {
            newGame.snake.direction = "left"
          }
          break;
      case 38:
          if (newGame.snake.direction != "down") {
            newGame.snake.direction = "up"
          }
          break;
      case 39:
          if (newGame.snake.direction != "left") {
            newGame.snake.direction = "right"
          }
          break;
      case 40:
          if (newGame.snake.direction != "up") {
            newGame.snake.direction = "down"
          }
          break;
      case 80:
          newGame.pause()
          break;
      case 82:
          newGame.resume()
          break;
      }
  }
  ```

### Game Over

In the event that the game ends, our fourth div should pop up, the high score div. We have two buttons in that div, "Play Again" and "Choose New User Name."

Thus, we'll need two event listeners.

```
playAgainButton.addEventListener("click", playAgain)
newNameButton.addEventListener("click", playAgainWithNewName)
```

We'll also need to create the functions that go with them as well.

```
function playAgain() {
    snake = new Snake("src/images/snapple.png")
    newGame = new Game(null, name, snake, 0, apple, skull)
    highScoreScreen.style.display = "none"
    canvas.style.display = "block"
    newGame.start()
}

function playAgainWithNewName() {
    document.getElementById("name").value = ""
    highScoreScreen.style.display = "none"
    nameScreen.style.display = ""
    mainImage.style.display= ""
}
```
### Conclusion

And that's how you create the front end of a Snake game.

To add the backend, that's another story.