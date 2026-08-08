# Pokerbot — MIT Pokerbots 2026

A poker-playing bot built for MIT's annual Pokerbots competition, combining
Monte Carlo simulation, opponent modeling, and EV-based decision logic to
play a 3-hole-card variant of Texas Hold'em (choose one card to discard
face-up after the flop).

## Approach

- **Monte Carlo discard simulation** — for each of the three hole cards,
  runs randomized rollouts against a simulated opponent hand and future
  board to estimate win rate if that card is discarded, then picks the
  discard that maximizes simulated equity. Simulation depth scales with
  remaining game clock and round number to balance accuracy against time
  budget.
- **Opponent modeling** — tracks the opponent's discard tendencies
  (frequency of discarding high cards, frequency of discarding cards that
  would have added board entropy) using Laplace-smoothed counts, updated
  after every round. This bias feeds into both discard scoring and
  in-game aggression.
- **Board entropy heuristic** — scores each board 0–5 based on pairing,
  flush-draw density, straight-draw gaps, and rank clustering, then uses
  that score (together with the opponent bias) to dynamically scale
  aggression — betting looser on "quiet" boards, tighter on chaotic ones.
- **EV-based raise/call logic** — for medium-strength hands, simulates
  expected value of raising vs. calling using pot odds, position, and a
  simulated opponent fold probability, and picks the action with the
  higher expected value rather than a fixed strength threshold.

## Files

- [`python_skeleton/player.py`](python_skeleton/player.py) — the bot itself
- [`python_skeleton/skeleton/`](python_skeleton/skeleton) — the MIT Pokerbots
  Python skeleton (actions, bot base class, state objects, and the runner
  that talks to the engine)

## Running it

This bot runs against the MIT Pokerbots engine, which lives in this repo.

1. Install [`uv`](https://docs.astral.sh/uv/) for dependency management:
   ```bash
   # macOS or Linux
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```
2. From the repo root, set up the environment:
   ```bash
   uv venv
   uv sync
   ```
3. Run the engine (edit `config.py` to point at `python_skeleton/` for this bot):
   ```bash
   .venv/bin/python engine.py
   ```

## Background

Built for [MIT Pokerbots](https://www.pokerbots.org/), an annual
programming competition where teams write autonomous poker-playing bots
that compete head-to-head across many hands.
