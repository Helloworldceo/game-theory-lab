# Game Theory Lab

An interactive playground for the **Iterated Prisoner's Dilemma** — run Robert Axelrod's famous 1980s computer tournaments yourself, then watch an entire population of strategies evolve across generations under replicator dynamics.

No install, no build step, no dependencies. It's a single HTML file — open it and it runs.

![Strategies, payoffs, and the auto-run tournament](screenshots/1-tournament.png)

## Quick start

1. Download or clone this repo.
2. Open `index.html` in any modern browser.
3. The tournament runs automatically on load — scroll down to see the results.
4. Click **Run Evolution** to watch the population shift across generations.

## Step by step

### 1. Meet the strategies

Eight classic strategies are included, each a different answer to "what do I do given what's happened so far?":

| Strategy | Rule |
|---|---|
| Always Cooperate | Cooperates, no matter what |
| Always Defect | Defects, no matter what |
| Tit for Tat | Cooperates first, then mirrors the opponent's last move |
| Suspicious Tit for Tat | Defects first, then mirrors the opponent's last move |
| Grim Trigger | Cooperates until the opponent ever defects once — then defects forever |
| Tit for Two Tats | Only retaliates after the opponent defects *twice in a row* |
| Pavlov (Win-Stay, Lose-Shift) | Repeats its last move if the opponent cooperated, switches if they defected |
| Random | Coin flip every round |

Uncheck any strategy to exclude it from the tournament and evolution below. The **payoff matrix** (T/R/P/S) is editable too — the classic values (5/3/1/0) are what makes this specifically a *Prisoner's Dilemma* rather than some other game (you need T > R > P > S for the dilemma to hold).

### 2. Watch two strategies play head-to-head

Pick any two strategies from the dropdowns and click **▶ Play 20 rounds** to see their moves unfold one at a time — green `C` for cooperate, red `D` for defect — with the running score underneath. This is the fastest way to build intuition before looking at the full tournament: try Tit for Tat vs. Always Defect, or Grim Trigger vs. Suspicious Tit for Tat.

### 3. Run the full tournament

Every strategy plays every other strategy (and itself) for however many rounds you set, and total scores are ranked. Click **Run Tournament** any time you change the strategy roster, rounds, noise, or payoff values.

Turn up **Noise** above 0% and rerun — moves now occasionally get flipped by "mistake." Watch what happens to **Grim Trigger** (a single accidental defection locks it into permanent retaliation) compared to more forgiving strategies like **Pavlov** or **Tit for Two Tats**.

The score matrix below the chart shows the average points-per-round every strategy earns against every other strategy — useful for understanding *why* the ranking came out the way it did.

### 4. Run evolution

This is where it gets interesting. Instead of one tournament, imagine a population where every checked strategy starts with an equal share, and each generation, strategies that scored above the population's average grow their share while below-average strategies shrink — standard replicator dynamics, using the payoff matrix from the tournament above.

Click **Run Evolution** and watch the stacked-area chart animate generation by generation.

![A full run: tournament results plus an evolutionary population history](screenshots/2-full-run.png)

With the default 8 strategies, this consistently reproduces Axelrod's real historical finding: **Always Defect**, **Suspicious Tit for Tat**, and **Random** get squeezed out toward extinction, while the reciprocal, "nice-but-not-a-pushover" strategies — **Tit for Tat**, **Tit for Two Tats**, **Grim Trigger**, **Pavlov** — grow to dominate the population. **Always Cooperate** typically survives at a smaller share, propped up by the now-mostly-cooperative population around it.

Try removing strategies from the roster before rerunning evolution — e.g., take out all three "nice" reciprocal strategies and leave only Always Cooperate, Always Defect, and Random, and watch a very different (and much bleaker) population history play out.

## How it works

- **Match**: each strategy is a plain function of both players' move histories so far, called once per round.
- **Tournament**: a full round-robin (including self-play), accumulating total scores and an average-payoff-per-round matrix.
- **Evolution**: replicator dynamics — `share_i' = share_i · (fitness_i / average_fitness)`, renormalized each generation, using the same payoff matrix computed by the tournament.

Everything lives in `index.html` with no external libraries — open it in a text editor to see exactly how it works.

## Related projects

- **[cartpole-control](../cartpole-control)** — classical control (PID and LQR) balancing an inverted pendulum.
- **[cartpole-rl](../cartpole-rl)** — the same pendulum balanced by a reinforcement-learning agent instead.
