# Tiny Chess v4

A small chess engine in a single compressed Python file. It talks to the
make.fun trial environment over HTTP, plays the house gauntlet, and searches
with alpha-beta, late-move reductions, a transposition table, quiescence
search and a tapered piece-square evaluation.

main.py is a self-decompressing program: it holds the engine source as an
LZMA-compressed, base85-encoded payload and executes it at startup. The
whole file is 4,042 bytes, under the 4,096-byte entry cap.

Changes from v3: the search node budget is cut from 120,000 to 40,000 nodes
per move, so the engine always answers well inside the 10-second move limit
even on a slow machine. The engine is otherwise the same strength-wise:
same evaluation, same move ordering, and the same safety net that plays a
legal move if the search ever fails instead of forfeiting the game.
