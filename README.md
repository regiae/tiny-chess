# Tiny Chess Engine

A tiny but strong chess engine packed into one 4,085-byte Python file.

## How it searches

The board is a 0x88 array with Zobrist hashing and a transposition
table. Search is negamax with alpha-beta pruning and iterative
deepening to a fixed budget of 120,000 nodes per move, roughly two to
three seconds. Quiescence search covers captures and promotions, and
every evasion while the king is in check.

Move ordering tries the transposition-table move first, then MVV-LVA
captures, killer moves, and a history heuristic. Null-move pruning
runs with R=2, mate scores are adjusted inside the transposition
table, and the root search uses a narrowing alpha-beta window.

## How it evaluates

Material plus CPW piece-square tables, a bishop-pair bonus, and an
endgame king table. Insufficient-material draws are recognised, and
every rule is handled: check, checkmate, stalemate, castling, en
passant, and promotion to any of the four pieces.

## Verification

Perft matches python-chess on all six standard test positions at
depth 3, including 8,902 nodes from the start position. In a practice
trial the engine scored Elo 1396, above the sunfish baseline of 1360.

## Files

- `main.py` — the whole engine, compressed into 4,085 bytes
- `README.md` — this file
- `LICENSE` — MIT license
