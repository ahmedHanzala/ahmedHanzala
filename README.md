<!DOCTYPE html>
<html>
  <head>
    <title>Game of Life with Hanzala</title>
    <style>
      body {
        margin: 0;
        overflow: hidden;
      }

      canvas {
        position: absolute;
      }

      #heading {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        font-size: 32px;
        font-weight: bold;
        color: white;
        text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.8);
      }
    </style>
  </head>
  <body>
    <canvas id="game-of-life"></canvas>
    <h1 id="heading">My name is Hanzala</h1>

    <script>
      const canvas = document.getElementById('game-of-life');
      const ctx = canvas.getContext('2d');
      const width = (canvas.width = window.innerWidth);
      const height = (canvas.height = window.innerHeight);

      const resolution = 10;
      const cols = Math.floor(width / resolution);
      const rows = Math.floor(height / resolution);

      function buildGrid() {
        return new Array(cols)
          .fill(null)
          .map(() =>
            new Array(rows).fill(null).map(() => Math.floor(Math.random() * 2))
          );
      }

      let grid = buildGrid();

      requestAnimationFrame(update);

      function update() {
        grid = nextGeneration(grid);
        render(grid);
        requestAnimationFrame(update);
      }

      function nextGeneration(grid) {
        const nextGen = grid.map((arr) => [...arr]);

        for (let i = 0; i < cols; i++) {
          for (let j = 0; j < rows; j++) {
            const cell = grid[i][j];
            let neighbors = 0;

            for (let x = -1; x < 2; x++) {
              for (let y = -1; y < 2; y++) {
                if (x === 0 && y === 0) continue;
                const neighborX = i + x;
                const neighborY = j + y;

                if (
                  neighborX >= 0 &&
                  neighborY >= 0 &&
                  neighborX < cols &&
                  neighborY < rows
                ) {
                  neighbors += grid[neighborX][neighborY];
                }
              }
            }

            if (cell === 1 && (neighbors < 2 || neighbors > 3)) {
              nextGen[i][j] = 0;
            } else if (cell === 0 && neighbors === 3) {
              nextGen[i][j] = 1;
            }
          }
        }

        return nextGen;
      }

      function render(grid) {
        ctx.clearRect(0, 0, width, height);

        for (let i = 0; i < cols; i++) {
          for (let j = 0; j < rows; j++) {
            const cell = grid[i][j];
            ctx.beginPath();
            ctx.rect(i * resolution, j * resolution, resolution, resolution);
            ctx.fillStyle = cell ? '#ffffff' : '#000000';
            ctx.fill();
            ctx.stroke();
          }
        }
      }

      window.addEventListener('resize', () => {
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
      });
    </script>
  </body>
</html>
