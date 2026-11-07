# AdjointPIC-Lite
### A From-Scratch Computational Photonics and Inverse-Design Framework

**AdjointPIC-Lite** is an ongoing computational photonics project focused on developing a lightweight, from-scratch framework for electromagnetic simulation, guided-mode analysis, and gradient-based photonic inverse design.

The project aims to build the numerical foundations required for designing and optimizing photonic integrated circuit (PIC) components without relying entirely on existing commercial simulation and inverse-design packages.

The framework is being developed incrementally, beginning with numerical eigenmode analysis and finite-difference frequency-domain (FDFD) modeling, followed by guided-mode excitation, adjoint sensitivity analysis, and photonic device optimization.

**Project status: Work in Progress**

---

## 1. Project Objectives

The primary objective of AdjointPIC-Lite is to develop and integrate the essential numerical methods used in computational photonics and inverse design.

The main research and development objectives are:

- Develop electromagnetic simulation tools using the finite-difference frequency-domain (FDFD) method.
- Implement numerical eigenmode analysis for dielectric waveguides.
- Investigate mesh convergence and numerical accuracy.
- Implement perfectly matched layer (PML) absorbing boundary conditions.
- Develop guided-mode excitation and modal transmission analysis.
- Implement adjoint-based sensitivity analysis for efficient gradient computation.
- Explore gradient-based optimization of photonic device geometries.
- Investigate density filtering and material binarization for practical device design.
- Establish a foundation for future fabrication-aware photonic inverse design.

---

## 2. Development Roadmap

The framework is being developed through a sequence of interconnected computational modules.

| Module | Development Stage | Description |
|---|---|---|
| 01 | Numerical foundations | Effective-index calculation and mesh convergence |
| 02 | Electromagnetic modeling | 2D FDFD solver and PML boundary conditions |
| 03 | Mode analysis | Guided-mode excitation and transmission |
| 04 | Inverse design | Adjoint-based optimization of a 90° waveguide bend |
| 05 | Future development | Advanced adjoint optimization and fabrication-aware design |

Each module builds on the numerical methods and computational infrastructure established in the previous stages.

---

## 3. Implemented Modules

### Module 01 — Effective-Index Calculation and Mesh Convergence

**Objective:** Establish the numerical foundation for waveguide eigenmode analysis and investigate the accuracy of finite-difference discretization.

This module investigates the effective refractive index of a dielectric slab waveguide using a finite-difference eigenvalue formulation.

The numerical results are compared against an analytical solution to evaluate accuracy and convergence with respect to spatial mesh resolution.

**Implementation:**

- Define the refractive-index profile of a dielectric slab waveguide.
- Construct the finite-difference second-derivative operator.
- Solve the numerical eigenvalue problem to extract propagation constants and effective refractive indices.
- Calculate the analytical effective index of the fundamental guided mode.
- Compare numerical and analytical results across different mesh resolutions.
- Investigate the effect of subpixel permittivity averaging on material-interface representation.
- Evaluate numerical convergence behavior.

**Numerical parameters:**

| Parameter | Value |
|---|---:|
| Operating wavelength | 1.55 µm |
| Core refractive index | 3.48 |
| Cladding refractive index | 1.44 |
| Waveguide width | 0.5 µm |
| Computational domain | 4.0 µm |
| Grid resolutions | 100, 200, 400, 800, 1200 |

**Generated outputs:**

- Waveguide refractive-index profile.
- Fundamental guided-mode intensity distribution.
- Numerical versus analytical effective-index comparison.
- Relative effective-index error versus grid spacing.
- Standard versus subpixel permittivity averaging comparison.
- Numerical convergence analysis.

### Module 02 — 2D FDFD Solver and Perfectly Matched Layer (PML)

**Objective:** Develop the electromagnetic simulation infrastructure required for modeling optical field propagation in an open computational domain.

This module introduces a two-dimensional scalar finite-difference frequency-domain (FDFD) formulation and implements absorbing boundary conditions using perfectly matched layers.

**Implementation:**

- Construct the two-dimensional discrete Laplacian operator.
- Formulate the frequency-domain electromagnetic problem.
- Implement PML boundary treatment using complex coordinate stretching.
- Investigate the influence of PML parameters on numerical absorption.
- Compare field behavior with and without absorbing boundary treatment.
- Examine numerical convergence with respect to PML configuration.

