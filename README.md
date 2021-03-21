# Uncle-Kua-s-Snake
my first practice work of Javascript
<!DOCTYPE html>
<html lang="en">
    
<head>
    <title>Uncle Kua's Snake</title>
    <meta charset="UTF-8">
    <meta name="description" content="貪食蛇testing_version1">
    <meta name="keywords" content="testing_version1">
    <!--<link rel="stylesheet" type="text/css" href="main.css"/>-->

    <button onclick="gameStart()">開始遊戲</button>
    <h1 id="score_id">Score:0</h1>
    <canvas width="400" height="400" id="canvas_id"></canvas>
    <body>
    <script type="text/javascript">

        var BLOCK_SIZE = 20
        var BLOCK_COUNT = 20

        var snake
        var apple
        var score
        var level
        var gameInterval

        function putApple() 
        {
            apple = {x: Math.floor(Math.random() * BLOCK_COUNT), y: Math.floor(Math.random() * BLOCK_COUNT)}
            for (var i=0; i<snake.body.length; i++)
            {
                if(snake.body[i].x === apple.x && snake.body[i].y === apple.y) 
                {
                    putApple()
                    break
                }
            }
        }   
    
        function updateScore(newsScore) 
        {
            score = newsScore
            document.getElementById('score_id').innerHTML = score
        }

        function updateGameLevel(newLevel)
        {
            level = newLevel
            if (gameInterval) 
            {
                clearInterval(gameInterval)
            }
            gameInterval = setInterval(gameRoutine, 1000) / (10 +level)
        }

        function gameStart() 
        {
            snake = 
            {
                body: [ { x: BLOCK_COUNT / 2, y: BLOCK_COUNT / 2 }], size: 5, direction: { x: 0, y: -1 }
            }
            putApple()
            updateScore(0)
            updateGameLevel(1)
        }

        function gameRoutine() 
        {
            moveSnake()
            if (snakeIsDead()) 
            {
                ggler()
                return
            }
            if (snake.body[0].x === apple.x && snake.body[0].y === apple.y) 
            {
                eatApple()
            }
            updateCanvas()
        }

        function eatApple() 
        {
            snake.size += 1
            putApple()
            updateScore(score + 1)
        }

        function snakeIsDead() 
        {
            // hit walls
            if (snake.body[0].x < 0) 
            {
                return true
            } 
            else if (snake.body[0].x >= BLOCK_COUNT) 
            {
                return true
            } 
            else if (snake.body[0].y < 0)
            {
                return true
            }
            else if (snake.body[0].y >= BLOCK_COUNT) 
            {
                return true
            }
            // hit body
            for (var i=1; i<snake.body.length; i++) 
            {
                if (snake.body[0].x === snake.body[i].x & snake.body[0].y === snake.body[i].y) 
                {
                    return true
                }
                return false
            }
        }

        function ggler() 
        {
            clearInterval(gameInterval)
        }

        function moveSnake() 
        {
            var newBlock = 
            {x: snake.body[0].x + snake.direction.x, y: snake.body[0].y + snake.direction.y}
            snake.body.unshift(newBlock)
            while (snake.body.length > snake.size) 
            {
                snake.body.pop()
            }
        }

        function updateCanvas() 
        {
            var canvas = document.getElementById('canvas_id')
            var context = canvas.getContext('2d')

            context.fillStyle = 'black'
            context.fillRect(0, 0, canvas.width, canvas.height)

            context.fillStyle = 'lime'
            for (var i=0; i<snake.body.length; i++) 
            {
                context.fillRect(snake.body[i].x * BLOCK_SIZE + 1, snake.body[i].y * BLOCK_SIZE + 1, BLOCK_SIZE - 1, BLOCK_SIZE - 1)
            }

            context.fillStyle = 'red'
            context.fillRect(
            apple.x * BLOCK_SIZE +1,
            apple.y * BLOCK_SIZE +1,
            BLOCK_SIZE -1,
            BLOCK_SIZE -1
            )
        }

        function onPageLoaded() 
        {
            document.addEventListener('keydown', handleKeyDown)
        }

        window.onload = onPageLoaded

        function handleKeyDown(event) 
        {
            var originX = snake.direction.x
            var originY = snake.direction.y

            if (event.keyCode === 37) 
            { // left arrow
                snake.direction.x = originY
                snake.direction.y = -originX
            } 
            else if (event.keyCode === 39) 
            { // right arrow
                snake.direction.x = -originY
                snake.direction.y = originX
            }
        }

    </script>
    </body>
</head>
</html>
