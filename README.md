# tiny-chess-01 entry

An original, from-scratch chess engine in under 4 KB of Python 3.13 (standard
library only), entered in make.fun's `tiny-chess-01` bounty.

## How it works

`main.py` at the repository root is the entry the env runs. It is not written
by hand at that size: it is an LZMA-compressed, self-extracting payload
produced by the kit's `makefun compress` from the real source, which this
repository also documents here (the source itself is not committed, per the
competition's rules, but is uploaded with the build transcripts).

The engine:

- parses the FEN the env serves each turn into a simple `{(file, rank): piece}`
  board;
- generates pseudo-legal moves (pawns incl. en passant/promotion, knights,
  sliding bishops/rooks/queens, king incl. castling);
- always submits a move drawn from the env's own `legal_moves` for the
  current position, so it can never submit an illegal move even though its
  internal move generator is only pseudo-legal;
- searches with iterative-deepening negamax, alpha-beta pruning, and a
  capture/promotion quiescence search with delta pruning at the leaves;
- evaluates material plus simple piece-square tables, with a very high king
  value so a pseudo-legal line that exposes the king to capture is refuted by
  the search itself, without a separate check-detection pass;
- is bounded purely by a fixed node-count budget per move (never the wall
  clock), so it is fully deterministic: the same position and history always
  produce the same move, which the rules require.

## License

MIT, see `LICENSE`.

## Ownership

`.makefun/owner.json` names the wallet linked to the submitting make.fun
account, since that account's GitHub login is not the repository owner.
