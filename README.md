# Emerging Technologies Problems

This notebook explores the difference between classical and quantum algorithms using the Deutsch-Jozsa algorithm as an example.

## Problems

### Problem 1: Generating Random Boolean Functions
Implements a function that randomly generates either a constant or balanced Boolean function taking four Boolean inputs.

### Problem 2: Classical Testing for Function Type
Implements a classical algorithm to determine whether a function is constant or balanced. The maximum number of calls needed to be 100% certain is 9 — if after 9 calls the function has only returned one value, any further result will either confirm constant or reveal balanced. A constant function requires all 16 calls to be certain.

### Problem 3: Quantum Oracles
Implements quantum oracles for each of the four possible single-Boolean-input functions using Qiskit.

### Problem 4: Deutsch's Algorithm
Implements Deutsch's algorithm using Qiskit to determine whether a single-input function is constant or balanced using only one query.

### Problem 5: Deutsch-Jozsa Algorithm
Scales Deutsch's algorithm to handle four-input functions, demonstrating the quantum advantage over the classical approach from Problem 2.

## Requirements
- Python 3
- Qiskit
- Jupyter Notebook

