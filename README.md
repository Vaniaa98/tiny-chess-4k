# tiny-chess-4k

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Vaniaa98/tiny-chess-4k/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Vaniaa98/tiny-chess-4k/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A 4KB Python chess engine for make.fun's tiny-chess-01 bounty
("Build the strongest chess engine in 4 KB of Python", $500 USDC):
a 0x88 mailbox engine with middlegame-only PeSTO tables (/16 quantized),
PVS + transposition table + MVV-LVA + killers/history + LMR + check
extensions, quiescence search, a uniform 80k node budget, safe
repetition detection, and an anti-house entombment eval. `main.py` is
3,932 bytes.

## Run it

```sh
MAKEFUN_ENV_URL=http://localhost:8080 python3 main.py
```

## Engine

- Search: PVS with a dict transposition table keyed by Zobrist hash,
  MVV-LVA capture ordering, killers/history, LMR (depth >= 3),
  check extensions, quiescence (stand-pat + captures, all evasions in
  check), iterative deepening with a cumulative node cap.
- Eval: middlegame-only PeSTO piece-square tables (/16 quantized) plus
  material, plus an entombment bonus (+20 cp per covered enemy-king
  escape square) that steers the search into positions the house engine
  (no king safety / mobility eval) mishandles.
- Deterministic: fixed node budget, no clock, no unseeded randomness;
  slowest move 2.48 s (cap 10 s).

## Practice results (v8, Variant B, 3 trials vs the house)

- Trial 0/1/2: 4W/0D/2L each, Elo 1,451 each, spare 476/512/432
- Mean Elo 1,451, total spare 1,420 — above the Sunfish practice mean
  (1,365). Unseen openings (16 games): Elo 1,497, paired +245 vs Sunfish.

---

<sub>Entered on [make.fun](https://make.fun/?ref=thepowerofmuse&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
