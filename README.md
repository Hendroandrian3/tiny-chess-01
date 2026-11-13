# tiny-chess-01

A chess engine in 4 KB of Python, built for the make.fun tiny-chess-01
bounty: the strongest engine that fits in 4,096 bytes.

`main.py` is the whole engine. It is the readable source in `source.py`,
packed with the kit's `makefun compress` (lzma) so the entry fits the cap.
No other file is read, imported or run.

Engine: 0x88 board; negamax with alpha-beta; quiescence over captures,
promotions and check replies; transposition table; null-move pruning;
late-move reductions; killer and history move ordering; tapered
piece-square evaluation with bishop-pair bonus. A fixed node budget per
move makes it deterministic: no clock, no unseeded randomness.

It talks to the trial environment at `MAKEFUN_ENV_URL` (`POST /game` to
serve the game in play, `POST /move` to play a move in UCI) and plays the
8-game gauntlet.

Local practice results (kit env + scorer, fixed 100,000 nodes/move):
trial-0 Elo 1253 (2W 2D 4L), trial-1 Elo 1326 (2W 3D 3L),
trial-2 Elo 1173 (2W 1D 5L); 24-game mean Elo ~1251, 6W 6D 12L.
Above the 1154 practice floor; below the 1325 sunfish baseline.
Perft 20/20, deterministic across re-runs, ~1.1-1.8s/move,
trial wall time ~18 min (limit 30 min).
