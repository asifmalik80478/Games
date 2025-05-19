<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Sliding Puzzle Game</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <h1>Sliding Puzzle Game</h1>
  <div class="puzzle" id="puzzle"></div>
  <button onclick="shuffle()">Shuffle</button>

  <script src="script.js"></script>
</body>
</html>

const puzzle = document.getElementById("puzzle");

let tiles = [...Array(8).keys()].map(x => x + 1); // [1-8]
tiles.push(null); // last tile is empty

function drawPuzzle() {
  puzzle.innerHTML = '';
  tiles.forEach((val, i) => {
    const tile = document.createElement("div");
    tile.className = val ? "tile" : "tile empty";
    tile.textContent = val || "";
    tile.addEventListener("click", () => moveTile(i));
    puzzle.appendChild(tile);
  });
}

function moveTile(index) {
  const emptyIndex = tiles.indexOf(null);
  const validMoves = [index - 1, index + 1, index - 3, index + 3];

  if (validMoves.includes(emptyIndex) &&
      Math.abs(emptyIndex % 3 - index % 3) + Math.abs(Math.floor(emptyIndex / 3) - Math.floor(index / 3)) === 1) {
    [tiles[emptyIndex], tiles[index]] = [tiles[index], tiles[emptyIndex]];
    drawPuzzle();
  }
}

function shuffle() {
  for (let i = 0; i < 100; i++) {
    const possibleMoves = tiles
      .map((_, i) => i)
      .filter(i => {
        const empty = tiles.indexOf(null);
        const x = Math.abs(i % 3 - empty % 3);
        const y = Math.abs(Math.floor(i / 3) - Math.floor(empty / 3));
        return x + y === 1;
      });
    const move = possibleMoves[Math.floor(Math.random() * possibleMoves.length)];
    moveTile(move);
  }
}

drawPuzzle();
