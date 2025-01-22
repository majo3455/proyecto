# Import the correct packages for current Qiskit version
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_bloch_multivector, plot_histogram
from qiskit.primitives import Sampler
import matplotlib.pyplot as plt

# Create a quantum circuit with one qubit
qc = QuantumCircuit(1)

# Step 1: Initial state |0⟩
print("Step 1: Initial state |0⟩")
initial_state = Statevector.from_instruction(qc)
print("State vector:", initial_state)

# Visualize initial state
initial_plot = plot_bloch_multivector(initial_state)
plt.show()

# Step 2: Create superposition with Hadamard gate
qc.h(0)  # Apply Hadamard gate
print("\nStep 2: After applying Hadamard gate")
print("Circuit:")
print(qc)

# Get state after Hadamard
superposition_state = Statevector.from_instruction(qc)
print("\nState vector:", superposition_state)
print("This represents: |ψ⟩ = (|0⟩ + |1⟩)/√2")

# Visualize superposition
superposition_plot = plot_bloch_multivector(superposition_state)
plt.show()

# Create a new circuit for measurement demonstration
qc_measure = QuantumCircuit(1, 1)  # 1 qubit and 1 classical bit
qc_measure.h(0)     # Create superposition
qc_measure.measure([0], [0])  # Measure the qubit

# Run the circuit with measurement
sampler = Sampler()
job = sampler.run(qc_measure, shots=1000)
result = job.result()

# Process the results
counts = result.quasi_dists[0]
counts_dict = {format(key, '0' + str(1) + 'b'): value for key, value in counts.items()}

# Show measurement results
print("\nMeasurement Results (1000 shots):")
print(counts_dict)

# Plot the histogram
plot_histogram(counts_dict)
plt.show()
