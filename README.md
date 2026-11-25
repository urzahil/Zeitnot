# ZEITNOT ⏱️♞

**Bullet-speed chess tactics, with puzzles made on your own device by a chess engine running in the page.**

▶ **Play:** https://urzahil.github.io/Zeitnot/

*Zeitnot* is German for "time trouble", the chess player's oldest enemy. You get one position, exactly one winning move, and a clock that's running out. Find the move.

---

## Modes

| Mode | Clock | Rules |
|---|---|---|
| **Bullet rush** `1\|+2` | 60 seconds | +2s per solve, −5s per miss |
| **Blitz rush** `3\|0` | 3 minutes | no increment, −5s per miss |
| **Zen** `∞` | none | retry misses, reveal the answer, watch the engine's full line |

Puzzles get harder as a rush goes on. At the end you get accuracy, average and fastest solve time, best streak, a bar chart of your solve time for each puzzle, how you did on each motif (fork, mate, sacrifice, discovered attack…), and a board for every miss with the correct move drawn on it.

## What's under the hood

Everything is in one `index.html`, with no libraries, no build step, no server and no tracking.

- **Engine written from scratch:** a `0x88` board representation with full legal move generation (castling, en passant, promotions). It's checked against standard perft counts (start position, Kiwipete and others).
- **Search:** negamax alpha-beta with iterative deepening, quiescence search, check extensions, and MVV-LVA plus killer-move ordering. Runs at about 1M nodes/s in a modern browser.
- **Live puzzle mining:** up to 4 Web Workers (one per spare CPU core) play quick, slightly sloppy games against themselves and test every position. A position becomes a puzzle only if:
  1. material is roughly level,
  2. no simple capture already wins (so no "take the hanging piece"),
  3. exactly one move wins at least 3 pawns or forces mate, and every other move stays roughly level.
- **Starter bank:** 150 puzzles mined the same way at build time, then checked one ply deeper, so you can play straight away. Fresh puzzles from your device are marked ⚡ and go to the front of the queue.
- **Motif tagging:** each solution is classified as fork, discovered attack, sacrifice, check, promotion, capture, quiet move or mate in N.
- Mining pauses when the queue holds 40 puzzles or the tab is hidden, to save battery.

The engine panel on the home screen shows it all working live: nodes per second, positions searched, and puzzles mined this visit and in total on this device.

## Run locally

```bash
git clone https://github.com/urzahil/Zeitnot.git
cd Zeitnot
python3 -m http.server 8000
# open http://localhost:8000
```

Opening `index.html` directly from disk also works in most browsers.

## Privacy

Nothing leaves your device. Personal bests and the count of puzzles you've mined are kept in your browser's `localStorage`.

## Deploy

GitHub Pages serves this repo from `main` at the root. Push a change to `index.html` and it's live in about a minute.
