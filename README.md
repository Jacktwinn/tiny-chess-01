# tiny-chess-01 entry

An original chess engine in 4,094 bytes of Python (main.py). The file is a
compact lzma payload plus a small unpacker; the decompressed source is a
self-contained engine (stdlib only, no model calls, fully deterministic).

Design: 0x88 board, negamax alpha-beta with PVS and iterative deepening
under a fixed 120,000-node budget per move (no clocks, no randomness),
transposition table, quiescence search over captures, promotions and check
evasions, null-move pruning, check extensions, MVV-LVA / promotion / killer
move ordering, incremental material + piece-square evaluation, repetition
and 50-move draws scored 0 in search.

MIT licensed, see LICENSE.
