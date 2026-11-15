# tiny-chess-01 entry

An original chess engine in 4 KB of Python 3.13 (standard library only),
entered in make.fun's `tiny-chess-01` bounty.

`main.py` is the whole engine: an LZMA-packed payload written by the kit's
`makefun compress` from the readable source (uploaded with the transcripts).

## Engine

- 120-square mailbox board kept as a string; the side to move is always
  "white" (the board is rotated and case-swapped after every move), so one
  move generator and one evaluation serve both colours
- incremental material + piece-square evaluation, with a separate king table
  for the endgame
- legal search: moves that leave the king attacked are skipped, so mates and
  stalemates are scored exactly (stalemate = 0)
- iterative deepening alpha-beta with a transposition table (best move,
  bounds), null-move pruning, late-move reductions, a killer move per ply,
  MVV-LVA ordering, check extension and capture/promotion quiescence
- threefold repetition seen from the game's full move list
- bounded by a fixed node budget per move, never a clock: the same position
  and history always give the same move, so a replay matches

## Local practice results

Run on the kit's env and scorer, one trial per practice instance (6 games
each, depth-2 to nodes-500k):

| instance | Elo | W-D-L |
|---|---|---|
| trial-0 | 1279 | 3-0-3 |
| trial-1 | 1364 | 3-1-2 |
| trial-2 | 1451 | 4-0-2 |

Every move stayed under about 4.3 s on one CPU (40,000-node budget, hard cap
three times that), well inside the 10 s move limit.

## License

MIT, see `LICENSE`.

## Ownership

`.makefun/owner.json` names a wallet linked to the submitting make.fun
account, since that account's GitHub login is not the repository owner.
