# Semiconductor Yield Estimation — Monte Carlo Simulation

A Python simulation that estimates semiconductor manufacturing yield by modeling random
process variation across thousands of virtual chips, then identifies which process
parameter is the biggest yield-limiting factor.

## Problem Statement

When a fab manufactures a wafer full of chips, not every chip works — tiny, unavoidable
variations in the manufacturing process cause some chips to fall outside spec and fail.
**Yield** is the percentage of chips on a wafer that actually work.

This project simulates that process computationally: instead of physically manufacturing
chips, it generates thousands of "virtual" chips with randomized process variation and
checks how many would pass a functional spec — enabling yield prediction before
fabrication, and identifying which parameter to control more tightly to improve it.

## What's in this project

| File | What it does |
|---|---|
| `yield_simulation.py` | Single-parameter model: threshold voltage (Vt) variation only |
| `yield_simulation_combined.py` | Two-parameter model: Vt + aspect ratio (W/L), with sensitivity analysis |

## Methodology

### 1. Single-parameter model (`yield_simulation.py`)

Models threshold voltage (Vt) as a normally distributed random variable:
- Target: 0.7V
- Process variation (wobble): ±0.05V

A chip **passes** if its Vt falls inside a fixed spec window (0.6V–0.8V).

Runs 10,000 simulated chips and computes:

```
yield = passed / total
```

Visualizes the result as a histogram, with the pass/fail regions shaded to show exactly
where the spec boundary cuts into the distribution.

### 2. Combined two-parameter model (`yield_simulation_combined.py`)

Adds a second random process parameter: **aspect ratio (W/L)**, the transistor's
width-to-length ratio:
- Target: 10
- Process variation (wobble): ±1.5

A chip passes only if **both** conditions hold:
1. It turns on: `V_GS > V_T`
2. It drives enough current to meet spec: `I_D >= I_min`

**Sensitivity analysis:** isolates each parameter's individual contribution to yield loss
by holding the other parameter fixed at its target value and varying only one at a time —
this is what identifies which parameter is the dominant yield-limiting factor, rather than
just reporting a combined yield number.

Visualizes the joint pass/fail boundary as a 2D scatter plot of Vt vs. W/L, with passing
and failing chips colored separately, showing the actual shape of the spec region in
two-parameter space.

## Key Finding

Threshold voltage variation is the dominant yield-limiting factor — tightening control on
Vt process variation would improve yield more than equivalent tightening on the aspect
ratio (W/L).

## Setup

```bash
pip install numpy matplotlib
```

## Run

```bash
python yield_simulation.py            # single-parameter model
python yield_simulation_combined.py   # two-parameter model + sensitivity analysis
```

Each script prints the computed yield percentage to the console and displays/saves the
corresponding plot (histogram for the single-parameter model, scatter plot for the
combined model).

## Possible extensions

- Add a third process parameter (e.g. oxide thickness) and extend the sensitivity analysis
  to a full multi-way decomposition of yield loss
- Replace the fixed spec window with a more realistic multi-corner spec (different limits
  at different operating temperatures/voltages)
- Model correlated process variation (e.g. Vt and W/L are not fully independent on a real
  wafer) instead of treating each parameter as independently random
- Fit the simulated yield against a closed-form analytical yield model and compare accuracy
