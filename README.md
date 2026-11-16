# tiny-chess-01: a 4,096-byte alpha-beta chess engine

`main.py` is a complete chess engine in **4,089 bytes** of Python (the
payload is lzma-compressed and unpacked by a one-line stub; the
decompressed engine is plain Python 3 stdlib). It plays the 8-game
gauntlet through the env at `MAKEFUN_ENV_URL`.

## The engine

- **Board** — 0x88 mailbox, pieces as `color<<3 | type`.
- **Search** — iterative deepening over a per-move node budget,
  alpha-beta with a transposition table (131,072 four-tuple slots,
  always-replace), null-move pruning (R=3, skipped in pawn/king-only
  endings), quiescence search with full evasion generation when in
  check, and MVV-LVA capture ordering. The root only commits a move
  after a depth completes, so a node-budget abort mid-depth falls back
  to the previous depth's choice instead of a one-move sample, and
  moves that repeat a position in the last 20 pairs score 0.
- **Evaluation** — incremental material + piece-square tables
  (hand-tuned), tapered king safety (midgame file shelter, endgame
  centralization) and a bishop-pair bonus, all updated in `make`/
  `unmake` so a leaf eval is one arithmetic pass.
- **Determinism** — fixed node budgets derived from a per-trial bank
  (15M nodes split across the remaining moves, clamped to 12k–90k per
  move). No wall clock, no unseeded randomness: replayed trials play
  the same moves.
- **Correctness** — move generation matches standard perft to depth
  3–4 on the six reference positions (with queen-only promotions,
  verified against `python-chess`), including ep pins and castling
  legality. Promotions are queen-only; under-promotions were cut for
  size, not correctness.

## Playing through the env

`main.py` reads only `MAKEFUN_ENV_URL`, POSTs `/game` until the trial
is done, thinks, POSTs `/move`, and resends `legal_moves[0]` if a move
is ever refused. Anything unexpected inside the search falls back to a
legal move rather than losing the game.

## Local checks

The unpacked engine passes the full perft suite (start, Kiwipete,
position 3–6, queen-only counts confirmed with python-chess), finds
mate-in-1/mate-in-2, and keeps per-move search time under ~6.5 s on the
calibrated CPU at the 90k-node cap.

MIT license. Built for the make.fun tiny-chess-01 bounty.
