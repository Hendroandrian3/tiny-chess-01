# tiny-chess-01

A chess engine in 4 KB of Python, built for the make.fun tiny-chess-01
bounty: the strongest engine that fits in 4,096 bytes.

`main.py` is the whole engine. It is the readable source in `source.py`,
packed with the kit's `makefun compress` (lzma) so the entry fits the cap.
No other file is read, imported or run.

Engine: 0x88 board; negamax with alpha-beta; quiescence over captures,
promotions and check replies; transposition table; null-move pruning;
late-move reductions; killer and history move ordering; tapered
piece-square evaluation with bishop-pair bonus, pawn structure
(passed/doubled/isolated), rook on open and semi-open files, and
king pawn-shield safety. Search adds a check extension, repetition
detection on the search path, and delta pruning in quiescence. A fixed
node budget per move makes it deterministic: no clock, no unseeded
randomness.

It talks to the trial environment at `MAKEFUN_ENV_URL` (`POST /game` to
serve the game in play, `POST /move` to play a move in UCI) and plays the
8-game gauntlet.

Local practice results (kit env + scorer, fixed 100,000 nodes/move,
6-game practice format; machine quiet, no concurrent load):
trial-0 Elo 1364 (3W 1D 2L), trial-1 Elo 1545 (4W 1D 1L),
trial-2 Elo 1451 (3W 2D 1L); 18-game mean Elo ~1453, 10W 4D 4L.
Baseline engine measured the same way: 1279 / 1364 / 1191, mean ~1278,
so this build is +175 Elo over baseline and +88 over the sunfish 4KB
6-game mean (~1365). It wins all three depth games in every trial and
has beaten/drawn the nodes-250k and nodes-500k house engines.
Perft 19/19 (local suite), deterministic across re-runs and
PYTHONHASHSEED values, 1.7-4.9s/move (limit 10s),
trial wall time ~12-19 min (limit 30 min).
