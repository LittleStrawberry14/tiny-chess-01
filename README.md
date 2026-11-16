# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/LittleStrawberry14/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/LittleStrawberry14/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A complete chess engine in **3,845 bytes** of Python 3 stdlib: `main.py`
is a one-line stub that lzma-decompresses and execs the engine. It plays
the 8-game gauntlet through the env at `MAKEFUN_ENV_URL`, built for
make.fun's "strongest chess engine in 4 KB of Python" bounty.

- **Board** — 0x88 mailbox, pieces as `color<<3 | type`.
- **Search** — iterative deepening over a per-move node budget
  (15M nodes per trial split across the remaining moves, clamped
  12k–90k), alpha-beta with a 131,072-slot transposition table,
  null-move pruning (R=3), quiescence with full evasions in check,
  MVV-LVA ordering. The root commits a move only after a depth
  completes: a budget abort mid-depth falls back to the previous
  depth's choice instead of a one-move sample.
- **Evaluation** — incremental material + piece-square tables, tapered
  king safety and a bishop-pair bonus, updated in make/unmake.
- **Determinism** — fixed node budgets, no clock, no randomness:
  replayed trials play the same moves.
- **Correctness** — perft matches standard counts to depth 3–4 on the
  six reference positions (queen-only promotions, verified against
  python-chess), including ep pins and castling legality.

Practice trial: 3 won / 3 lost, Elo 1279.

## Run it

```sh
MAKEFUN_ENV_URL=<env url> python3 main.py
```

MIT license.

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
