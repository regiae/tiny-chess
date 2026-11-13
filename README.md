# Tiny Chess v6

A 4 KB chess engine in Python, written for the make.fun tiny-chess
bounty. It plays through the competition gateway: it fetches the
current game, searches with iterative deepening, and posts its move
back, one legal move at a time.

How it works: 0x88 board, Zobrist hashing with a transposition
table, null-move pruning, late-move reduction without re-search,
quiescence search on captures, and a tapered evaluation that blends
middlegame and endgame piece-square tables by game phase.

Version 6 keeps the strong v3 evaluation and search, but runs at a
40,000-node budget per move instead of 120,000, so it stays well
inside the judge's time limit on a slower machine. A safety net in
the move loop falls back to the first legal move if the search ever
fails, so a single bad position can never forfeit a game.

Run it with MAKEFUN_ENV_URL set to the gateway URL.
