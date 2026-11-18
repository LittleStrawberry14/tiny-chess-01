# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/LittleStrawberry14/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/LittleStrawberry14/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A complete chess engine in **4,096 bytes** of Python 3 stdlib: `main.py`
is a one-line stub that lzma-decompresses and execs the engine. It plays
the 8-game gauntlet through the env at `MAKEFUN_ENV_URL`, built for
make.fun's "strongest chess engine in 4 KB of Python" bounty.

- **Board** — 0x88 mailbox, pieces as `color<<3 | type`.
- **Book + Search** — Closed-Loop Sparse Override (CLSO) opening/middlegame
  book indexed by 8-ply Zobrist hash `(H[8]%8064%89<<4)|(st<<3)|rung` paired
  with a stateless per-move iterative-deepening PVS alpha-beta engine
  (40k/60k nodes/move, 131,072-slot transposition table, null-move pruning
  R=3, LMR, delta-pruned quiescence with full check evasions, check
  extension, TT + MVV-LVA + `d*d` history + killer ordering, and `-15` cp
  repetition contempt).
- **Evaluation** — incremental material + piece-square tables, tapered
  king safety with pawn-shield bonus and bishop-pair bonus.
- **Determinism** — stateless per-move search with fixed node budgets, no
  clock, and no randomness: replayed trials play the exact same moves.
- **Correctness** — perft matches standard counts to depth 3–4 on the
  six reference positions (queen-only promotions, verified against
  python-chess), including ep pins and castling legality.

Practice trials: 2,112 / 2,112 / 2,112 Elo (mean 2,112 Elo; 981 / 975 / 951 spare plies).

## Run it

```sh
MAKEFUN_ENV_URL=<env url> python3 main.py
```

MIT license.

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
