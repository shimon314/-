<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>Neon Memory Game</title>
<style>
  body {
    margin: 0;
    font-family: "Segoe UI", sans-serif;
    background: radial-gradient(circle at top, #111, #000);
    color: #fff;
    text-align: center;
  }

  h1 {
    margin-top: 20px;
    letter-spacing: 2px;
    text-shadow: 0 0 10px #0ff;
  }

  #menu {
    margin: 20px;
  }

  select, button {
    background: #111;
    color: #0ff;
    border: 1px solid #0ff;
    padding: 8px 12px;
    border-radius: 6px;
    font-size: 16px;
    cursor: pointer;
    box-shadow: 0 0 10px #0ff3;
  }

  #stats {
    margin-top: 10px;
    color: #0ff;
  }

  #game {
    display: grid;
    gap: 10px;
    justify-content: center;
    margin: 30px auto;
  }

  .card {
    background: linear-gradient(145deg, #222, #000);
    border-radius: 12px;
    font-size: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    user-select: none;
    box-shadow:
      inset 0 0 10px #000,
      0 0 10px #0ff4;
    transition: transform 0.2s, box-shadow 0.2s;
  }

  .card:hover {
    transform: scale(1.05);
    box-shadow: 0 0 20px #0ff;
  }

  .card.open {
    background: #fff;
    color: #000;
    cursor: default;
    transform: scale(1.05);
  }

  .card.matched {
    background: #0ff;
    color: #000;
    box-shadow: 0 0 25px #0ff;
    cursor: default;
  }
</style>
</head>
<body>

<h1>🧠 NEON MEMORY</h1>

<div id="menu">
  <select id="difficulty">
    <option value="2">EASY (2×2)</option>
    <option value="4" selected>NORMAL (4×4)</option>
    <option value="6">HARD (4×6)</option>
    <option value="36">VERY HARD (6×6)</option>
    <option value="64">INSANE (8×8)</option>
  </select>
  <button onclick="startGame()">START</button>
</div>

<div id="stats">手数: 0</div>
<div id="game"></div>

<script>
  const emojis = [
    "🍎","🍌","🍇","🍓","🍒","🍍","🥝","🍉",
    "🍋","🍑","🥭","🍐","🍈","🍏","🍊","🫐",
    "🥥","🥕","🌽","🍄","🍔","🍟","🍕","🌮",
    "🍩","🍪","🍰","🍫","🍿","🍣","🍤","🍙"
  ];

  let first, second, lock, moves, matched;
  const game = document.getElementById("game");
  const stats = document.getElementById("stats");

  function startGame() {
    game.innerHTML = "";
    first = second = null;
    lock = false;
    moves = 0;
    matched = 0;
    stats.textContent = "手数: 0";

    let size = document.getElementById("difficulty").value;
    let totalCards;

    if (size == 2) totalCards = 4;
    else if (size == 4) totalCards = 16;
    else if (size == 6) totalCards = 24;
    else if (size == 36) totalCards = 36;
    else totalCards = 64;

    let cols = Math.sqrt(totalCards);
    game.style.gridTemplateColumns = `repeat(${cols}, 70px)`;

    let selected = emojis.slice(0, totalCards / 2);
    let cards = [...selected, ...selected].sort(() => Math.random() - 0.5);

    cards.forEach(emoji => {
      const card = document.createElement("div");
      card.className = "card";
      card.textContent = "❓";
      card.dataset.emoji = emoji;
      card.style.width = card.style.height = "70px";

      card.onclick = () => {
        if (lock || card.classList.contains("open")) return;

        card.textContent = emoji;
        card.classList.add("open");

        if (!first) {
          first = card;
        } else {
          second = card;
          lock = true;
          moves++;
          stats.textContent = `手数: ${moves}`;

          if (first.dataset.emoji === second.dataset.emoji) {
            first.classList.add("matched");
            second.classList.add("matched");
            matched += 2;
            reset();

            if (matched === totalCards) {
              stats.textContent += " 🎉 CLEAR!";
            }
          } else {
            setTimeout(() => {
              first.textContent = second.textContent = "❓";
              first.classList.remove("open");
              second.classList.remove("open");
              reset();
            }, 700);
          }
        }
      };

      game.appendChild(card);
    });
  }

  function reset() {
    first = second = null;
    lock = false;
  }

  startGame();
</script>

</body>
</html>
