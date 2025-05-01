

        let player = {
            x: 100,
            y: canvas.height - 150,
            width: 50,
            height: 50,
            speed: 5,
            dx: 0,
            dy: 0
        };

        const keys = {
            left: false,
            right: false,
            up: false
        };

        const obstacles = [];

        function updateGameArea() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            movePlayer();
            createObstacles();
            drawObstacles();
            requestAnimationFrame(updateGameArea);
        }

        function movePlayer() {
            if (keys.left) player.x -= player.speed;
            if (keys.right) player.x += player.speed;
            if (keys.up) player.y -= player.speed;
            else player.y += player.speed;
            
            // Borders check
            if (player.x < 0) player.x = 0;
            if (player.x + player.width > canvas.width) player.x = canvas.width - player.width;
            if (player.y < 0) player.y = 0;
            if (player.y + player.height > canvas.height) player.y = canvas.height - player.height;

            ctx.fillStyle = "blue";
            ctx.fillRect(player.x, player.y, player.width, player.height);
        }

        function createObstacles() {
            if (Math.random() < 0.01) {
                let obstacle = {
                    x: canvas.width,
                    y: canvas.height - 100,
                    width: 50,
                    height: 50,
                    speed: 3
                };
                obstacles.push(obstacle);
            }
        }

        function drawObstacles() {
            for (let i = 0; i < obstacles.length; i++) {
                obstacles[i].x -= obstacles[i].speed;
                ctx.fillStyle = "red";
                ctx.fillRect(obstacles[i].x, obstacles[i].y, obstacles[i].width, obstacles[i].height);
            }
            obstacles.filter(obstacle => obstacle.x + obstacle.width > 0);
        }

        // Event listeners for keyboard (PC)
        function keyDownHandler(e) {
            if (e.key === "ArrowRight") keys.right = true;
            if (e.key === "ArrowLeft") keys.left = true;
            if (e.key === "ArrowUp") keys.up = true;
        }

        function keyUpHandler(e) {
            if (e.key === "ArrowRight") keys.right = false;
            if (e.key === "ArrowLeft") keys.left = false;
            if (e.key === "ArrowUp") keys.up = false;
        }

        window.addEventListener('keydown', keyDownHandler, false);
        window.addEventListener('keyup', keyUpHandler, false);

        // Touch controls for mobile
        let touchStartX = 0;
        let touchStartY = 0;

        canvas.addEventListener('touchstart', function(e) {
            touchStartX = e.touches[0].clientX;
            touchStartY = e.touches[0].clientY;
        });

        canvas.addEventListener('touchmove', function(e) {
            const touchEndX = e.touches[0].clientX;
            const touchEndY = e.touches[0].clientY;

            if (touchEndX < touchStartX) {
                keys.left = true;
                keys.right = false;
            } else if (touchEndX > touchStartX) {
                keys.right = true;
                keys.left = false;
            }

            if (touchEndY < touchStartY) {
                keys.up = true;
            } else {
                keys.up = false;
            }

            e.preventDefault();
        });

        canvas.addEventListener('touchend', function() {
            keys.left = false;
            keys.right = false;
            keys.up = false;
        });

        updateGameArea();
    </script>
</body>
</html>
