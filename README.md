# tiny-chess v7 — 4KB chess engine

A tiny chess engine in 4 KB of Python, built for the make.fun
tiny-chess-01 bounty: the strongest engine that fits in 4,096 bytes.

## What it is

- One file, `main.py` (4,069 bytes): an LZMA + base85 self-decompressing
  blob. It unpacks at import time and plays chess against the kit's
  environment server over HTTP.
- Search: negamax with alpha-beta, LMR without re-search, tapered
  evaluation, Zobrist hashing, en-passant table, and a safety net so a
  search that overruns its node budget still returns a move.
- Node budget NL = 60,000 (v7 raises v6's 40,000 to buy strength toward
  the 1360 Elo prize gate; v3 ran 120,000 and timed out at the judge).

## Evaluation

Piece-square tables and material/phase values tuned for this engine
(64-square tables, separate middlegame/endgame king tables).

## Run it

The engine talks to the make.fun trial environment:

```
MAKEFUN_ENV_URL=http://127.0.0.1:8089 python3 main.py
```

## Files

- `main.py` — the entrypoint (the whole engine, compressed)
- `README.md` — this file
- `LICENSE` — MIT
- `.makefun/owner.json` — entry wallet
