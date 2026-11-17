# tiny-chess-01: a 4 KB Python chess engine

`main.py` is the whole engine: it unpacks and runs an LZMA-compressed
Python source (the readable source was packed with the kit's
`makefun compress`). It plays the house gauntlet through the env at
`MAKEFUN_ENV_URL`, rebuilding each position by replaying the game's
move list.

## How it plays

- Negamax alpha-beta with PVS, iterative deepening under a fixed
  budget of 70,000 nodes per move. No clock is ever read, so replays
  match move for move.
- Transposition table keyed by incremental Zobrist hashing; move
  ordering by TT move, MVV-LVA captures, killers, then history.
- Null-move pruning, late-move reductions, futility pruning at shallow
  depth, check extensions, and mate-distance pruning so forced mates
  are found (and preferred) quickly.
- Quiescence over captures and promotions, with every check evasion
  searched when in check; repetition and the fifty-move rule score 0
  inside the search, as the house scores them.
- Tapered evaluation: middlegame/endgame piece-square tables blended
  by a material phase, plus the bishop pair and passed-pawn bonuses
  in the endgame.

## Layout

- `main.py` — the engine (4,096-byte cap, stdlib only)
- `LICENSE` — MIT
- `.makefun/owner.json` — claim file

## Local practice results

Replicated practice trials in the kit env against the house gauntlet:
Elo 1,451 / 1,451 / 2,112 across trials 0–2 (mean 1,671, vs the
sunfish baseline's 1,365). Local sparring against the v1 engine:
12–0, every game a checkmate win.
