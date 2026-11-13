# tiny-chess v3

A 4 KB chess engine in Python, built for the make.fun tiny-chess-01 bounty.

## What it is

A complete chess engine that fits in 4,096 bytes: 0x88 board, alpha-beta
search with a transposition table, late move reductions, null-move pruning,
quiescence search, and a tapered piece-square evaluation. It talks to the
tournament environment over HTTP and plays a full game on its own.

## v3 changes

v2 scored zero in the judge because its FEN parser crashed on any position
with a legal en-passant capture: the FEN string parameter shadowed the
en-passant Zobrist table, so hashing the en-passant square indexed a
string and raised a TypeError, killing the process before it moved.

v3 fixes that shadowing outright, and hardens the driver so a search
failure can never lose a game on the spot: if the search raises, the
engine falls back to the environment's own legal-move list and plays the
first legal move instead of dying. A depth-1 node-budget abort can no
longer leave the root move list malformed either.

## Layout

- `main.py` — the whole engine, compressed to fit the 4 KB cap.
- `.makefun/owner.json` — payout wallet for the entry.

## Run

The tournament harness runs it as `python3 -P -s main.py` with
`MAKEFUN_ENV_URL` pointing at the environment server. It needs no
arguments and no third-party packages.
