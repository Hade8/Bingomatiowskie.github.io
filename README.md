<!doctype html>
<html lang="pl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>BINGO CS z Matim</title>
  <style>
    body {
      font-family: 'Orbitron', sans-serif;
      background-color: #0b0f1a;
      color: #e0e6f0;
      margin: 0;
      padding: 24px;
      display: flex;
      align-items: flex-start;
      justify-content: center;
      min-height: 100vh;
    }

    h1 {
      text-align: center;
      font-size: 36px;
      color: #00b4ff;
      text-shadow: 0 0 15px #0077ff;
      margin-bottom: 20px;
    }

    button {
      background: #0077ff;
      color: white;
      border: none;
      padding: 10px 16px;
      border-radius: 8px;
      font-weight: bold;
      cursor: pointer;
      transition: 0.2s;
    }

    button:hover {
      background: #0099ff;
      box-shadow: 0 0 10px #0099ff;
    }

    .wrap {
      width: min(760px, 95%);
    }

    .controls {
      display: flex;
      justify-content: center;
      gap: 10px;
      flex-wrap: wrap;
      margin-bottom: 15px;
    }

    .board {
      background: #111827;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 0 30px rgba(0, 183, 255, 0.15);
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 8px;
    }

    .cell {
      height: 80px;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 8px;
      background: #1e293b;
      border: 1px solid #334155;
      color: #cbd5e1;
      font-size: 13px;
      text-align: center;
      padding: 6px;
      cursor: pointer;
      position: relative;
      transition: 0.2s;
      user-select: none;
    }

    .cell:hover {
      background: #1f3b56;
    }

    .cell.marked::after {
      content: '✖';
      position: absolute;
      color: #00b4ff;
      font-size: 40px;
      text-shadow: 0 0 10px #00b4ff;
    }

    footer {
      margin-top: 10px;
      text-align: center;
      color: #64748b;
      font-size: 13px;
    }
  </style>
</head>
<body>
  <div class="wrap">
    <h1>BINGO CS z Matim</h1>
    <div class="controls">
      <button id="drawBtn">Losuj nową planszę</button>
    </div>

    <div class="board">
      <div class="grid" id="grid"></div>
    </div>

    <footer>
    </footer>
  </div>

  <script>
    const WORDS = [
      'Polak w teamie', 'Rusek w teamie', 'Mati wyzywa teammeatow', 'Ktos cheatuje', 'Mati jest wyzywany',
      'Granie deaglem', 'Mati chleje i gra', 'Sprawdzanie profili', 'Essa', 'Mati gra jakby miał wyjebane',
      'Mati otwiera tabele', 'Mati zamyka tabele', 'Jebie mnie ta gra', 'Dzieciak/dziecko', 'X kurwa D',
      'Mati gra awp', 'Mati mutuje na dc', 'Co mój team robi', 'Ile on ma godzin', 'Profil prywatny',
      'Zabijanie teameatow', 'Mati się poci', 'Mute teameatow', 'Sprawdzanie co chwile taba', 'Rage',
      'O co chodzi', 'Strzelanie do teammeatow', 'Mati ma kamerkę', 'Ale mam fps dużo', 'Skąd on się wziął', 'Idę na mida', 'Zabij się', 'pizde klej', 'mati dostaje onetapa', 'mati wali onetapa', 'ktoś mi wytłumaczy', 'mati wychodzi', 'takich jak ty to się kurwa wiesza', 'pizde klej', 'z tobą nie gram', 'wyzywanie przeciwnika na czacie', 
    ];

    const gridEl = document.getElementById('grid');
    const drawBtn = document.getElementById('drawBtn');
    const copyBtn = document.getElementById('copyBtn');

    function shuffle(array) {
      const a = array.slice();
      for (let i = a.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [a[i], a[j]] = [a[j], a[i]];
      }
      return a;
    }

    function buildBoard() {
      const source = WORDS.slice();
      while (source.length < 25) source.push(...WORDS);
      const chosen = shuffle(source).slice(0, 25);

      gridEl.innerHTML = '';
      for (let i = 0; i < 25; i++) {
        const div = document.createElement('div');
        div.className = 'cell';
        if (i === 12) div.classList.add('center');
        div.textContent = chosen[i];
        div.addEventListener('click', () => {
          div.classList.toggle('marked');
        });
        gridEl.appendChild(div);
      }
    }

    buildBoard();
    drawBtn.addEventListener('click', buildBoard);

    copyBtn.addEventListener('click', () => {
      const cells = Array.from(gridEl.children).map(c => c.textContent.trim());
      const rows = [];
      for (let r = 0; r < 5; r++) rows.push(cells.slice(r * 5, (r + 1) * 5).join(' | '));
      const text = rows.join('\n');
      navigator.clipboard.writeText(text).then(() => alert('Skopiowano do schowka!'));
    });
  </script>
</body>
</html>
