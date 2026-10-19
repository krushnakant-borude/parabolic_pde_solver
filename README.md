# Parabolic PDE Solver

[![Python 3](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![SciPy](https://img.shields.io/badge/SciPy-Sparse%20Matrices-8CAAE6?logo=scipy&logoColor=white)](https://scipy.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A study and selected modification of an existing open-source Python finite-difference codebase for one-dimensional parabolic partial differential equations, focused on the transient heat equation. The repository contains code for three time-integration schemes, boundary conditions, internal heat generation, variable diffusion coefficients, numerical error analysis, and 3D visualisation; those codebase features are not a claim that I independently authored each component.

## Attribution and Personal Contribution

The starting point for this repository is the [open-source project by Harry Blakiston Houston](https://github.com/hcbh96/parabolic_pde_solver), released under the MIT License. My contribution has been to work through the existing implementation and modify selected parts while studying the Forward Euler, Backward Euler, and Crank-Nicolson schemes and the existing error-comparison workflow across spatial and time-step refinements.

I did **not** independently develop the complete solver, create the accuracy explorer from scratch, or write the pre-existing automated test suite. Feature descriptions below describe code available in this repository; they should not be read as a claim that every component is my original work. Please consult the upstream repository for the original implementation and history.

## Overview

The project solves problems of the form

$$
\frac{\partial u}{\partial t}
=
\frac{\partial}{\partial x}
\left(\kappa(x)\frac{\partial u}{\partial x}\right)
+q(x,t),
\qquad 0 \le x \le L,\quad 0 \le t \le T,
$$

where:

- $u(x,t)$ is the solution, such as temperature;
- $\kappa(x)$ is a constant or spatially varying diffusion coefficient;
- $q(x,t)$ is an optional internal heat source;
- boundary values may vary with time.

The available implementation uses vectorised operations and SciPy sparse matrices to reduce memory use and improve computational efficiency.

## Key Features

- **Forward Euler (FE):** explicit and computationally inexpensive, subject to a stability restriction.
- **Backward Euler (BE):** implicit and robust for larger time steps.
- **Crank-Nicolson (CN):** second-order accurate in time for smooth solutions.
- **Time-dependent Dirichlet boundaries:** supplied through `bcf(t)`.
- **Internal heat sources:** supplied through `heat_source(x, t)`.
- **Variable diffusion coefficients:** supplied through `f_kappa(x)`.
- **Sparse matrix formulation:** efficient assembly and solution of linear systems.
- **3D visualisation:** inspect the solution or heat-source distribution over space and time.
- **Accuracy exploration:** compare FE, BE, and CN errors using an L2-norm-based measure.

## Numerical Methods

| Method | Time discretisation | Typical accuracy | Stability |
|---|---|---:|---|
| Forward Euler | Explicit | First order | Conditionally stable |
| Backward Euler | Implicit | First order | Unconditionally stable for the standard linear diffusion problem |
| Crank-Nicolson | Implicit, centred | Second order | Unconditionally stable for the standard linear diffusion problem |

> **Forward Euler stability:** for the constant-coefficient 1D heat equation, choose the grid so that the diffusion number $r=\kappa\Delta t/\Delta x^2$ satisfies the method's stability requirement, commonly $r\leq 1/2$.

## Repository Structure

```text
parabolic_pde_solver/
├── parabolic_pde_solver.py       # PDE solver and visualisation
├── accuracy_explorer.py          # Error and convergence exploration
├── test_parabolic_pde_solver.py  # Automated tests
├── README.md
└── LICENSE
```

## Requirements

- Python 3
- NumPy
- SciPy
- Matplotlib
- pytest, for running tests

Install the required packages:

```bash
python3 -m pip install numpy scipy matplotlib pytest
```

## Installation

Clone the repository and enter the project directory:

```bash
git clone https://github.com/krushnakant-borude/parabolic_pde_solver.git
cd parabolic_pde_solver
```

Optional but recommended: create an isolated virtual environment.

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install --upgrade pip
python3 -m pip install numpy scipy matplotlib pytest
```

On Windows, activate the environment with:

```powershell
.venv\Scripts\activate
```

## Usage

Run the PDE solver and display the heat-equation solution:

```bash
python3 parabolic_pde_solver.py
```

Run the accuracy explorer to inspect truncation-error behaviour:

```bash
python3 accuracy_explorer.py
```

## Defining a Problem

The solver accepts functions that describe the physical model.

### Boundary conditions

Use `bcf(t)` to return the Dirichlet boundary values at time `t`.

```python
def bcf(t):
    left_temperature = 100.0
    right_temperature = 20.0 + 5.0 * t
    return left_temperature, right_temperature
```

### Internal heat source

Use `heat_source(x, t)` for a source that varies in space and time.

```python
import numpy as np


def heat_source(x, t):
    return 10.0 * np.exp(-50.0 * (x - 0.5) ** 2) * np.exp(-t)
```

### Variable diffusion coefficient

Use `f_kappa(x)` to represent a non-uniform material.

```python
def f_kappa(x):
    return 1.0 + 0.5 * x
```

> Function signatures and return shapes must match the public functions documented in `parabolic_pde_solver.py`.

## Accuracy Explorer

`accuracy_explorer.py` compares the numerical schemes across selected spatial or temporal step sizes. It estimates error with an L2-norm-based metric and can be used to:

- examine convergence as the grid is refined;
- compare FE, BE, and CN behaviour;
- identify configurations affected by stability or discretisation error;
- visualise truncation-error trends.

The accuracy explorer is intended as an analysis utility. Validate its reference solution, norm definition, and refinement strategy before using its results for formal verification.

## Running Tests

Run all tests from the repository root:

```bash
python3 -m pytest -v
```

## Applications

- Transient heat conduction in a one-dimensional bar
- Diffusion with spatially varying material properties
- Heating problems with distributed, time-dependent sources
- Comparison of explicit and implicit finite-difference schemes
- Teaching, verification, and experimentation in numerical PDE methods

## Limitations

- The current implementation is intended for a one-dimensional spatial domain.
- The documented boundary treatment is Dirichlet-based.
- Forward Euler requires an appropriately small time step.
- Accuracy depends on spatial and temporal resolution, coefficient regularity, and the selected method.

## Contributing

Contributions are welcome. To propose an improvement:

1. Fork the repository.
2. Create a branch: `git checkout -b feature/your-feature`.
3. Add or update tests for the change.
4. Run `python3 -m pytest -v`.
5. Commit the change and open a pull request.

Useful contribution areas include additional boundary-condition types, performance benchmarks, expanded verification cases, and two-dimensional support.

## Maintainer

**Krushnakant Borude**  
Mechanical Engineer interested in scientific computing, numerical methods, and engineering simulation.

## Licence

This project is distributed under the MIT Licence. See [`LICENSE`](LICENSE) for details.
