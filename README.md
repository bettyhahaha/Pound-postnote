# Pound-postnote


// Copyright (c) 2019 ml5
//
// This software is released under the MIT License.
// https://opensource.org/licenses/MIT

/* ===
ml5 Example
PoseNet example using p5.js
=== */

let video;
let poseNet;
let poses = [];

function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.size(width, height);

  // Create a new poseNet method with a single detection
  poseNet = ml5.poseNet(video, modelReady);
  // This sets up an event that fills the global variable "poses"
  // with an array every time new poses are detected
  poseNet.on('pose', function(results) {
    poses = results;
  });
  // Hide the video element, and just show the canvas
  video.hide();
  Poundsetup();
}


function modelReady() {
  select('#status').html('Model Loaded');
}

function draw() {
  image(video, 0, 0, width, height);


  // We can call both functions to draw all keypoints and the skeletons
  drawKeypoints();
  drawSkeleton();
}

// A function to draw ellipses over the detected keypoints
function drawKeypoints() {
  // Loop through all the poses detected
  for (let i = 0; i < poses.length; i++) {
    // For each pose detected, loop through all the keypoints
    let pose = poses[i].pose;
    for (let j = 0; j < pose.keypoints.length; j++) {
      // A keypoint is an object describing a body part (like rightArm or leftShoulder)
      let keypoint = pose.keypoints[j];
      // Only draw an ellipse is the pose probability is bigger than 0.2
      if (keypoint.score > 0.2) {
        fill(255, 0, 0);
        noStroke();
        ellipse(keypoint.position.x, keypoint.position.y, 10, 10);
      }
    }
  }
  Poundraw();
}

// A function to draw the skeletons
function drawSkeleton() {
  // Loop through all the skeletons detected
  for (let i = 0; i < poses.length; i++) {
    let skeleton = poses[i].skeleton;
    // For every skeleton, loop through all body connections
    for (let j = 0; j < skeleton.length; j++) {
      let partA = skeleton[j][0];
      let partB = skeleton[j][1];
      stroke(255, 0, 0);
      line(partA.position.x, partA.position.y, partB.position.x, partB.position.y);
    }
  }
}

///Pound code///
var x, y; // location of the ball
var vx, vy; // speed of the ball
var x2,y2;
var vx2,vy2;

var leftPaddle, rightPaddle;
var PADDLE_WIDTH = 10;
var PADDLE_HEIGHT = 50;
var PADDLE_OFFSET = 30;

var BALL_DIAMETER = 30;
var BALL_DIAMETER2 = 30;
var r;
var g;
var b;
var a;
var scoreLeft=0;
var scoreRight=0;


function gameOver(side) {
  x1=width/3;
  y1=height/3;
  x = width / 2;
  y = height / 2;
  if (side == "left") {
    vx = random(1, 3);
    vx2 = random(2,4);
  } else {
    vx = random(-3, -1);
    vx2 =random(-2,-4);
  }
  vy = random(-1, 1);
  vy2 = random(-2,2);
}



function Poundsetup() {


  x = width / 2;
  y = height / 2;
  x2=width/3;
  y2= height/3;

  vx = 1.5;
  vy = 1;
vx2=2;
  vy=vx2+0.5
  
  rightPaddle = height / 2;
  leftPaddle = height / 2;
}

function Poundraw() {
  background(255);
 
fill(0);
   textSize(20);
 text("score"+scoreLeft,10,30);
 text("score"+scoreRight,550,30);

r = random(255); // r is a random number between 0 - 255
  g = random(100,200); // g is a random number betwen 100 - 200
  b = random(100); // b is a random number between 0 - 100
  a = random(200,255); // a is a random number between 200 - 255
  
 
  stroke(150);
  line(width / 2, 0, width / 2, height);

  fill(0);
  noStroke();
  rectMode(CENTER);
//wrist to move
  if (poses.length > 0) {
    LeftPaddle = poses[0].pose.rightWrist.y;
    rightPaddle = height - poses[0].pose.rightWrist.y;
  }


  rect(PADDLE_OFFSET, leftPaddle, PADDLE_WIDTH, PADDLE_HEIGHT);
  rect(width - PADDLE_OFFSET, rightPaddle, PADDLE_WIDTH, PADDLE_HEIGHT);

  noStroke();
  fill(r,g,b,a);
  ellipse(x, y, BALL_DIAMETER);
  ellipse(x2,y2,BALL_DIAMETER2)

  x += vx;
  y += vy;
  x2 +=vx2;
  y +=vy2;
  
    if (y2 < 15) {
    vy2 = -vy2;
  }
  if (y2 > height - 15) {
    vy2 = -vy2;
  }

  if (x2 < PADDLE_OFFSET + PADDLE_WIDTH / 2 + BALL_DIAMETER2 / 2) {
    if (y2 < leftPaddle - PADDLE_HEIGHT / 2 ||
      y2 > leftPaddle + PADDLE_HEIGHT / 2) {
      gameOver("left");
    } else {
      vx2 = -vx2*1.5;
      scoreLeft =scoreLeft+1;
      
    }
  }
  if (x2 > width - (PADDLE_OFFSET + PADDLE_WIDTH / 2 + BALL_DIAMETER2 / 2)) {
    if (y2 < rightPaddle - PADDLE_HEIGHT / 2 ||
      y2 > rightPaddle + PADDLE_HEIGHT / 2) {
      gameOver("right");
    } else {
      vx2 = -vx2*2;
      scoreRight=scoreRight+1;
    }
  }

  if (y < 15) {
    vy = -vy;
  }
  if (y > height - 15) {
    vy = -vy;
  }

  if (x < PADDLE_OFFSET + PADDLE_WIDTH / 2 + BALL_DIAMETER / 2) {
    if (y < leftPaddle - PADDLE_HEIGHT / 2 ||
      y > leftPaddle + PADDLE_HEIGHT / 2) {
      gameOver("left");
    } else {
      vx = -vx*1.5;
      scoreLeft =scoreLeft+1;
      
    }
  }
  if (x > width - (PADDLE_OFFSET + PADDLE_WIDTH / 2 + BALL_DIAMETER / 2)) {
    if (y < rightPaddle - PADDLE_HEIGHT / 2 ||
      y > rightPaddle + PADDLE_HEIGHT / 2) {
      gameOver("right");
    } else {
      vx = -vx*2;
      scoreRight=scoreRight+1;
    }
  }
}
