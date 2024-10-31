let ball;
let leftPaddle;
let rightPaddle;

function setup() {
  createCanvas(800, 400);
  
  ball = new Ball();
  leftPaddle = new Paddle(true);
  rightPaddle = new Paddle(false);
}

function draw() {
  background(0);
  
  ball.update();
  ball.show();
  
  leftPaddle.show();
  rightPaddle.show();
  
  leftPaddle.update();
  rightPaddle.update();
  
  ball.checkPaddle(leftPaddle);
  ball.checkPaddle(rightPaddle);
}

class Ball {
  constructor() {
    this.reset();
  }
  
  reset() {
    this.x = width / 2;
    this.y = height / 2;
    this.xSpeed = random([-5, 5]);
    this.ySpeed = random(-3, 3);
  }
  
  update() {
    this.x += this.xSpeed;
    this.y += this.ySpeed;
    
    // Verifica colisão com as bordas
    if (this.y < 0 || this.y > height) {
      this.ySpeed *= -1;
    }
    
    // Reinicia a bola se sair da tela
    if (this.x < 0 || this.x > width) {
      this.reset();
    }
  }
  
  show() {
    fill(255);
    ellipse(this.x, this.y, 20);
  }
  
  checkPaddle(paddle) {
    if (this.x - 10 < paddle.x + paddle.width && this.x + 10 > paddle.x && this.y > paddle.y && this.y < paddle.y + paddle.height) {
      this.xSpeed *= -1;
      this.x += this.xSpeed; // Move a bola para fora do paddle
    }
  }
}

class Paddle {
  constructor(isLeft) {
    this.width = 10;
    this.height = 80;
    this.isLeft = isLeft;
    this.y = height / 2 - this.height / 2;
    this.x = isLeft ? 0 : width - this.width;
  }
  
  update() {
    if (this.isLeft) {
      if (keyIsDown(87)) { // W
        this.y -= 5;
      }
      if (keyIsDown(83)) { // S
        this.y += 5;
      }
    } else {
      if (keyIsDown(UP_ARROW)) {
        this.y -= 5;
      }
      if (keyIsDown(DOWN_ARROW)) {
        this.y += 5;
      }
    }
    
    // Limita o paddle dentro da tela
    this.y = constrain(this.y, 0, height - this.height);
  }
  
  show() {
    fill(255);
    rect(this.x, this.y, this.width, this.height);
  }
}
