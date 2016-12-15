# Game? An Introduction to Coding

Slides: https://docs.google.com/presentation/d/1BJMuPPkH3cyHuPbPVLVln8UEhhmZxtxaJgmlxplPPuw/edit?usp=sharing

## 1 Setup Codepen

#### 1.1 Hold `ctrl` (or `cmd` on Mac) and click this link to open it in a new tab https://codepen.io/albertkawmi/pen/JbmdmX?editors=1010

#### 1.2 In the new Codepen tab ensure that you are logged out (in case a previous user had logged in)

#### 1.3 Click the 'Fork' button at the top.

You should see a dark grey bar separating the code editor (left side) from the preview (right side).

#### 1.4 Drag the grey bar to the right to make more room in the code editor.

About halfway should be enough.

## 2 Add the HTML Canvas Element

#### 2.1 In the HTML editor pane you should see the following:
```html
<canvas id="pong" width="480" height="320" style="background: gray"></canvas>
```
Note: if you are starting from scratch and the HTML box is empty, just copy/paste the above code into the HTML box.

#### 2.2 Double click where it says 'JS (babel)' in the bottom editor pane.

This will expand the JS pane and hide the others. From now on, all code we enter will be in the JS editor.

#### 2.3 In the JS pane, you should see the following lines of code:
```javascript
// Get our canvas ready
let canvasElement = document.getElementById('pong');
let canvas = canvasElement.getContext('2d');
let canvasWidth = canvasElement.width;
let canvasHeight = canvasElement.height;
```
(Again, if the pane is empty, copy/paste the above lines in.)

This gets our `canvas` ready to draw on and we grab the `canvasWidth` and `canvasHeight` values so we can use them later.

## 3 Let's define some colours and sizes
Type in the following code:
```javascript
// colours and sizes
let ballColour = 'lime';
let ballRadius = 10;
```
This tells our game what we want the colour and size of the ball to be.

## 4 Create our main gameLoop function
```javascript
gameLoop();

function gameLoop() {
  // some more code for our game will go here later
  requestAnimationFrame(gameLoop);
}
```
This tells the web browser to keep looping the game. We will add more instructions later to tell the game what to do as the ball moves around.

## 5 Draw a line in the middle of the canvas
#### 5.1 Add this at the bottom of our file:
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

#### 5.2 The first shape we will draw is a simple line in the middle of the canvas.

#### 5.3Add this to the bottom of the file:
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
Now we can use `drawLine` in our `gameLoop`.

#### 5.4 Scroll to the `gameLoop` function that we already defined. Insert the `drawLine` function inside the `gameLoop` so that it looks like this:
```javascript
function gameLoop() {
  drawLine();
  requestAnimationFrame(gameLoop);
}
```
So far, our `gameLoop` draws just that same line over and over. Let's add a ball...

## 6 Draw the ball
#### 6.1 Scroll back to where we set the `ballColour` and `ballRadius`. Insert a new line and add this code:
```javascript
// ball position and direction
let x = canvasWidth / 2;
let y = canvasHeight / 2;
```
This means we want the ball to start off in the centre of the canvas.

#### 6.2 Scroll to the bottom of the file and add this function:
```javascript
function drawBall() {
  let startAngle = 0;
  let endAngle = 2 * Math.PI; // 360 degrees
  draw('arc', ballColour, x, y, ballRadius, startAngle, endAngle);
}
```
Remember, we defined the initial values for our ball in step 4.

#### 6.3 Now we can add `drawBall` to our `gameLoop`, just after `drawLine();`
```javascript
function gameLoop() {
  drawLine();
  drawBall();
  requestAnimationFrame(gameLoop);
}
```

## 7 Move the ball
#### 7.1 Scroll back to where we set the `x` and `y` values. Insert a new line and add this code:
```javascript
let dx = 1;
let dy = 1;
```
#### 7.2 Scroll to the bottom of the file and add this function:
```javascript
function moveBall() {
  x = x + dx;
  y = y + dy;
}
```
#### 7.3 Now we use `moveBall` in our `gameLoop`:
```javascript
function gameLoop() {
  drawLine();
  drawBall();
  moveBall();
  requestAnimationFrame(gameLoop);
}
```
#### 7.4 Look at the canvas. You should be able to see the path of the ball.

