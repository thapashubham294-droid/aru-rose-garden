# aru-rose-garden

html, body {
  margin: 0;
  padding: 0;
  height: 100%;
  overflow: hidden;
  background: radial-gradient(circle at top, #2b0018, #000);
  font-family: 'Segoe UI', sans-serif;
}

canvas { display: block; }

.overlay {
  position: fixed;
  top: 0;
  width: 100%;
  text-align: center;
  z-index: 10;
  padding-top: 25px;
}

.overlay h1 {
  color: #ffb6c1;
  font-size: 2.4rem;
  text-shadow: 0 0 12px rgba(255,182,193,0.9);
}

.msg {
  color: #ffd6e8;
  font-size: 1.1rem;
}

button {
  margin-top: 12px;
  padding: 10px 22px;
  font-size: 1rem;
  border-radius: 30px;
  border: none;
  cursor: pointer;
  background: #ff4d6d;
  color: white;
  box-shadow: 0 0 15px rgba(255,77,109,0.8);
}


<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Aru💓🧿</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

<div class="overlay">
  <h1>Aru💓🧿</h1>
  <p class="msg">Forever yours… in every universe 🌙💞</p>
  <button id="playBtn">Tap to feel the love 💓</button>
</div>

<canvas id="roseCanvas"></canvas>

<audio id="bgMusic" loop>
  <source src="assets/wanna_be_yours.mp3" type="audio/mpeg">
</audio>

<script src="script.js"></script>
</body>
</html>


const canvas = document.getElementById("roseCanvas");
const ctx = canvas.getContext("2d");
const music = document.getElementById("bgMusic");
const playBtn = document.getElementById("playBtn");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

const roseColors = ["red","pink","white","yellow","purple"];
const roseImages = [];
const roses = [];

roseColors.forEach(c => {
  const img = new Image();
  img.src = `assets/rose_${c}.png`;
  roseImages.push(img);
});

class Rose {
  constructor(x=null,y=null){
    this.x = x ?? Math.random()*canvas.width;
    this.y = y ?? -Math.random()*canvas.height;
    this.size = 25+Math.random()*45;
    this.speed = 0.5+Math.random()*1.6;
    this.rot = Math.random()*Math.PI;
    this.spin = (Math.random()-0.5)*0.01;
    this.img = roseImages[Math.floor(Math.random()*roseImages.length)];
  }
  update(){
    this.y += this.speed;
    this.rot += this.spin;
    if(this.y > canvas.height+this.size){
      this.y = -50;
      this.x = Math.random()*canvas.width;
    }
  }
  draw(){
    ctx.save();
    ctx.translate(this.x,this.y);
    ctx.rotate(this.rot);
    ctx.shadowColor="rgba(255,182,193,0.7)";
    ctx.shadowBlur=15;
    ctx.drawImage(this.img,-this.size/2,-this.size/2,this.size,this.size);
    ctx.restore();
  }
}

for(let i=0;i<45;i++) roses.push(new Rose());

function animate(){
  ctx.clearRect(0,0,canvas.width,canvas.height);
  roses.forEach(r=>{r.update();r.draw();});
  requestAnimationFrame(animate);
}
animate();

canvas.addEventListener("click",e=>{
  for(let i=0;i<6;i++) roses.push(new Rose(e.clientX,e.clientY));
});

playBtn.onclick=()=>{
  music.volume=0.5;
  music.play();
  playBtn.innerText="Love is playing 💞";
};