**Main outputs:**

- Two-dimensional electromagnetic field distributions.
- Comparison of absorbing and non-absorbing boundary configurations.
- PML thickness convergence analysis.
- Numerical validation of the electromagnetic solver.

### Module 03 — Guided-Mode Excitation and Transmission Analysis

**Objective:** Integrate waveguide eigenmode analysis with the two-dimensional FDFD solver to investigate guided-mode excitation and optical transmission.

This module connects the previously developed waveguide eigenmode calculations and electromagnetic simulation infrastructure.

**Implementation:**

- Calculate guided modes using a one-dimensional eigenmode solver.
- Introduce guided-mode excitation into the two-dimensional FDFD simulation.
- Investigate electromagnetic field propagation through the computational domain.
- Analyze modal transmission at the output waveguide.
- Verify the numerical simulation workflow.

This module establishes the connection between eigenmode calculations, electromagnetic field propagation, and modal transmission analysis.

### Module 04 — Adjoint-Based Inverse Design of a 90° Waveguide Bend

**Objective:** Extend the electromagnetic simulation framework toward gradient-based photonic inverse design.

This module implements a numerical inverse-design workflow for a 90° silicon photonic waveguide bend.

The objective is to optimize the refractive-index distribution within a predefined design region to maximize optical power transfer between two orthogonal waveguide ports.

The implementation combines the FDFD electromagnetic solver, adjoint-based gradient computation, density filtering, and gradient-based optimization.

#### 4.1 Computational Domain and Geometry

- Define a two-dimensional computational domain.
- Establish fixed horizontal and vertical waveguide sections.
- Introduce a variable 20 × 20 design region at the waveguide corner.
- Define silicon and cladding material properties.

#### 4.2 Electromagnetic Simulation

- Implement the two-dimensional scalar FDFD solver.
- Construct the discrete Laplacian operator.
- Apply PML boundary treatment using complex coordinate stretching.
- Calculate input and output waveguide modes using a one-dimensional eigenmode solver.

#### 4.3 Inverse-Design Formulation

- Represent the design region using continuous density variables.
- Apply spatial density filtering to control the minimum feature scale.
- Use a hyperbolic tangent projection to gradually transform the continuous density distribution toward a binary material configuration.
- Formulate the optical transmission objective.

#### 4.4 Adjoint-Based Optimization

- Calculate normalized upward output modal power.
- Implement the adjoint method for efficient design-gradient computation.
- Verify the adjoint gradient implementation against finite-difference estimates.
- Optimize the design using the L-BFGS-B algorithm with bounded design variables.
- Apply projection continuation with increasing beta values.

#### 4.5 Design Evaluation and Validation

- Compare initial, optimized continuous, and binarized designs.
- Evaluate transmission and estimated reflection.
- Visualize electromagnetic field magnitude distributions.
- Examine optimization convergence history.
- Perform wavelength-dependent analysis at 1.50, 1.55, and 1.60 µm.

**Numerical parameters:**

| Parameter | Value |
|---|---:|
| Computational grid | 100 × 100 |
| Grid resolution | 0.04 µm |
| Design region | 20 × 20 |
| Silicon refractive index | 3.48 |
| Cladding refractive index | 1.44 |
| Central wavelength | 1.55 µm |
| PML thickness | 10 grid cells |
| Density filter radius | 0.12 µm |
| Projection continuation | β = 2, 5 |
| Optimization algorithm | L-BFGS-B |
| Maximum iterations per stage | 8 |

**Generated outputs:**

- Initial and optimized material-density distributions.
- Continuous and binarized design configurations.
- Electromagnetic field magnitude (|Ez|) distributions.
- Optimization convergence history.
- Wavelength-dependent transmission comparison.
- Numerical optimization results.

The notebook saves the numerical optimization results in:

`notebook08_bend_results.npz`

The saved data includes:

- Initial design variables and density distribution.
- Optimized continuous density distribution.
- Binarized density distribution.
- Optimization history.
- Wavelength sweep data.
- Initial, continuous, and binary transmission and reflection estimates.

Example for loading the saved results:

