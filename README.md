
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kamondan O'q Otish O'yini</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background: #87CEEB;
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
        
        let bow = { x: 100, y: canvas.height / 2, angle: 0 };
        let arrows = [];
        let target = { x: canvas.width - 150, y: Math.random() * (canvas.height - 100) + 50, radius: 50 };
        let score = 0;
        
        function drawBow() {
            ctx.fillStyle = "brown";
            ctx.fillRect(bow.x - 10, bow.y - 30, 10, 60);
            ctx.beginPath();
            ctx.arc(bow.x, bow.y, 30, Math.PI / 2, -Math.PI / 2);
            ctx.strokeStyle = "black";
            ctx.lineWidth = 3;
            ctx.stroke();
        }
        
        function drawArrow(arrow) {
            ctx.fillStyle = "black";
            ctx.fillRect(arrow.x, arrow.y, 20, 5);
        }
        
        function drawTarget() {
            ctx.beginPath();
            ctx.arc(target.x, target.y, target.radius, 0, Math.PI * 2);
            ctx.fillStyle = "red";
            ctx.fill();
            ctx.strokeStyle = "black";
            ctx.lineWidth = 3;
            ctx.stroke();
        }
        
        function drawScore() {
            ctx.fillStyle = "black";
            ctx.font = "20px Arial";
            ctx.fillText("Ochko: " + score, 20, 30);
        }
        
        function update() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawBow();
            drawTarget();
            drawScore();
            
            arrows.forEach((arrow, index) => {
                arrow.x += 10;
                if (arrow.x > canvas.width) {
                    arrows.splice(index, 1);
                }
                if (Math.hypot(arrow.x - target.x, arrow.y - target.y) < target.radius) {
                    score += 10;
                    target.y = Math.random() * (canvas.height - 100) + 50;
                    arrows.splice(index, 1);
                }
                drawArrow(arrow);
            });
            
            requestAnimationFrame(update);
        }
        
        window.addEventListener("click", () => {
            arrows.push({ x: bow.x + 20, y: bow.y });
        });
        
        window.addEventListener("mousemove", (event) => {
            bow.y = event.clientY;
        });
        
        update();
    </script>
</body>
</html>
