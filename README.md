# Game? An Introduction to Coding

Slides: https://docs.google.com/presentation/d/1BJMuPPkH3cyHuPbPVLVln8UEhhmZxtxaJgmlxplPPuw/edit?usp=sharing

## 1 Setup Codepen

1.1 Go to http://codepen.io/pen

1.2 Click the 'Change View' button on the top right

1.3 Choose the first layout option

1.4 Click the 'Change View' button again to dismiss the menu

1.5 Drag the vertical bar to the right to give yourself more room to write code.

About halfway should be enough.

## 2 Add the HTML Canvas Element

In the HTML box enter the following:

```html
<canvas id="pong" width="480" height="320" style="background: gray"></canvas>
```

## 3 Define some constant values

3.1 Double click where it says 'JS (babel)' in the bottom editor pane.

This will expand the JS pane and hide the others. From now on, all code we enter will be in the JS editor.

3.2 Enter the following code:

```javascript
// Define some constants to use throughout the game
let colour = 'lime';
let ballRadius = 10;
let paddleHeight = 75;
let paddleWidth = 10;
```
These are constant values about our game that will not change.

## 4 Set initial values for the ball

```javascript
// Initial values for the ball
let x = canvasWidth / 2;
let y = canvasHeight / 2;

let dx = 1;
let dy = 1;
```
The variables `x` and `y` will store the position of the centre of the ball.

`dx` and `dy` are the amounts that the ball will move across the canvas every time we update it.

## 5 Set initial values for the paddles
```javascript
// Inital values for the players
let leftPlayerX = 0;
let leftPlayerY = (canvasHeight - paddleHeight) / 2;

let rightPlayerX = canvasWidth - paddleWidth;
let rightPlayerY = (canvasHeight - paddleHeight) / 2;
```

## 6 Create our main gameLoop function
This is a function that will run every time the web browser is ready to update the canvas.

6.1 First, define this function at the bottom of the file:
```javascript
function gameLoop() {
  // wait for the next frame to run gameLoop again
  requestAnimationFrame(gameLoop);
}
```

6.2 Then, above that function definition we will 'call' (i.e. use) that function:
```javascript
gameLoop();
```

## 7 Draw a line in the middle of the canvas
7.1 Define the following function at the bottom of our file:
```javascript
function drawLine() {
  canvas.beginPath();
  canvas.rect(canvasWidth / 2, 0, 2, canvasHeight);
  canvas.fillStyle = 'white';
  canvas.fill();
  canvas.closePath();
}
```
7.2 Then call this function inside our `gameLoop`, as the first line of code _inside_ the function:
```javascript
function gameLoop() {
  drawLine();
  // wait for the next frame to run gameLoop again
  requestAnimationFrame(gameLoop);
}
```

## 8 Draw the ball
8.1 Define this function at the bottom of the file:
```javascript
function drawBall() {
  canvas.beginPath();
  canvas.arc(x, y, ballRadius, 0, Math.PI * 2);
  canvas.fillStyle = colour;
  canvas.fill();
  canvas.closePath();
}
```
8.2 Then call it in our `gameLoop` function, just after `drawLine();`
```javascript
function gameLoop() {
  drawLine();
  drawBall();
  // wait for the next frame to run gameLoop again
  requestAnimationFrame(gameLoop);
}
```

## 9 Move the ball
9.1 Define this function at the bottom of the file:
```javascript
function moveBall() {
  x = x + dx;
  y = y + dy;
}
```
9.2 Call `moveBall` from inside `gameLoop`:
```javascript
function gameLoop() {
  drawLine();
  drawBall();
  moveBall();
  // wait for the next frame to run gameLoop again
  requestAnimationFrame(gameLoop);
}
```
9.3 Look at the canvas. You should be able to see the path of the ball.

## 10 Clear the canvas after each frame
The canvas is draw a _new_ ball at a new position in every frame.

To make it look like the ball is moving, we need to clear the canvas before every frame.

10.1 Define the following function at the bottom of the file:
```javascript
function clearCanvas() {
  canvas.clearRect(0, 0, canvasWidth, canvasHeight);
}
```

10.2 Call the `clearCanvas` function at the top of our `gameLoop`:
```javascript
function gameLoop() {
  clearCanvas();
  drawLine();
  drawBall();
  moveBall();
  // wait for the next frame to run gameLoop again
  requestAnimationFrame(gameLoop);
}
```
You should see the ball move down and off the canvas.

To start the animation again, change the value of `dx` to `2`.

Any change will refresh the game.

## 11 Bounce the ball of the walls
11.1 Define this function at the bottom:
```javascript
function bounceOffWalls() {
  if (y + dy < 0) {
    dy = -dy;
  }
  if (y + dy > canvasHeight) {
    dy = -dy;
  }
}
```
11.2 Call `bounceOffWalls` inside our `gameLoop`:
```javascript
function gameLoop() {
  clearCanvas();
  drawLine();
  drawBall();
  moveBall();
  bounceOffWalls();
  // wait for the next frame to run gameLoop again
  requestAnimationFrame(gameLoop);
}
```

## 12 Draw the paddles
12.1 Define this function at the bottom:
```javascript
function drawPaddle(x0, y0, w, h) {
  canvas.beginPath();
  canvas.rect(x0, y0, w, h);
  canvas.fillStyle = colour;
  canvas.fill();
  canvas.closePath();
}
```
Note that this is our first function that takes _arguments_.

This is so that we can use the same function to draw _both_ paddles by passing in different arguments for each paddle.

12.2 Inside the `gameLoop` function call the `drawPaddle` function immediately after `drawBall`:
```javascript
drawPaddle(leftPaddleX, leftPaddleY, paddleWidth, paddleHeight);
```
12.3 On the following line, call the function again, this time with arguments for the _right_ paddle:
```javascript
drawPaddle(rightPaddleX, rightPaddleY, paddleWidth, paddleHeight);
```
12.4 Check your code. The `gameLoop` function should now look like this:
```javascript
function gameLoop() {
  clearCanvas();
  drawLine();
  drawBall();
  drawPaddle(leftPaddleX, leftPaddleY, paddleWidth, paddleHeight);
  drawPaddle(rightPaddleX, rightPaddleY, paddleWidth, paddleHeight);
  moveBall();
  bounceOffWalls();
  // wait for the next frame to run gameLoop again
  requestAnimationFrame(gameLoop);
}
```
## 13 Move the computer paddle
13.1 Define this function at the bottom:
```javascript
function moveComputerPaddle() {
  rightPaddleY = y - paddleHeight / 2;
}
```
This will simply keep the right paddle in line with the ball - it can't lose!

13.2 Call `moveComputerPaddle` inside the `gameLoop`, on the line after `moveBall` is called:
```javascript
function gameLoop() {
  clearCanvas();
  drawLine();
  drawBall();
  drawPaddle(leftPaddleX, leftPaddleY, paddleWidth, paddleHeight);
  drawPaddle(rightPaddleX, rightPaddleY, paddleWidth, paddleHeight);
  moveBall();
  moveComputerPaddle();
  bounceOffWalls();
  // wait for the next frame to run gameLoop again
  requestAnimationFrame(gameLoop);
}
```
