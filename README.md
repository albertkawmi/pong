# Game? An Introduction to Coding

Slides: https://docs.google.com/presentation/d/1BJMuPPkH3cyHuPbPVLVln8UEhhmZxtxaJgmlxplPPuw/edit?usp=sharing

## 1 Setup Codepen

#### 1.1 Go to http://codepen.io/pen

#### 1.2 Click the 'Change View' button on the top right

#### 1.3 Choose the first layout option

#### 1.4 Click the 'Change View' button again to dismiss the menu

#### 1.5 Drag the vertical bar to the right to give yourself more room to write code.

About halfway should be enough.

## 2 Add the HTML Canvas Element

In the HTML box copy/paste the following:

```html
<canvas id="pong" width="480" height="320" style="background: gray"></canvas>
```

## 3 Define colours and sizes

#### 3.1 Double click where it says 'JS (babel)' in the bottom editor pane.

This will expand the JS pane and hide the others. From now on, all code we enter will be in the JS editor.

#### 3.2 Enter the following code:

```javascript
// colours and sizes
let ballColour = 'lime';
let ballRadius = 10;

let paddleColour = 'lime';
let paddleHeight = 75;
let paddleWidth = 10;
```
This tells our game what we want the colour and size of the ball, and the paddles to be.

## 4 Set the initial position of the ball

```javascript
// ball position and direction
let x = canvasWidth / 2;
let y = canvasHeight / 2;

let dx = 1;
let dy = 1;
```
This tells the game that want our ball to start in the middle of the canvas and which direction it should start moving in.

## 5 Set initial values for the paddles
```javascript
// paddle positions
let leftPaddleX = 0;
let leftPaddleY = (canvasHeight - paddleHeight) / 2;

let rightPaddleX = canvasWidth - paddleWidth;
let rightPaddleY = (canvasHeight - paddleHeight) / 2;
```
This tells the game where paddles are position at the start.

## 6 Create our main gameLoop function

```javascript
gameLoop();

function gameLoop() {
  // some more code for our game will go here later
  requestAnimationFrame(gameLoop);
}
```
This tells the web browser to keep looping the game. We will add more instructions later to tell the game what to do as the ball moves around.

## 7 Draw a line in the middle of the canvas
#### 7.1 At the very top of our file, insert these lines:
```javascript
let canvasElement = document.getElementById('pong');
let canvas = canvasElement.getContext('2d');
let canvasWidth = canvasElement.width;
let canvasHeight = canvasElement.height;
```
This gets our `canvas` ready to draw on and we grab the `canvasWidth` and `canvasHeight` values so we can use them later.

#### 7.2 Add this at the bottom of our file:
```javascript
function draw(shape, colour, ...details) {
  canvas.beginPath();
  canvas[shape](...details);
  canvas.fillStyle = colour;
  canvas.fill();
  canvas.closePath();
}
```
We will use this function in a few places to draw different shapes on the canvas.

#### 7.3 The first shape we will draw is a simple line in the middle of the canvas.

#### Add this to the bottom of the file:
```javascript
function drawLine() {
  let lineX = canvasWidth / 2;
  let lineY = 0;
  let lineWidth = 2;
  let lineHeight = canvasHeight;
  let lineColour = 'white';
  
  draw('rect', lineColour, lineX, lineY, lineWidth, lineHeight);
}
```

#### 7.4 Now we can use `drawLine` in our `gameLoop`:
```javascript
function gameLoop() {
  drawLine();
  requestAnimationFrame(gameLoop);
}
```
So far, our `gameLoop` draws just that same line over and over. Let's add a ball...

## 8 Draw the ball
#### 8.1 Add this to the bottom of the file:
```javascript
function drawBall() {
  let startAngle = 0;
  let endAngle = 2 * Math.PI; // 360 degrees
  draw('arc', ballColour, x, y, ballRadius, startAngle, endAnngle);
}
```
Remember, we defined the initial values for our ball in step 4.

#### 8.2 Now we can add `drawBall` to our `gameLoop`, just after `drawLine();`
```javascript
function gameLoop() {
  drawLine();
  drawBall();
  requestAnimationFrame(gameLoop);
}
```

## 9 Move the ball
#### 9.1 Add this to the bottom of the file:
```javascript
function moveBall() {
  x = x + dx;
  y = y + dy;
}
```
#### 9.2 Now we use `moveBall` in our `gameLoop`:
```javascript
function gameLoop() {
  drawLine();
  drawBall();
  moveBall();
  requestAnimationFrame(gameLoop);
}
```
#### 9.3 Look at the canvas. You should be able to see the path of the ball.

## 10 Clear the canvas after each update
The canvas is drawing a _new_ ball at a new position every time it updates.

To make it look like the ball is moving, we need to clear the canvas before every update.

#### 10.1 Add this to bottom of the file:
```javascript
function clearCanvas() {
  canvas.clearRect(0, 0, canvasWidth, canvasHeight);
}
```

#### 10.2 Use the `clearCanvas` function at the top of our `gameLoop`:
```javascript
function gameLoop() {
  clearCanvas();
  drawLine();
  drawBall();
  moveBall();
  requestAnimationFrame(gameLoop);
}
```
You should see the ball move down and off the canvas.

