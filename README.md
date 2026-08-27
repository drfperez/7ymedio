# Siete y Media: Dynamic Programming Analysis

Python implementation of exact dynamic programming solutions for the Spanish card game **Siete y Media**, including optimal stopping rules, competitive play, Nash equilibrium analysis, and finite-deck card counting.

## Overview

Siete y Media ("Seven and a Half") is played with a 40-card Spanish deck.

Card values:

- Face cards (Jack, Knight, King): 0.5 points
- Cards 1–7: face value

The goal is to get as close as possible to **7.5** without exceeding it.

This notebook analyzes three settings:

1. Infinite deck (with replacement)
2. Competitive play (maximize win probability)
3. Finite deck (without replacement)

All results are computed exactly using dynamic programming.

---

## Part I: Infinite Deck

### Objective

Maximize expected final score.

At each decision point, a player can:

- **Stand** and keep the current score.
- **Hit** and draw another card.

The Bellman equation is solved exactly.

### Main Result

Optimal policy:

- Hit if score < 4.0
- Stand if score ≥ 4.0

Expected value from the start:

```text
V(0) = 4.75112192
```

This optimal stopping rule is called the **Rule of Four**.

---

## Part II: Competitive Play

### Objective

Maximize the probability of defeating an opponent.

The opponent follows a threshold strategy and the player computes the optimal response.

The notebook computes:

- Opponent score distributions
- Best-response policies
- Win probabilities
- Threshold equilibria

### Main Results

Against an opponent using:

```text
Threshold = 4.0
```

the optimal response is:

```text
Threshold = 5.0
```

with

```text
P(win) = 0.502284
```

Competition rewards additional risk and shifts behavior from the Rule of Four to a **Rule of Five**.

---

## Nash Equilibrium Analysis

The notebook explicitly computes:

- Payoff matrices
- Best-response correspondences
- Symmetric fixed points

### Main Result

The unique symmetric equilibrium within the class of pure threshold strategies is:

```text
T* = 5.0
```

with

```text
P(win | 5.0, 5.0) = 0.500000
```

No profitable threshold deviation exists against an opponent using threshold 5.0.

---

## Part III: Finite Deck

### Objective

Analyze optimal play when cards are drawn without replacement.

The state consists of:

- Current score
- Remaining deck composition

The notebook solves the finite dynamic program exactly using memoized recursion.

### Full Deck Result

Initial deck composition:

| Value | Count |
|---------|---------|
| 0.5 | 12 |
| 1 | 4 |
| 2 | 4 |
| 3 | 4 |
| 4 | 4 |
| 5 | 4 |
| 6 | 4 |
| 7 | 4 |

Result:

```text
V(0) = 4.74075121
```

Optimal threshold:

```text
T* = 4.0
```

---

## Deck Composition Experiments

### Low-Rich Deck

Removed cards:

```text
6 and 7
```

Result:

```text
Threshold = 4.0
V(0) = 4.862246
```

### High-Rich Deck

Removed cards:

```text
0.5, 1, and 2
```

Result:

```text
Threshold = 3.0
V(0) = 5.000000
```

---

## Summary of Results

| Setting | Objective | Optimal Threshold |
|----------|----------|----------|
| Infinite Deck | Maximize expected score | 4.0 |
| Competitive Play | Maximize win probability | 5.0 |
| Finite Deck | Depends on composition | 3.0–4.0 |

Key insight:

> The same card game produces different optimal strategies when the objective function changes. Score maximization favors stopping at four, while head-to-head competition favors stopping at five.

---

## Computational Method

The notebook uses:

- Dynamic programming
- Backward induction
- Bellman equations
- Memoization (`functools.lru_cache`)
- Exact probability calculations

No Monte Carlo simulation is used.

---

## Requirements

Install dependencies:

```bash
pip install numpy pandas
```

Supported versions:

```text
Python 3.9+
```

---

## Running

Open the notebook in:

- Google Colab
- Jupyter Notebook
- JupyterLab

Run all cells sequentially.

The notebook generates:

- Infinite-deck value tables
- Best-response tables
- Nash equilibrium analysis
- Finite-deck computations
- Summary tables

---

## Reproducibility

All numerical results reported in the accompanying paper are generated directly from this notebook.

Key outputs:

```text
Infinite deck:
V(0) = 4.75112192
Threshold = 4.0
```

```text
Competitive model:
Best response to 4.0 = 5.0
P(win) = 0.502284
```

```text
Nash equilibrium:
T* = 5.0
P(win | 5.0, 5.0) = 0.500000
```

```text
Finite deck:
V(0) = 4.74075121
Threshold = 4.0
```

---

## Associated Paper

**Why Four? And Why Five? Optimal Play in Siete y Media**

This repository contains the code used to generate the computational results appearing in the manuscript.

---

## License

MIT License





