# Siete y Media: Dynamic Programming Analysis

This repository contains a complete Python implementation of the analyses presented in:

> **Why Four? And Why Five? Optimal Play in Siete y Media**

The notebook computes all results exactly using dynamic programming. No Monte Carlo simulation is used.

---

## Overview

Siete y Media ("Seven and a Half") is a traditional Spanish card game played with a 40-card Spanish deck.

Card values:

- Face cards (Jack, Knight, King): 0.5 points
- Cards 1 through 7: face value

The objective is to obtain a score as close as possible to **7.5** without exceeding it.

This notebook studies three mathematical models:

1. **Infinite deck, expected-score maximization**
2. **Competitive play (win-probability maximization)**
3. **Finite deck (without replacement)**

---

# Part I: Infinite Deck

## Objective

Maximize expected final score.

At each score $$x$$, the player may:

- **Stand** and keep score $$x$$
- **Hit** and draw another card

The notebook solves the Bellman equation

$$
V(x)
=
\max
\left\{
x,
\sum_c p(c)V(x+c)
\right\}.
$$

Busts contribute zero.

## Main Result

The optimal policy is:

- **Hit** if $$x < 4.0$$
- **Stand** if $$x \ge 4.0$$

This produces

$$
V(0)
=
4.75112192.
$$

This result is known in the paper as the **Rule of Four**.

---

# Part II: Competitive Play

## Objective

Maximize the probability of winning against another player.

The opponent follows a threshold strategy $$T_{opp}$$.

The notebook computes:

1. Opponent final-score distributions.
2. Best-response policies.
3. Optimal stopping thresholds.
4. Win probabilities.

The value function becomes

$$
W(x)
=
\max
\left\{
R(x),
\sum_c p(c)W(x+c)
\right\},
$$

where $$R(x)$$ is the probability of winning by standing at score $$x$$.

## Main Result

Against an opponent using threshold

$$
T_{opp}=4.0,
$$

the optimal response is

$$
T_{player}=5.0.
$$

with win probability

$$
P(\text{win}) = 0.502284.
$$

Competition rewards additional risk, shifting behavior from the Rule of Four to a Rule of Five.

---

# Nash Equilibrium Analysis

The notebook explicitly computes:

- Threshold payoffs
- Best-response correspondences
- Symmetric fixed points

A threshold $$T^*$$ is a symmetric equilibrium if

$$
T^* \in BR(T^*).
$$

## Result

The unique symmetric equilibrium within the class of pure threshold strategies is

$$
T^* = 5.0.
$$

The notebook verifies:

$$
P(\text{win}\mid 5,5)=0.5,
$$

and finds no profitable threshold deviation.

---

# Part III: Finite Deck

## Objective

Analyze optimal play without replacement.

State variables consist of:

- Current score
- Remaining deck composition

The notebook solves

$$
V(x,c)
=
\max
\left\{
x,
\sum_i
\frac{c_i}{N}
V(x+v_i,c-e_i)
\right\}
$$

using memoized dynamic programming.

---

## Full Deck Result

Initial deck:

| Card Value | Count |
|------------|--------|
| 0.5 | 12 |
| 1 | 4 |
| 2 | 4 |
| 3 | 4 |
| 4 | 4 |
| 5 | 4 |
| 6 | 4 |
| 7 | 4 |

Result:

$$
V(0)
=
4.74075121
$$

with optimal threshold

$$
T^*=4.0.
$$

---

## Deck Composition Experiments

### Low-Rich Deck

Removed cards:

- 6
- 7

Result:

- Threshold = 4.0
- $$V(0)=4.862246$$

### High-Rich Deck

Removed cards:

- 0.5
- 1
- 2

Result:

- Threshold = 3.0
- $$V(0)=5.000000$$

---

# Main Conclusions

The notebook demonstrates how changing the objective function changes optimal behavior:

| Setting | Objective | Optimal Threshold |
|----------|------------|------------------|
| Infinite Deck | Maximize expected score | 4.0 |
| Competitive Play | Maximize win probability | 5.0 |
| Finite Deck | Depends on composition | 3.0–4.0 |

Key lesson:

> The underlying card game remains the same, but optimal decisions depend strongly on the payoff structure.

Risk-neutral score maximization favors stopping at **4**.

Head-to-head competition rewards additional variance and moves the equilibrium threshold to **5**.

---

# Computational Method

All reported values are computed exactly via dynamic programming.

The notebook uses:

- Backward induction
- Recursive dynamic programming
- Memoization (`functools.lru_cache`)
- Exact probability calculations

No simulation-based estimates are used.

---

# Requirements

Install:

```bash
pip install numpy pandas
```

Python version:

```text
Python 3.9+
```

---

# Running

Open the notebook in:

- Google Colab
- Jupyter Notebook
- JupyterLab

Run all cells sequentially.

The notebook prints:

- Part I value tables
- Best-response tables
- Nash equilibrium analysis
- Finite-deck computations
- Final summary tables

---

# Reproducibility

All numerical values reported in the accompanying paper are generated directly from this notebook.

Notable outputs:

```text
V(0) infinite deck = 4.75112192
```

```text
Best response to Topp = 4.0 is Tplayer = 5.0
```

```text
Symmetric equilibrium threshold = 5.0
```

```text
Finite-deck V(0) = 4.74075121
```

---

# Author

Prepared for the study:

**Why Four? And Why Five? Optimal Play in Siete y Media**

Dynamic programming implementation in Python.





