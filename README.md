let creature;
let stars = [];
let obstacles = [];
let score = 0;

function setup() {
  createCanvas(600, 400);
  creature = new Creature();
  
  // Criar estrelas
  for (let i = 0; i < 10; i++) {
    stars.push(new Star(random(width), random(height)));
  }
  
  // Criar obstáculos
  for (let i = 0; i < 5; i++) {
    obstacles.push(new Obstacle(random(width), random(height), random(20, 50)));
  }
}

function draw() {
  background(0);
  
  // Mostrar e mover a criatura
  creature.show();
  creature.move();
  
  // Mostrar estrelas
  for (let star of stars) {
    star.show();
    if (creature.hits(star)) {
      score++;
      stars.splice(stars.indexOf(star), 1);
      stars.push(new Star(random(width), random(height)));
    }
  }

  // Mostrar obstáculos
  for (let obstacle of obstacles) {
    obstacle.show();
    if (creature.hits(obstacle)) {
      noLoop(); // Para o jogo se a criatura colidir com um obstáculo
      textSize(32);
      fill(255);
      text("Game Over!", width / 2 - 80, height / 2);
    }
  }

  // Mostrar pontuação
  fill(255);
  textSize(24);
  text("Pontuação: " + score, 10, 30);
}

class Creature {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
    this.size = 20;
    this.speed = 5;
  }

  show() {
    fill(0, 255, 0);
    ellipse(this.x, this.y, this.size);
  }

  move() {
    if (keyIsDown(LEFT_ARROW)) {
      this.x -= this.speed;
    }
    if (keyIsDown(RIGHT_ARROW)) {
      this.x += this.speed;
    }
    if (keyIsDown(UP_ARROW)) {
      this.y -= this.speed;
    }
    if (keyIsDown(DOWN_ARROW)) {
      this.y += this.speed;
    }

    // Limitar movimento dentro da tela
    this.x = constrain(this.x, this.size / 2, width - this.size / 2);
    this.y = constrain(this.y, this.size / 2, height - this.size / 2);
  }

  hits(object) {
    let d = dist(this.x, this.y, object.x, object.y);
    return d < (this.size / 2 + object.size / 2);
  }
}

class Star {
   constructor(x, y) {
     this.x = x;
     this.y = y;
     this.size = random(10,20);
   }

   show() {
     fill(255,255,0);
     ellipse(this.x, this.y, this.size);
   }
}

class Obstacle {
   constructor(x, y, size) {
     this.x = x;
     this.y = y;
     this.size = size;
   }

   show() {
     fill(255,0,0);
     rect(this.x,this.y,this.size,this.size)
   }
}
