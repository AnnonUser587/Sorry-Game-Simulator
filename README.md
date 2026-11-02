# Sorry! Simulator

> **"With no Player 4, whoever is seated right before the gap gets a free pass from one direction of attack without losing any of their own offense — a structural advantage that has nothing to do with skill."**

A browser-based Monte Carlo simulator for the board game *Sorry!*. It plays thousands of full games with 2, 3, or 4 players (using either a "smart" or "random" strategy for each seat) and reports each player's actual win rate, so you can see whether turn order / seating gives anyone a statistically significant edge.

## Features

- Simulate **2, 3, or 4 players**, any color combination (Yellow, Green, Red, Blue)
- Per-seat strategy: **Smart** (heuristic-driven) or **Random**
- Choose whether the first player is **fixed (always P1)** or **random each game**
- Optional seed for reproducible runs
- Runs in Web Workers (multi-threaded) for fast large-scale simulation, with a main-thread fallback
- Live view of a sample game while the batch runs (board, card draws, win-rate bars)
- Results panel with:
  - Win counts and win share (with 95% confidence interval) per player
  - A chi-square test against the "everyone wins equally" null hypothesis
  - A plain-language fair / not fair verdict
  - Game stats: average rounds per game, pawns bumped back to Start, slides taken, and first-mover win rate

## Usage

1. Open `index.html` in any modern browser — no build step or server required.
2. Pick 2–4 players and a strategy for each.
3. Set the number of games to simulate and whether the first player is fixed or random.
4. Click **Run simulation** and watch the live preview, or **Stop** at any time.
5. Review the results panel for win rates and the fairness verdict.

## How fairness is measured

For each run, the simulator compares each player's actual win count to the "fair share" (`1 / number of players`) using a chi-square goodness-of-fit test. If the resulting p-value is below 0.05, the win-rate gap is unlikely to be chance, and the simulator reports the game as **not fair** for that configuration (and explains why, e.g. mixed strategies vs. turn-order/seating effects).

## Why 3-player games are unfair

*Sorry!* is built around four seats in a fixed clockwise turn order: Yellow → Green → Red → Blue → back to Yellow. Every seat's "Sorry!" card lets you pull a pawn out of your start and bump an opponent's pawn in one move, and in the normal 4-player game, each color has a natural target: the player who goes right before you is the one whose pawns you're most likely to run into and bump back to Start, and the player who goes right after you is the one most likely to bump *you*. With all four seats filled, that pressure is symmetric — everyone has someone bumping them and someone they can bump.

Drop one seat and that symmetry breaks. The board itself doesn't change — the vacant color's track, start, and safety zone are just empty — but the turn order does: play skips straight over the missing seat. That means the player who sits right before the empty seat effectively loses the opponent who would normally be bumping their pawns back from that direction, while still being able to bump the players on either side of them. In other words, with no Player 4, whoever is seated right before the gap gets a free pass from one direction of attack without losing any of their own offense — a structural advantage that has nothing to do with skill, which is exactly what the simulator's win-rate gap and chi-square test are picking up on in the 3-player case.