## 11 Bounce the ball off the walls
#### 11.1 Add this to the bottom of the file:
```javascript
function bounceOffWalls() {
  let ballTouchesTop = y < 0;
  let ballTouchesBottom = y > canvasHeight;

  if (ballTouchesTop || ballTouchesBottom) {
    dy = -dy;
  }
}
```
This just means that if the ball hits the bottom _or_ the top it will bounce and go in the _opposite_ direction.

#### 11.2 Now we can add `bounceOffWalls` to our `gameLoop`:
```javascript
function gameLoop() {
  clearCanvas();
  drawLine();
  drawBall();
  moveBall();
  bounceOffWalls();
  requestAnimationFrame(gameLoop);
}
```

## 12 Draw the paddles
#### 12.1 Add this to the bottom of the file:
```javascript
function drawLeftPaddle() {
  draw('rect', paddleColour, leftPaddleX, leftPaddleY, paddleWidth, paddleHeight);
}

function drawRightPaddle() {
  draw('rect', paddleColour, rightPaddleX, rightPaddleY, paddleWidth, paddleHeight);
}
```
These are two similar functions for drawing rectangles to represent the left and right paddles.

#### 12.2 Add each of these to the `gameLoop` immediately after `drawBall`:
```javascript
drawPaddle(leftPaddleX, leftPaddleY, paddleWidth, paddleHeight);
```
This will draw the _left_ paddle.

#### 12.3 On the following line, add `drawPaddle` again, this time for the _right_ paddle:
```javascript
drawPaddle(rightPaddleX, rightPaddleY, paddleWidth, paddleHeight);
```
#### 12.4 Check your code. The `gameLoop` function should now look like this:
```javascript
function gameLoop() {
  clearCanvas();
  drawLine();
  drawBall();
  drawLeftPaddle();
  drawRightPaddle();
  moveBall();
  bounceOffWalls();
  requestAnimationFrame(gameLoop);
}
```
## 13 Move the computer paddle
#### 13.1 Add this to the bottom:
```javascript
function moveComputerPaddle() {
  rightPaddleY = y - paddleHeight / 2;
}
```
This will simply keep the right paddle in line with the ball - the computer can't lose!

#### 13.2 Add `moveComputerPaddle` to the `gameLoop`, on the line after `moveBall`:
```javascript
function gameLoop() {
  clearCanvas();
  drawLine();
  drawBall();
  drawLeftPaddle();
  drawRightPaddle();
  moveBall();
  moveComputerPaddle();
  bounceOffWalls();
  requestAnimationFrame(gameLoop);
}
```
## 14 Bounce the ball off the paddles
#### 14.1 Add this to the bottom of the file:
```javascript
function handleBoundary(ballHitPaddle) {    
  if (ballHitPaddle) {
    // change direction of the ball
    dx = -dx;
  } else {
    // ball missed the paddle
    // reset its position to the centre of the canvas
    x = canvasWidth / 2;
    y = canvasHeight / 2;
    dy = -dy;
    dx = -dx;
  }   
}
```
Instead of using in our `gameLoop`, we will use this it in another function in the next step.

#### 14.2 Add another function at the bottom, starting with just these two lines:
```javascript
function bounceOffPaddles() {
  let leftBoundary = x < paddleWidth;

  let ballHitsLeftPaddle =
    y > leftPaddleY
    && y < leftPaddleY + paddleHeight;
}
```
These are rules to check when the ball is at the left edge of the canvas and whether it has hit the left paddle.

#### 14.3 Next we add a few more lines to this function:
```javascript
function bounceOffPaddles() {
  let leftBoundary = x < paddleWidth;

  let ballHitsLeftPaddle =
    y > leftPaddleY
    && y < leftPaddleY + paddleHeight;

  if (leftBoundary) { 
    handleBoundary(ballHitsLeftPaddle);
  }
```

We have now finished setting the rules for the left edge and now we need to do the same for the right edge.

#### 14.4 Add the remaining code to the function so that the completed function looks like this:

```javascript
function bounceOffPaddles() {
  let leftBoundary = x < paddleWidth;

  let ballHitsLeftPaddle =
    y > leftPaddleY
    && y < leftPaddleY + paddleHeight;
  

  if (leftBoundary) { 
    handleBoundary(ballHitsLeftPaddle);
  }

  let rightBoundary = x > canvasWidth - paddleWidth;
  
  let ballHitsRightPaddle =
    y > rightPaddleY
    && y < rightPaddleY + paddleHeight;
  
  if (rightBoundary) {
    handleBoundary(ballHitsRightPaddle);
  }
}
```
#### 14.5 Lastly, add the `bounceOffPaddles` function to our `gameLoop`, immediately after `bounceOffWalls`:
```javascript
function gameLoop() {
  clearCanvas();
  drawLine();
  drawBall();
  drawLeftPaddle();
  drawRightPaddle();
  moveBall();
  moveComputerPaddle();
  bounceOffWalls();
  bounceOffPaddles();
  requestAnimationFrame(gameLoop);
}
```
## 15 Move the 'human' paddle
#### 15.1 Add this to the bottom: of the file:
```javascript
function moveLeftPaddle(event) {
  leftPaddleY = event.pageY - canvasElement.offsetTop;
}
```
#### 15.2 Add this line, just before `gameLoop` is first used:
```javascript
// Track mouse movements
document.addEventListener('mousemove', moveLeftPaddle);

gameLoop();
```
