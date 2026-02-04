# Quantum vs Classical SVM: Cross-Framework Benchmarking on MNIST

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)

A capstone project for **The Coding School — Qubit x Qubit** program, continued investigating how quantum support vector machines (QSVMs) perform across three major quantum frameworks: **PennyLane**, **Cirq**, and **Qiskit**. This work reveals a critical insight often overlooked in quantum ML literature: **framework choice significantly impacts classical baseline performance** — sometimes more than the quantum model itself.

![Results Summary](Cirq_QSVM vs SVM.png, Pennylane_QSVM vs SVM.png, Qiskit_QSVM vs SVM.png)

## 🔬 Key Finding

While QSVM accuracy remained stable at **99%** in PennyLane and Cirq implementations, the *classical SVM baseline* accuracy varied dramatically (**71.5% → 88.0% → 99.0%**) depending solely on framework integration overhead. Training time similarly spanned three orders of magnitude (**3.33s → 50.27s → 1057.90s**).

> 💡 **Novelty**: First empirical demonstration that framework-specific implementation details—not quantum advantage alone—drive performance variations in QSVM benchmarking. Most prior work evaluates QSVMs within *single* frameworks; we benchmark *identical* models across three.

## 📊 Results Summary

| Framework          | Model         | Accuracy (%) | Time (s) | Key Insight                          |
|--------------------|---------------|--------------|----------|--------------------------------------|
| **PennyLane** (NumPy) | QSVM          | 99.0         | 0.00     | Fastest simulation; lightweight wrapper |
|                    | Classical SVM | 71.5         | 3.33     | Baseline impacted by minimal overhead |
| **Cirq** (TFQ)     | QSVM          | 99.0         | 0.02     | Balanced performance                 |
|                    | Classical SVM | 88.0         | 50.27    | Moderate integration overhead        |
| **Qiskit** (Aer)   | QSVM          | 45.0         | 0.00     | ComputeUncompute bottleneck          |
|                    | Classical SVM | 99.0         | 1057.90  | Full circuit simulation overhead     |

## 🧪 Methodology

- **Dataset**: MNIST binary classification (digits 0 vs 1)
  - 14,780 total samples → 100 training / 200 test (NISQ constraints)
  - PCA dimensionality reduction to 2 features → 2-qubit amplitude encoding
- **Quantum Feature Map**: ZZFeatureMap (2 repetitions) with RY rotations + CZ entanglement
- **Kernel**: Fidelity-based quantum kernel $K(x_i,x_j) = |\langle 0|U^\dagger(x_i)U(x_j)|0\rangle|^2$
- **Classical Baseline**: scikit-learn SVM with RBF kernel ($C=1.0$)
- **Hardware**: Google Colab runtime (identical hardware across all runs)

## 🚀 Setup & Usage

### Prerequisites
# Core dependencies
pip install scikit-learn numpy matplotlib pandas

# Quantum frameworks (install selectively based on notebook)
pip install pennylane          # For PennyLane notebook
pip install cirq               # For Cirq notebook
pip install qiskit             # For Qiskit notebook
pip install qiskit-machine-learning qiskit-aer qiskit-algorithms  # Qiskit ML stack
