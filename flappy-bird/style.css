const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");
const scoreElement = document.getElementById("score");
const gameOverText = document.getElementById("game-over-text");

// Game Variables
let bird;
let pipes = [];
let frameCount = 0;
let score = 0;
let gameActive = true;

// Configuration Constants
const GRAVITY = 0.25;
const JUMP_STRENGTH = -5.5;
const PIPE_SPEED = 2;
const PIPE_SPAWN_RATE = 100; // Spawn pipe every 100 frames
const PIPE_GAP = 120;        // Vertical opening size between pipes
const PIPE_WIDTH = 60;

class Bird {
    constructor() {
        this.x = 50;
        this.y = 150;
        this.width = 34;
        this.height = 24;
        this.velocity = 0;
    }

    draw() {
        // Draw Bird Body
        ctx.fillStyle = "#facc15"; // Yellow
        ctx.fillRect(this.x, this.y, this.width, this.height);
        
        // Draw Bird Eye
        ctx.fillStyle = "#000";
        ctx.fillRect(this.x + 22, this.y + 4, 5, 5);

        // Draw Bird Beak
        ctx.fillStyle = "#f97316"; // Orange
        ctx.fillRect(this.x + this.width, this.y + 8, 8, 8);
    }

    update() {
        this.velocity += GRAVITY;
        this.y += this.velocity;

        // Prevent falling through floor
        if (this.y + this.height > canvas.height) {
            this.y = canvas.height - this.height;
            endGame();
        }
        // Prevent flying past ceiling
        if (this.y < 0) {
            this.y = 0;
            this.velocity = 0;
        }
    }

    jump() {
        this.velocity = JUMP_STRENGTH;
    }
}

class Pipe {
    constructor() {
        this.x = canvas.width;
        // Randomize the top pipe length
        this.topHeight = Math.floor(Math.random() * (canvas.height - PIPE_GAP - 100)) + 30;
        this.bottomY = this.topHeight + PIPE_GAP;
        this.bottomHeight = canvas.height - this.bottomY;
        this.passed = false;
    }

    draw() {
        ctx.fillStyle = "#22c55e"; // Green pipes
        
        // Top Pipe
        ctx.fillRect(this.x, 0, PIPE_WIDTH, this.topHeight);
        ctx.strokeStyle = "#15803d";
        ctx.lineWidth = 3;
        ctx.strokeRect(this.x, 0, PIPE_WIDTH, this.topHeight);

        // Bottom Pipe
        ctx.fillRect(this.x, this.bottomY, PIPE_WIDTH, this.bottomHeight);
        ctx.strokeRect(this.x, this.bottomY, PIPE_WIDTH, this.bottomHeight);
    }

    update() {
        this.x -= PIPE_SPEED;
    }
}

// Initialize / Reset Game Settings
function init() {
    bird = new Bird();
    pipes = [];
    score = 0;
    frameCount = 0;
    gameActive = true;
    scoreElement.textContent = score;
    gameOverText.classList.add("hidden");
    gameLoop();
}

function checkCollision(bird, pipe) {
    // Check horizontal overlap
    if (bird.x + bird.width > pipe.x && bird.x < pipe.x + PIPE_WIDTH) {
        // Check vertical overlap with top pipe or bottom pipe
        if (bird.y < pipe.topHeight || bird.y + bird.height > pipe.bottomY) {
            return true;
        }
    }
    return false;
}

function endGame() {
    gameActive = false;
    gameOverText.classList.remove("hidden");
}

// Primary Animation Loop
function gameLoop() {
    if (!gameActive) return;

    // Clear Canvas
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Handle Bird
    bird.update();
    bird.draw();

    // Spawn Pipes
    if (frameCount % PIPE_SPAWN_RATE === 0) {
        pipes.push(new Pipe());
    }

    // Process Pipes
    for (let i = pipes.length - 1; i >= 0; i--) {
        pipes[i].update();
        pipes[i].draw();

        // Collision logic
        if (checkCollision(bird, pipes[i])) {
            endGame();
        }

        // Scoring tracker
        if (!pipes[i].passed && bird.x > pipes[i].x + PIPE_WIDTH) {
            score++;
            scoreElement.textContent = score;
            pipes[i].passed = true;
        }

        // Garbage collection (remove offscreen pipes)
        if (pipes[i].x + PIPE_WIDTH < 0) {
            pipes.splice(i, 1);
        }
    }

    frameCount++;
    requestAnimationFrame(gameLoop);
}

// Event Listeners for Player Inputs
window.addEventListener("keydown", (e) => {
    if (e.code === "Space") {
        if (gameActive) {
            bird.jump();
        } else {
            init(); // Restart game if dead
        }
    }
});

// Click / Tap support for mouse/mobile users
window.addEventListener("touchstart", () => {
    if (gameActive) {
        bird.jump();
    } else {
        init();
    }
});

// Fire up the engine on load
init();
