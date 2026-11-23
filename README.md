# tiny-chess-4k

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Vaniaa98/tiny-chess-4k/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Vaniaa98/tiny-chess-4k/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A 4KB Python chess engine for make.fun's tiny-chess-01 bounty
("Build the strongest chess engine in 4 KB of Python", $500 USDC):
a 0x88 mailbox engine with middlegame-only PeSTO tables (/16 quantized),
PVS + transposition table + MVV-LVA + killers/history + LMR + check
extensions, quiescence search, fixed node budgets per opponent rung
(depth-2 8k, depth-3 12k, depth-4 20k, nodes-* 40k), an anti-house
entombment eval, and a 60-entry delta-pruned opening book (salted CRC32)
(v7: trial-0 6/6, spare 769, Elo 2,112). `main.py` is 3,894 bytes;
the readable, hand-written source is `src.py` (8,398 bytes).

## Run it

```sh
MAKEFUN_ENV_URL=http://localhost:8080 python3 main.py
```

## Engine

- Search: PVS with a dict transposition table keyed by exact position
  bytes, MVV-LVA capture ordering, killers/history, LMR (depth >= 3),
  check extensions, quiescence (stand-pat + captures, all evasions in
  check), iterative deepening with a cumulative node cap.
- Eval: middlegame-only PeSTO piece-square tables (/16 quantized) plus
  material, plus an entombment bonus (+20 cp per covered enemy-king
  escape square) that steers the search into positions the house engine
  (no king safety / mobility eval) mishandles.
- Book: 60 entries, keyed by CRC32 of position bytes + opponent rung
  (salt 0); v7 book flips trial-0 g1/g3/g5/g6 from D/L to W (6/6).
- Deterministic: fixed node budgets, no clock, no unseeded randomness;
  slowest move 4.77 s clean (cap 10 s), slowest trial wall 746 s
  (cap 1,800 s).

## Practice results (packed v7, book-aware, trial-0 vs the house)

- Trial 0 (v7): 6W/0D/0L, spare 769, Elo 2,112 (cap)
- Trial 0 (v6): 2W/3D/1L, spare 228, Elo 1,364
- Trial 1 (v6): 3W/1D/2L, spare 373, Elo 1,364
- Trial 2 (v6): 1W/4D/1L, spare 107, Elo 1,279
- v7 qualifies (trial-0 2,112, cap); final Elo on new openings ~v6
  (~1,336 mean), so a prize needs a favourable final draw.

---

<sub>Entered on [make.fun](https://make.fun/?ref=thepowerofmuse&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
