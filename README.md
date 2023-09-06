# Spectacular-Of-Galaxy-Planet
let stars = [];
let planets = [];
let numStars = 20000;
let numPlanets = 7;

function setup() {
  createCanvas(1080, 1080, WEBGL);
  for (let i = 0; i < numStars; i++) {
    stars.push(new Star());
  }
  for (let i = 0; i < numPlanets; i++) {
    planets.push(new Planet(100 * i + 200, 0, 0.01 * (i + 1)));
  }
}

function draw() {
  background(0);
  
  // Draw Galaxy
  push();
  for (let i = 0; i < stars.length; i++) {
    stars[i].show();
    stars[i].update();
  }
  pop();
  
  // Draw Earth instead of Sun
  push();
  fill(0, 128, 255); // Earth's color, blue
  sphere(75);  // Size remains the same for this example
  pop();
  
  // Draw Planets
  for (let i = 0; i < planets.length; i++) {
    planets[i].update();
    planets[i].show();
  }
}

class Star {
  constructor() {
    this.x = random(-width, width);
    this.y = random(-height, height);
    this.z = random(width);
    this.size = random(0.2, 0.7);
  }

  update() {
    this.z = this.z - 4;
    if (this.z < 1) {
      this.z = width;
      this.x = random(-width, width);
      this.y = random(-height, height);
    }
  }

  show() {
    fill(255);
    noStroke();
    let sx = map(this.x / this.z, 0, 1, 0, width);
    let sy = map(this.y / this.z, 0, 1, 0, height);
    let sz = map(this.z, 0, width, 4, 0);
    ellipse(sx, sy, sz);
  }
}

class Planet {
  constructor(x, y, speed) {
    this.x = x;
    this.y = y;
    this.z = 0;
    this.angle = random(TWO_PI);
    this.speed = speed;
  }

  update() {
    this.angle += this.speed;
    this.x = cos(this.angle) * 300;
    this.y = sin(this.angle) * 300;
  }

  show() {
    push();
    translate(this.x, this.y, this.z);
    fill(64, 224, 208);
    sphere(30);
    pop();
  }

