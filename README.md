# Tiny Chess v2

A complete chess engine squeezed into less than 4 KB of Python (standard
library only). It plays a full game: legal move generation, search, and
evaluation, with no external dependencies and no model calls.

## Layout

- `main.py` is the whole program, 4,073 bytes. It is a small loader: the
  engine source is LZMA-compressed and base85-encoded into two string
  pieces, which are joined, decoded, decompressed, and executed at
  startup. Keeping the payload in two pieces keeps the file inside the
  4,096-byte limit while leaving the loader readable.

## How the engine thinks

The board uses the 0x88 representation, with Zobrist hashing and a
transposition table. The search is negamax with alpha-beta pruning and
iterative deepening, plus quiescence search at the leaves so captures are
resolved before a position is judged. Moves are ordered so the most
promising ones are tried first, and late move reductions trim the search
on quiet moves that come late in the ordering. Lines that give check get
a small depth extension, and the whole search runs under a fixed node
budget so it always answers in time.

Evaluation is tapered: piece values and the king table are interpolated
between middlegame and endgame weights based on how much material is
left on the board, and a bonus is given for holding both bishops. The
result is a single score from the side to move's point of view.

## Correctness

Move generation was checked with perft on the six standard test
positions at depth 3, and all six counts match the known values.

## Local trial result

In local 80-game matches at 8,000 nodes per move against the previous
version of this engine, this version scored 25 wins, 42 draws, and 13
losses, about +53 Elo.

## Running it

The engine talks to the competition environment over HTTP:

```
MAKEFUN_ENV_URL=http://127.0.0.1:18081 python3 main.py
```

It needs nothing beyond Python 3 and its standard library.
