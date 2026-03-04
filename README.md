# Survival-Game
its a survival game where you only need to play safe as long as you can
<!DOCTYPE html>
<html>
<head>
    <title>Dark Room Survival</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background: black;
        }
        canvas {
            display: block;
        }
    </style>
</head>
<body>

<canvas id="gameCanvas"></canvas>

<script>
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

let player = {
    x: canvas.width / 2,
    y: canvas.height / 2,
    size: 20,
    speed: 5
};

let monster = {
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    size: 30,
    speed: 1.5
};

let keys = {};
let startTime = Date.now();

document.addEventListener("keydown", (e) => keys[e.key] = true);
document.addEventListener("keyup", (e) => keys[e.key] = false);

function update() {

    // Player movement
    if (keys["w"]) player.y -= player.speed;
    if (keys["s"]) player.y += player.speed;
    if (keys["a"]) player.x -= player.speed;
    if (keys["d"]) player.x += player.speed;

    // Monster chase AI
    if (monster.x < player.x) monster.x += monster.speed;
    if (monster.x > player.x) monster.x -= monster.speed;
    if (monster.y < player.y) monster.y += monster.speed;
    if (monster.y > player.y) monster.y -= monster.speed;

    // Collision detection
    let dx = player.x - monster.x;
    let dy = player.y - monster.y;
    let distance = Math.sqrt(dx*dx + dy*dy);

    if (distance < player.size + monster.size) {
        alert("YOU WERE CAUGHT...");
        location.reload();
    }
}

function draw() {
    ctx.fillStyle = "black";
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    // Draw player
    ctx.fillStyle = "white";
    ctx.beginPath();
    ctx.arc(player.x, player.y, player.size, 0, Math.PI * 2);
    ctx.fill();

    // Draw monster
    ctx.fillStyle = "red";
    ctx.beginPath();
    ctx.arc(monster.x, monster.y, monster.size, 0, Math.PI * 2);
    ctx.fill();

    // Survival timer
    ctx.fillStyle = "white";
    ctx.font = "20px Arial";
    let survival = Math.floor((Date.now() - startTime) / 1000);
    ctx.fillText("Survival Time: " + survival, 20, 30);
}

function gameLoop() {
    update();
    draw();
    requestAnimationFrame(gameLoop);
}

gameLoop();
</script>

</body>
</html>
