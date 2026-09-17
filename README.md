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

Ten classic strategies are included, each a different answer to "what do I do given what's happened so far?":

| Strategy | Rule |
|---|---|
| Always Cooperate | Cooperates, no matter what |
| Always Defect | Defects, no matter what |
| Tit for Tat | Cooperates first, then mirrors the opponent's last move |
| Suspicious Tit for Tat | Defects first, then mirrors the opponent's last move |
| Grim Trigger | Cooperates until the opponent ever defects once — then defects forever |
| Tit for Two Tats | Only retaliates after the opponent defects *twice in a row* |
| Pavlov (Win-Stay, Lose-Shift) | Repeats its last move if the opponent cooperated, switches if they defected |
| Generous Tit for Tat | Like Tit for Tat, but forgives a defection about 10% of the time instead of always retaliating |
| Tester | Defects once to probe the opponent; switches to Tit for Tat if they retaliate, otherwise exploits them by defecting every other move |
| Random | Coin flip every round |

Uncheck any strategy to exclude it from the tournament and evolution below.

### 2. Choose a game

The four buttons under **Game** switch the payoff matrix between classic 2×2 games — same four values (T/R/P/S), different orderings, completely different dynamics:

- **Prisoner's Dilemma** (T > R > P > S, the default) — defecting wins individually, but mutual cooperation beats mutual defection.
- **Stag Hunt** (R > T > P > S) — mutual cooperation is the single best outcome for *both* players, but trusting your partner is risky.
- **Chicken** (T > R > S > P) — mutual defection is the *worst* outcome for both — better to back down than crash together.
- **Deadlock** (T > P > R > S) — defecting is just better, period. No real dilemma.

Expand **Payoff matrix (editable)** to set T/R/P/S by hand instead of using a preset.

### 3. Watch two strategies play head-to-head

Pick any two strategies from the dropdowns and click **▶ Play 20 rounds** to see their moves unfold one at a time — green `C` for cooperate, red `D` for defect — with the running score underneath. This is the fastest way to build intuition before looking at the full tournament: try Tit for Tat vs. Always Defect, or Grim Trigger vs. Suspicious Tit for Tat.

### 4. Play against a strategy yourself

Pick an opponent and click **Cooperate** or **Defect** yourself, one round at a time.

![Playing five rounds against Tit for Tat by hand](screenshots/3-human-vs-strategy.png)

It uses whatever payoff matrix is currently active, so you can feel the difference directly — try a few rounds against Tit for Tat under Prisoner's Dilemma, then switch to Stag Hunt and notice how much more tempting defection stops being (or, under Chicken, how much more it costs to never back down).

### 5. Run the full tournament

Every strategy plays every other strategy (and itself) for however many rounds you set, and total scores are ranked. Click **Run Tournament** any time you change the strategy roster, rounds, noise, or payoff values.

Turn up **Noise** above 0% and rerun — moves now occasionally get flipped by "mistake." Watch what happens to **Grim Trigger** (a single accidental defection locks it into permanent retaliation) compared to more forgiving strategies like **Pavlov**, **Tit for Two Tats**, or **Generous Tit for Tat**.

The score matrix below the chart shows the average points-per-round every strategy earns against every other strategy — useful for understanding *why* the ranking came out the way it did.

### 6. Run evolution

This is where it gets interesting. Instead of one tournament, imagine a population where every checked strategy starts with an equal share, and each generation, strategies that scored above the population's average grow their share while below-average strategies shrink — standard replicator dynamics, using the payoff matrix from the tournament above.

Click **Run Evolution** and watch the stacked-area chart animate generation by generation.

![A full run: tournament results plus an evolutionary population history](screenshots/2-full-run.png)

With every strategy included, this consistently reproduces Axelrod's real historical finding: **Always Defect**, **Suspicious Tit for Tat**, and **Random** get squeezed out toward extinction, while the reciprocal, "nice-but-not-a-pushover" strategies — **Tit for Tat**, **Tit for Two Tats**, **Grim Trigger**, **Pavlov**, **Generous Tit for Tat** — grow to dominate the population.

Try removing strategies from the roster before rerunning evolution — e.g., take out all the "nice" reciprocal strategies and leave only Always Cooperate, Always Defect, and Random, and watch a very different (and much bleaker) population history play out. Or turn up the **Mutation rate** and watch even the worst-performing strategies get held just above zero instead of going fully extinct:

![The same evolution run with a 3% mutation rate — nothing hits exactly 0%](screenshots/4-evolution-mutation.png)

## How it works

- **Match**: each strategy is a plain, stateless function of both players' move histories so far, called once per round — required since the same strategy object plays many different opponents across a tournament, so it can't keep its own private memory between matches.
- **Tournament**: a full round-robin (including self-play), accumulating total scores and an average-payoff-per-round matrix.
- **Evolution**: replicator dynamics — `share_i' = share_i · (fitness_i / average_fitness)`, renormalized each generation, using the same payoff matrix computed by the tournament. An optional mutation term blends in a small uniform share for every strategy each generation, `(1 − rate) · replicator_share + rate · (1/n)`, so nothing is ever driven to exact zero.
- **Game presets**: the same payoff logic (`T`/`R`/`P`/`S`) reused with different value ordering — Prisoner's Dilemma, Stag Hunt, Chicken, and Deadlock are all the identical 2×2 symmetric game structure, just with different relative payoffs.

Everything lives in `index.html` with no external libraries — open it in a text editor to see exactly how it works.

## Related projects

- **[cartpole-control](../cartpole-control)** — classical control (PID and LQR) balancing an inverted pendulum.
- **[cartpole-rl](../cartpole-rl)** — the same pendulum balanced by a reinforcement-learning agent instead.
