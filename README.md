Parabolic PDE Solver
A high-performance Parabolic Partial Differential Equation (PDE) Solver for solving and analysing the 1D Heat Equation over a spatial domain and time interval.

The solver supports multiple finite difference methods and includes advanced features such as internal heat sources, variable diffusion coefficients, non-homogeneous boundary conditions, and 3D visualisation tools.

To improve computational performance, the implementation utilises vectorised operations and sparse matrix techniques provided by SciPy.

Features
Numerical Methods
The solver implements the following finite difference schemes:

Forward Euler (FE)
Backward Euler (BE)
Crank-Nicolson (CN)
The codebase has been modularised into reusable subroutines, allowing users to independently construct, analyse, and solve the matrices associated with each numerical method.

Non-Homogeneous Boundary Conditions
Time-dependent Dirichlet boundary conditions can be applied through a user-defined function:

bcf(t)
This enables accurate modelling of systems whose boundary temperatures vary over time.

Internal Heat Sources
Internal heat generation can be specified using:

heat_source(x, t)
This allows heat sources to vary with both:

Position, $x \in [0, L]$
Time, $t \in [0, T]$
making the solver suitable for a wide range of thermal engineering problems.

Variable Diffusion Coefficients
Spatially varying thermal conductivity can be modelled using:

f_kappa(x)
This feature allows simulation of materials whose diffusion properties change throughout the computational domain.

Visualisation
Built-in visualisation tools enable users to analyse simulation results through interactive plots.

Available visualisations include:

Temperature variation over position and time
Internal heat source distribution
3D surface plots for transient heat transfer analysis
Accuracy Explorer
The Accuracy Explorer module evaluates the numerical accuracy of the implemented methods.

The module:

Computes truncation errors for FE, BE, and CN methods
Uses the L2-Norm as the error metric
Compares solution accuracy across different discretisation step sizes
Returns approximation errors over the spatial domain
This functionality helps verify convergence behaviour and compare the performance of different numerical schemes.

Project Structure
parabolic_pde_solver/
│
├── parabolic_pde_solver.py
├── accuracy_explorer.py
├── tests/
├── LICENSE.md
└── README.md
Applications
The solver can be used for:

Heat conduction simulations
Thermal system analysis
Engineering and scientific research
Numerical methods education
Finite difference method verification
Transient diffusion process studies
Getting Started
Prerequisites
Install the following dependencies before running the project.

Python 3
Download from:

https://www.python.org/downloads/

SciPy
pip install scipy
PyTest
pip install pytest
Installation
Clone the Repository
git clone https://github.com/hcbh96/parabolic_pde_solver.git
Navigate to the Project Directory
cd parabolic_pde_solver
Usage
Run the Heat Equation Solver
python3 parabolic_pde_solver.py
Run the Accuracy Analysis Tool
python3 accuracy_explorer.py
Running Tests
Execute all unit tests using:

pytest
Numerical Methods Summary
Method	Type	Stability
Forward Euler (FE)	Explicit	Conditionally Stable
Backward Euler (BE)	Implicit	Unconditionally Stable
Crank-Nicolson (CN)	Semi-Implicit	Unconditionally Stable
Future Enhancements
Potential future improvements include:

Support for two-dimensional PDEs
Adaptive mesh refinement
Additional boundary condition types
Performance benchmarking tools
GPU-accelerated computations
Author
Krushnakant Borude

Mechanical Engineer | Scientific Computing & Numerical Methods

Designed and developed the Parabolic PDE Solver and Accuracy Explorer framework for studying and analysing heat transfer problems using finite difference methods.

License
This project is licensed under the MIT License.

See the LICENSE.md file for details.