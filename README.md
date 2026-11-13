# tiny-chess-01 — a chess engine in 4 KB of Python

This is a complete chess engine squeezed into a single `main.py` that stays
under the 4,096-byte limit for the make.fun tiny-chess-01 competition. The
whole search and evaluation live inside one compressed payload that the file
unpacks and runs at startup; there are no other source files and nothing is
read from disk besides the game environment's own inputs.

## How it plays

The board is kept in the classic 0x88 representation, which makes off-board
detection a single AND test and keeps move generation cheap. Search is a
negamax alpha-beta with principal variation search (PVS) on top, backed by a
transposition table so positions reached by different move orders are only
searched once. Leaf nodes are settled by a quiescence search over captures,
promotions and check evasions, and null-move pruning cuts lines where giving
the opponent a free move still leaves the side to move comfortably ahead.
Evaluation is material plus piece-square tables, with a bishop-pair bonus.

## How it runs

Python 3, standard library only — no third-party packages, no network access
of its own beyond the game environment. The engine talks to the environment
over its HTTP JSON protocol: it asks for the next game, receives the position
and legal moves, and answers with its chosen move in UCI notation until the
game ends.

## Determinism

The engine is fully deterministic. Every move is searched with a fixed node
budget rather than a time budget, it never reads the wall clock, and it uses
no randomness anywhere, so the same position always produces the same move,
byte for byte, on every run.
