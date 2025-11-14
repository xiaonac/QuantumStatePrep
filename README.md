## Problem:

- Input: psi: np.ndarray of size $2^{2^n}$, complex, normalized (up to floating error).
- Output: QuantumCircuit on n qubits (no classical bits) that prepares psi from |0^n>.
- Constraint: Only 1Q gates and multi-controlled Rz. We realize uniformly-controlled Ry by basis-changes plus multiplexed Rz.
- How to Use:
  - prepare_state_circuit(psi: np.ndarray) -> QuantumCircuit

## Implementation Idea

1. Magnitudes: build a binary tree of uniformly-controlled rotations on qubits $$0\dots n-1$$ (MSB->LSB). At level k, for each control prefix, apply an $R_y(
\theta)$ on target k splitting the block's norm.
2. Phases: apply a diagonal via uniformly-controlled $Rz(\phi)$ so that each basis state $|x>$ acquires the desired relative phase $arg(\psi_x)$.
3. Gate set: identify $$R_y(\theta) = S^† H R_z(-\theta) H S.$$ Thus we only need local H, S, Sdg, and multi-controlled Rz.
