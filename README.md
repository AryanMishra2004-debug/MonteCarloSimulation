# MonteCarloSimulation
Semiconductor Yield Estimation — Monte Carlo Simulation

A Python simulation that estimates semiconductor manufacturing yield by modeling random process variation across thousands of virtual chips, then identifies which process parameter is the biggest yield-limiting factor.

Problem Statement

When a fab manufactures a wafer full of chips, not every chip works — tiny, unavoidable variations in the manufacturing process cause some chips to fall outside spec and fail. Yield is the percentage of chips on a wafer that actually work.

This project simulates that process computationally: instead of physically manufacturing chips, it generates thousands of "virtual" chips with randomized process variation and checks how many would pass a functional spec — enabling yield prediction before fabrication, and identifying which parameter to control more tightly to improve it.

Methodology
1. Single-parameter model (yield_simulation.py)
Models threshold voltage (Vt) as a normally distributed random variable (target 0.7V, ±0.05V wobble).
A chip passes if its Vt falls inside a fixed spec window (0.6V–0.8V).
Runs 10,000 simulated chips and computes yield = passed / total.
Visualizes the result as a histogram, shading pass/fail regions.

2. Combined two-parameter model (yield_simulation_combined.py)
Adds a second random process parameter: aspect ratio (W/L), the transistor's width-to-length ratio (target 10, ±1.5 wobble).

A chip passes only if it turns on (V_GS > V_T) and drives enough current to meet spec (I_D >= I_min).
Includes a sensitivity analysis that isolates each parameter's individual contribution to yield loss by holding the other fixed.
Visualizes the joint pass/fail boundary as a 2D scatter plot of Vt vs. W/L




