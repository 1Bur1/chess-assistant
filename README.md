# Chess Assistant

Chess Assistant is a chess analysis web page. You enter the moves of a game on the board, and it shows the best move for the current position, with an arrow on the board and a score.

The chess engine is **Stockfish 10**. It runs inside your browser, so no server, account or camera is needed.

**Live site:** https://1bur1.github.io/chess-assistant/

---

## Features

- A board that starts in the normal starting position
- Enter a move by tapping a piece, then the square it goes to. Dots show the legal moves.
- After every move, the best move is shown as:
  - its name, for example `Nf3`
  - an arrow on the board
  - a score and how deep the engine searched
- A score bar beside the board shows who is winning
- Buttons: **Play best move**, **Undo**, **Redo**, **Flip board**, **New game**
- A picker for pawn promotion (queen, rook, bishop or knight)
- Settings:
  - think time, from 0.3 to 6 seconds
  - show the best move for both sides, or only for you
  - load or copy a position as a FEN
- Works on phones and computers

## How to use it

1. Open the live site, or run it yourself (see below).
2. Choose **I play White** or **I play Black**. The board turns so your side is at the bottom.
3. Enter every move of the game, yours and your opponent's: tap the piece, then tap the square it goes to.
4. After each move, read the best move:
   - **gold arrow**: the best move for you
   - **orange arrow**: the best move for your opponent
5. If you enter a wrong move, press **Undo**.

### Reading the score

| Score | Meaning |
|---|---|
| `+1.00` | White is about one pawn ahead |
| `-0.50` | Black is about half a pawn ahead |
| `0.00` | The position is equal |
| `White mates in 3` | White can force checkmate in 3 moves |

### Starting from the middle of a game

Open **Settings**, paste a FEN (a line of text that describes a position) into the box, and press **Load FEN**.

## How to run it

### Option 1: Open the live site
Go to https://1bur1.github.io/chess-assistant/ on any phone or computer.

### Option 2: Run it on your computer
You need [Git](https://git-scm.com/) and [Python 3](https://www.python.org/).

```bash
git clone https://github.com/1Bur1/chess-assistant.git
cd chess-assistant
python3 -m http.server 8000
```

Then open **http://localhost:8000** in your browser. To stop the server, press `Ctrl + C`.

> Double-clicking `index.html` may not work, because browsers block the engine from loading from a local file. Use the server command above.

### Option 3: Put it on any web host
Upload these three files to any static host (GitHub Pages, Netlify, tiiny.host and others), and keep them in the same folder:

- `index.html`
- `chess.js`
- `stockfish.js`

### Turning on GitHub Pages (for the owner of this repository)
1. Go to **Settings → Pages**.
2. Under **Source**, choose **Deploy from a branch**.
3. Under **Branch**, choose **main** and **/ (root)**, then press **Save**.
4. After 1–2 minutes the site is live at https://1bur1.github.io/chess-assistant/

## Files

| File | What it is |
|---|---|
| `index.html` | The app: the board, the buttons, and the code that talks to the engine |
| `chess.js` | [chess.js 0.10.3](https://github.com/jhlywa/chess.js): chess rules, legal moves and FEN |
| `stockfish.js` | [stockfish.js 10.0.2](https://github.com/niklasf/stockfish.js): the Stockfish 10 engine, compiled to JavaScript |
| `LICENSE` | GNU GPL v3 license |

## How the best move is chosen

1. After every move, the page turns the board into a **FEN** (a line of text that describes the position).
2. It sends the FEN to Stockfish using the **UCI** protocol: `position fen …`, then `go movetime 1000` to think for one second.
3. Stockfish searches the tree of possible moves with **minimax and alpha-beta pruning**:
   - It assumes each side always plays its best move.
   - It skips branches that cannot change the result.
4. It searches one move deeper each round (**iterative deepening**), so it always has a best move ready.
5. It saves time in two ways:
   - It remembers positions it has already scored (**transposition table**).
   - It tries the most promising moves first (**move ordering**).
6. Before scoring a position, it plays out any captures that are still possible (**quiescence search**).
7. It then scores the position with an **evaluation function** that looks at:
   - material
   - piece placement
   - mobility
   - king safety
   - pawn structure
8. While it searches, Stockfish keeps reporting its best line, for example `info depth 12 score cp 35 pv e2e4 …`. The page shows the first move of that line right away and updates it as the search gets deeper.

More think time means a deeper search, which means a stronger move.

## Built with

- HTML, CSS and JavaScript (no frameworks)
- [Stockfish](https://stockfishchess.org/), an open-source chess engine
- [chess.js](https://github.com/jhlywa/chess.js), a library for chess rules

## License

Stockfish is licensed under the GNU GPL v3, so this project is released under the GPL v3 as well. chess.js is under the BSD 2-Clause license.
