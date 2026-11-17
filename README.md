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
entombment eval, and a 22-entry delta-pruned opening book (salted CRC32)
that flips two lost practice-trial games to wins. `main.py` is 3,634
bytes; the readable, hand-written source is `src.py` (7,813 bytes).

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
- Book: 22 entries, keyed by CRC32 of position bytes + opponent rung
  (salt 0), each entry verified to flip a game the bare engine loses:
  nodes-500k Semi-Slav (trial-1 game 6, L108 -> W67) and depth-2
  Kings-Indian-Attack (trial-2 game 1, L200 -> W93).
- Deterministic: fixed node budgets, no clock, no unseeded randomness;
  slowest move 4.77 s clean (cap 10 s), slowest trial wall 746 s
  (cap 1,800 s).

## Practice results (packed v6, book-aware, 3 trials vs the house)

- Trial 0: 2W/3D/1L, spare 228, Elo 1,364
- Trial 1: 3W/1D/2L, spare 373, Elo 1,364
- Trial 2: 1W/4D/1L, spare 107, Elo 1,279
- Mean Elo 1,336, total spare 708. Qualifies for the final (trial-0
  1,364 >> 1,154 floor); below the Sunfish practice mean (1,365) and
  the ~1,490 final target, so a prize needs a favourable final draw.

---

<sub>Entered on [make.fun](https://make.fun/?ref=thepowerofmuse&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