```python
import numpy as np

data = np.load("notebook08_bend_results.npz")

rho_optimized = data["rho_continuous"]
rho_binary = data["rho_binary"]
history = data["history"]
sweep = data["sweep"]
```

---

## 4. Technologies and Numerical Methods

The project uses Python and scientific computing libraries to implement the numerical simulation and optimization workflows.

**Programming and scientific computing:**

- Python
- NumPy
- SciPy
- Matplotlib

**Electromagnetic modeling:**

- Finite-Difference Frequency-Domain (FDFD)
- Finite-Difference Eigenvalue Methods
- Waveguide Eigenmode Analysis
- Perfectly Matched Layer (PML)
- Complex Coordinate Stretching

**Optimization and numerical analysis:**

- Adjoint Sensitivity Analysis
- Gradient-Based Optimization
- L-BFGS-B
- Finite-Difference Gradient Verification
- Density Filtering
- Hyperbolic Tangent Projection
- Mesh and Numerical Convergence Analysis

---

## 5. Installation and Requirements

The framework uses standard Python scientific computing libraries.

**Requirements:**

- Python 3.x
- NumPy
- SciPy
- Matplotlib
- Jupyter Notebook or JupyterLab

Install the required dependencies:

```bash
pip install numpy scipy matplotlib jupyter
```

---

## 6. How to Run

1. Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/AdjointPIC-Lite.git
```

2. Navigate to the project directory:

```bash
cd AdjointPIC-Lite
```

3. Install the dependencies:

```bash
pip install numpy scipy matplotlib jupyter
```

4. Launch Jupyter Notebook:

```bash
jupyter notebook
```

5. Open the desired notebook and execute its cells sequentially.

The notebooks can also be executed using JupyterLab or Google Colab.

**Note:** Some modules generate numerical data and visualization outputs during execution. The inverse-design notebook currently uses a CPU-friendly optimization configuration with a maximum of eight iterations per projection stage. Increasing the iteration count allows more extensive optimization runs.

---

## 7. Future Development

AdjointPIC-Lite is an ongoing project. The current implementation establishes the numerical foundations for further development toward a more comprehensive photonic inverse-design framework.

Planned development directions include:

**Advanced Adjoint Optimization**

- Extend and improve adjoint sensitivity analysis.
- Investigate gradient accuracy and numerical stability.
- Explore optimization of additional photonic components.

**Fabrication-Aware Inverse Design**

- Investigate minimum-feature-size constraints.
- Improve material binarization.
- Introduce fabrication-related constraints into the optimization process.
- Evaluate robustness against geometric variations.

**Electromagnetic Validation**

- Improve numerical convergence and simulation accuracy.
- Investigate more comprehensive electromagnetic formulations.
- Validate optimized structures using full-vector electromagnetic simulations.

**Framework Development**

- Improve code organization and modularity.
- Integrate simulation, sensitivity analysis, and optimization workflows.
- Develop reusable numerical components for different photonic devices.

---

## 8. Scope and Limitations

The current framework focuses on developing and validating two-dimensional scalar electromagnetic simulation and inverse-design methods.

The implemented waveguide bend optimization demonstrates an adjoint-based numerical workflow, but it should not be interpreted as a complete fabrication-ready photonic device.

Current limitations include:

- Two-dimensional scalar electromagnetic modeling rather than full-vector three-dimensional Maxwell simulation.
- Limited optimization iterations in the current CPU-friendly configuration.
- Further investigation required for fabrication-aware constraints and robust binarization.
- Additional full-wave validation required before considering optimized structures for practical fabrication.

These limitations define the scope of ongoing development rather than the final capabilities of the project.

---

## 9. Project Status

**AdjointPIC-Lite is actively under development.**

The project is being built incrementally, with each module extending the existing numerical infrastructure toward more advanced computational photonics applications.

The current development progression is:

**Waveguide Eigenmode Analysis → FDFD Electromagnetic Modeling → PML Boundary Conditions → Guided-Mode Excitation → Adjoint Sensitivity Analysis → Photonic Inverse Design**

The long-term goal is to establish a lightweight, modular, and extensible computational framework for the numerical design and optimization of photonic integrated circuit components.

---

## 10. License

A license has not yet been specified. Licensing information will be added when the project distribution terms are established.
