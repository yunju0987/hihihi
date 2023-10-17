# hihihi

let balls = [];
let walls = [];
let wallThickness = 10;

function setup() {
  createCanvas(400, 400);
  noStroke();
  createWalls();
}

function draw() {
  background(220, 162, 220); 
  //우주를 표현하기 위해 배경 색상을 보라색으로 바꾸어주었습니다
  moveAndDisplayBalls();
}

function createWalls() {
  walls.push({ x: 0, y: 0, w: width, h: wallThickness });
  walls.push({ x: 0, y: height - wallThickness, w: width, h: wallThickness });
  walls.push({ x: 0, y: 0, w: wallThickness, h: height });
  walls.push({ x: width - wallThickness, y: 0, w: wallThickness, h: height }); 
}
//캔버스 외곽에 사각형 벽을 둔 뒤에 튕기는 식으로 표현하였습니다.

function moveAndDisplayBalls() {
  for (let i = balls.length - 1; i >= 0; i--) {
    let ball = balls[i];
    ball.move();
    ball.display();
    if (ball.isOutsideCanvas()) {
      balls.splice(i, 1);
    }
    for (let wall of walls) {
      ball.checkCollisionWithWall(wall);
    }
  }
}
//챗지피티가 도와준 이 함수는moveAndDisplayBalls() 함수인데요, 움직이는 공을 업데이트하고 화면에 표시하는 역할을 한다고 합니다.


function mousePressed() {
  let speedMultiplier = random(1, 6); 
  let ball = new MovingBall(mouseX, mouseY, speedMultiplier);
  balls.push(ball);
}
// 클릭하는 장소에서 1배 (지구)와 6배(달)의 속도로 움직이는 구체를 생성해주었습니다.

class MovingBall {
  constructor(x, y, speedMultiplier) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D().mult(2 * speedMultiplier);
    this.radius = 10;
  }

  move() {
    this.position.add(this.velocity);
  }

  display() {
    fill(0); //다 만들고나니 타로 버블티가 생각나서 검은색으로 설정해줬습니다..ㅎ
    ellipse(this.position.x, this.position.y, this.radius * 2);
  }

//isOutsideCanvas 함수와 checkCollisionWithWall함수는 PDF 파일에 정리 해두었습니다.
  isOutsideCanvas() {
    return (
      this.position.x < -this.radius ||
      this.position.x > width + this.radius ||
      this.position.y < -this.radius ||
      this.position.y > height + this.radius
    );
  }

  checkCollisionWithWall(wall) {
  const closestX = constrain(this.position.x, wall.x, wall.x + wall.w);
  const closestY = constrain(this.position.y, wall.y, wall.y + wall.h);

  const distance = dist(this.position.x, this.position.y, closestX, closestY);

  if (distance < this.radius) {
   
    this.velocity.mult(-1);
  }
}
}
