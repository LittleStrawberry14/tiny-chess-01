# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/LittleStrawberry14/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/LittleStrawberry14/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A complete chess engine in **4,021 bytes** of Python 3 stdlib: `main.py`
is a one-line stub that lzma-decompresses and execs the engine. It plays
the 8-game gauntlet through the env at `MAKEFUN_ENV_URL`, built for
make.fun's "strongest chess engine in 4 KB of Python" bounty.

- **Board** — 0x88 mailbox, pieces as `color<<3 | type`.
- **Search** — iterative deepening over a per-move node budget
  (55M nodes per trial split across the remaining moves, clamped
  25k–300k), PVS alpha-beta with a 131,072-slot transposition table
  storing best moves, null-move pruning (R=3), quiescence with delta
  pruning and full evasions in check, checks extended at the horizon,
  TT-move + MVV-LVA + `d*d` history + killer-move ordering, and
  repetition contempt (`-15` cp).
- **Evaluation** — incremental material + piece-square tables, tapered
  king safety with pawn-shield bonus and bishop-pair bonus.
- **Determinism** — fixed node budgets, no clock, no randomness:
  replayed trials play the same moves.
- **Correctness** — perft matches standard counts to depth 3–4 on the
  six reference positions (queen-only promotions, verified against
  python-chess), including ep pins and castling legality.

Practice trials: 1,809 / 1,545 / 1,654 Elo (mean 1,669 Elo).

## Run it

```sh
MAKEFUN_ENV_URL=<env url> python3 main.py
```

MIT license.

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
