<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>神経衰弱</title>
<style>
  body {
    font-family: sans-serif;
    background: #222;
    color: white;
    text-align: center;
  }

  #game {
    display: grid;
    grid-template-columns: repeat(4, 80px);
    gap: 10px;
    justify-content: center;
    margin-top: 30px;
  }

  .card {
    width: 80px;
    height: 80px;
    background: #555;
    font-size: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    border-radius: 10px;
    user-select: none;
  }

  .card.open {
    background: #fff;
    color: black;
    cursor: default;
  }

  .card.matched {
    background: #2ecc71;
    color: black;
    cursor: default;
  }
</style>
</head>
<body>

<h1>🧠 神経衰弱</h1>
<p id="status">カードをめくれ</p>

<div id="game"></div>

<script>
  const emojis = ["🍎","🍌","🍇","🍓","🍒","🍍","🥝","🍉"];
  let cards = [...emojis, ...emojis];
  cards.sort(() => Math.random() - 0.5);

  const game = document.getElementById("game");
  const status = document.getElementById("status");

  let first = null;
  let second = null;
  let lock = false;
  let matchedCount = 0;

  cards.forEach(emoji => {
    const card = document.createElement("div");
    card.className = "card";
    card.dataset.emoji = emoji;
    card.textContent = "❓";

    card.onclick = () => {
      if (lock || card.classList.contains("open")) return;

      card.textContent = emoji;
      card.classList.add("open");

      if (!first) {
        first = card;
      } else {
        second = card;
        lock = true;

        if (first.dataset.emoji === second.dataset.emoji) {
          first.classList.add("matched");
          second.classList.add("matched");
          matchedCount += 2;
          reset();

          if (matchedCount === cards.length) {
            status.textContent = "🎉 クリア！";
          }
        } else {
          setTimeout(() => {
            first.textContent = "❓";
            second.textContent = "❓";
            first.classList.remove("open");
            second.classList.remove("open");
            reset();
          }, 800);
        }
      }
    };

    game.appendChild(card);
  });

  function reset() {
    first = null;
    second = null;
    lock = false;
  }
</script>

</body>
</html>