## 8 Clear the canvas after each update
The canvas is drawing a _new_ ball at a new position every time it updates.

To make it look like the ball is moving, we need to clear the canvas before every update.

#### 8.1 Add this to bottom of the file:
```javascript
function clearCanvas() {
  canvas.clearRect(0, 0, canvasWidth, canvasHeight);
}
```

#### 8.2 Use the `clearCanvas` function at the top of our `gameLoop`:
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

## 9 Bounce the ball off the walls
#### 9.1 Add this to the bottom of the file:
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

#### 9.2 Now we can add `bounceOffWalls` to our `gameLoop`:
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

## 10 Set initial values for the paddles
#### 10.1 Scroll back to just above the `gameLoop` function. Insert a few blank lines above that function.

#### 10.2 Set the colour and dimensions of the paddles:
```javascript
// paddle colour and dimensions
let paddleColour = 'lime';
let paddleHeight = 75;
let paddleWidth = 10;
```

#### 10.3 Then set the starting positions of the paddles:
```javascript
// paddle positions
let leftPaddleX = 0;
let leftPaddleY = (canvasHeight - paddleHeight) / 2;

let rightPaddleX = canvasWidth - paddleWidth;
let rightPaddleY = (canvasHeight - paddleHeight) / 2;
```
This will give us two paddles - one on each edge of the canvas - centred vertcially.

## 11 Draw the paddles
#### 11.1 Add this to the bottom of the file:
```javascript
function drawLeftPaddle() {
  draw('rect', paddleColour, leftPaddleX, leftPaddleY, paddleWidth, paddleHeight);
}

function drawRightPaddle() {
  draw('rect', paddleColour, rightPaddleX, rightPaddleY, paddleWidth, paddleHeight);
}
```
These are two similar functions for drawing rectangles to represent the left and right paddles.

#### 11.2 Add each of these to the `gameLoop` immediately after `drawBall`:
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
## 12 Move the computer paddle
#### 12.1 Add this to the bottom:
```javascript
function moveComputerPaddle() {
  rightPaddleY = y - paddleHeight / 2;
}
```
This will simply keep the right paddle in line with the ball - the computer can't lose!

#### 12.2 Add `moveComputerPaddle` to the `gameLoop`, on the line after `moveBall`:
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

## 13 Bounce the ball off the paddles
#### 13.1 Add this to the bottom of the file:
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

#### 13.2 Add another function at the bottom, starting with just these two lines:
```javascript
function bounceOffPaddles() {
  let leftBoundary = x < paddleWidth;

  let ballHitsLeftPaddle =
    y > leftPaddleY
    && y < leftPaddleY + paddleHeight;
}
```
These are rules to check when the ball is at the left edge of the canvas and whether it has hit the left paddle.

#### 13.3 Next we add a few more lines to this function:
```javascript
function bounceOffPaddles() {
  let leftBoundary = x < paddleWidth;

  let ballHitsLeftPaddle =
    y > leftPaddleY
    && y < leftPaddleY + paddleHeight;

  if (leftBoundary) { 
    handleBoundary(ballHitsLeftPaddle);
  }
}
```

We have now finished setting the rules for the left edge and now we need to do the same for the right edge.

#### 13.4 Add the remaining code to the function so that the completed function looks like this:

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
#### 13.5 Lastly, add the `bounceOffPaddles` function to our `gameLoop`, immediately after `bounceOffWalls`:
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

## 14 Move the 'human' paddle
#### 14.1 Add this to the bottom: of the file:
```javascript
function moveLeftPaddle(event) {
  leftPaddleY = event.pageY - canvasElement.offsetTop;
}
```
#### 14.2 Add this line, just before `gameLoop` is first used:
```javascript
// Track mouse movements
document.addEventListener('mousemove', moveLeftPaddle);

gameLoop();
```
