# Emerging Technologies Problems

This notebook explores the difference between classical and quantum algorithms using the Deutsch-Jozsa algorithm as an example.

## Problems

### Problem 1: Generating Random Boolean Functions
The Deutsch-Jozsa algorithm operates on Boolean functions — functions that take Boolean inputs (`True`/`False`) and return a single Boolean output. These functions are guaranteed to be one of two types:

- **Constant** - always returns the same value regardless of input. With four inputs there are only two possible constant functions: always `True` or always `False`.
- **Balanced** - returns `True` for exactly half of all possible input combinations and `False` for the other half. With four Boolean inputs there are 2⁴ = 16 possible input combinations, so a balanced function returns `True` for exactly 8 of them.

This problem implements `random_constant_balanced`, a function that randomly generates and returns either a constant or balanced function. The returned function takes four Boolean arguments and returns a single Boolean value. This gives us a source of mystery functions that we can analyse both classically in Problem 2 and quantumly in Problem 5, allowing a direct comparison of the two approaches.

### Problem 2: Classical Testing for Function Type
Before exploring the quantum advantage, we must first understand the classical cost of solving the same problem. This problem implements `determine_constant_balanced`, a function that takes a mystery function `f` (as generated in Problem 1) and determines whether it is constant or balanced purely through classical means — by calling `f` with different inputs and observing the outputs.

**Strategy:** Call `f` with each possible input combination one at a time. The moment both a `True` and a `False` result have been observed, we can immediately conclude the function is balanced and stop — a constant function would never return two different values. If all inputs are exhausted and only one unique output value has been seen, the function must be constant.

**Efficiency:** In the worst case, we need to call `f` a maximum of **9 times** to be 100% certain of the answer. This is because after seeing the same output 8 times in a row, there is still a possibility the function is balanced — the 9th call will either return a different value (confirming balanced) or the same value again (at which point it is impossible for the function to be balanced, so it must be constant). If the function is constant, all 16 calls are required to be completely certain, as there is always a chance the next call could return a different value until every input has been tried.

This classical worst case of 9 calls contrasts sharply with the quantum approach demonstrated in Problem 5, where the Deutsch-Jozsa algorithm determines the answer with a **single query** to the function, regardless of how many inputs it takes.

### Problem 3: Quantum Oracles
To use a classical Boolean function in a quantum algorithm, it must first be encoded as a **quantum oracle** — a quantum circuit that implements the function in a way that is compatible with quantum superposition. Rather than simply computing `f(x)` and returning the result, the oracle transforms a two-qubit state according to the rule:

|x⟩|y⟩ → |x⟩|y ⊕ f(x)⟩

Where `x` is the input qubit, `y` is the output qubit, and `⊕` is the XOR operation. The output qubit is flipped if and only if `f(x) = 1`, leaving the input qubit unchanged. This formulation is necessary because quantum operations must be reversible — unlike classical logic gates, you cannot simply discard information.

With a single Boolean input, there are exactly four possible Boolean functions:

| Function | f(0) | f(1) | Type |
|----------|------|------|------|
| Constant 0 | 0 | 0 | Constant |
| Constant 1 | 1 | 1 | Constant |
| Identity | 0 | 1 | Balanced |
| NOT | 1 | 0 | Balanced |

Each oracle is implemented using two fundamental quantum gates:

- **X gate** - flips a qubit from |0⟩ to |1⟩ or vice versa, equivalent to a classical NOT operation
- **CNOT gate** - flips the target qubit only if the control qubit is |1⟩, equivalent to a classical XOR operation

This problem implements all four oracles and demonstrates each one by running it on the Qiskit simulator with both possible inputs, verifying that each oracle correctly encodes its corresponding function.


### Problem 4: Deutsch's Algorithm with Qiskit
[Deutsch's algorithm](https://quantum.cloud.ibm.com/learning/en/courses/fundamentals-of-quantum-algorithms/quantum-query-algorithms/deutsch-algorithm) is the simplest example of a quantum algorithm that demonstrates a provable advantage over any classical approach. While a classical computer must call the function at least twice to determine whether it is constant or balanced, Deutsch's algorithm determines the answer with a **single query** to the oracle, regardless of the result.

The circuit uses two qubits — an input qubit and an output qubit — and works as follows:

1. **Initialisation** - the input qubit starts as |0⟩ and the output qubit is flipped to |1⟩ using an X gate
2. **Superposition** - Hadamard gates are applied to both qubits, placing them into superposition so that both possible inputs are encoded simultaneously in a single quantum state
3. **Oracle query** - the oracle is applied once, encoding the function into the quantum state across both inputs at the same time
4. **Interference** - a final Hadamard gate is applied to the input qubit, causing constructive or destructive interference depending on whether the function is constant or balanced
5. **Measurement** - the input qubit is measured

**Reading the result:**

- If the input qubit measures as **0** → the function is **constant**
- If the input qubit measures as **1** → the function is **balanced**

The interference step is the key to why this works. For constant functions, the quantum states reinforce each other and the input qubit returns to |0⟩. For balanced functions, the states cancel each other out, leaving the input qubit as |1⟩. This allows a single query to reveal a global property of the function — something that is impossible classically without at least two calls.

The circuit is demonstrated below using each of the four oracles from Problem 3, with 1024 shots confirming that the result is deterministic and not probabilistic.

### Problem 5: Deutsch-Jozsa Algorithm

Scales Deutsch's algorithm from Problem 4 up to handle the four-bit functions generated in Problem 1, completing the comparison between the classical and quantum approaches. The circuit uses 5 qubits — 4 input qubits and 1 output qubit — and works by encoding all 16 possible input combinations into a single quantum state via superposition, querying the oracle once, and using interference to reveal whether the function is constant or balanced.

The classical function from Problem 1 is encoded as a quantum oracle by iterating over all 16 inputs and adding a multi-controlled CNOT gate for each input where `f(x) = 1`. The algorithm is demonstrated on both constant functions and two randomly generated balanced functions, showing that the circuit correctly identifies each one with a single query — compared to the worst case of 9 calls required by the classical approach in Problem 2.
## Requirements
- Python 3
- Qiskit
- Jupyter Notebook

