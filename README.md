# Emerging Technologies Problems

This notebook explores the difference between classical and quantum algorithms using the Deutsch-Jozsa algorithm as an example.

## Problems

### Problem 1: Generating Random Boolean Functions
The Deutsch-Jozsa algorithm operates on Boolean functions — functions that take Boolean inputs (`True`/`False`) and return a single Boolean output. These functions are guaranteed to be one of two types:

- **Constant** - always returns the same value regardless of input. With four inputs there are only two possible constant functions: always `True` or always `False`.
- **Balanced** - returns `True` for exactly half of all possible input combinations and `False` for the other half. With four Boolean inputs there are 2⁴ = 16 possible input combinations, so a balanced function returns `True` for exactly 8 of them.

This problem implements `random_constant_balanced`, a function that randomly generates and returns either a constant or balanced function. The returned function takes four Boolean arguments and returns a single Boolean value. This gives us a source of mystery functions that we can analyse both classically in Problem 2 and quantumly in Problem 5, allowing a direct comparison of the two approaches.

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

