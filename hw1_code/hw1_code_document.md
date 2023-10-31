
# Homework 1 Codes

There are two files needed to code:

- `junction_tree.py`, for question 3(exact inference)
- `baum_welch.py`, for question 4(parameter learning)

This file will show how I write python implementation.

## junction_tree

Homework 3.3 requires us to implement belief propagation on $\mathcal{T}$,
by completing `junction_tree.py` with four tasks:

- `initial_clique_potentials()`, computing the initial potential function.
- `messages()`, computing messages $\delta_{i \to j}$ from initial clique potential.
- `beliefs()`, compute calibrated clique belief $\beta_i(C_i)$ and sepset belief $\mu_{i,j}$.
- three queries: `query<num>()`, `<num>` in $\{1,2,3\}$.

## Baum-Welch Algorithm
Homework 4 requires an implementation for Baum-Welch algorithm to estimate HMM parameters from data, by coding `baum_welch.py`.
