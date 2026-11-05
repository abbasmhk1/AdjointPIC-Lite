# AdjointPIC-Lite

### From-scratch scalar FDFD simulation, discrete adjoints, and photonic inverse design.

> **Status: Work in progress — early numerical foundations.** This repository currently contains **three connected Jupyter notebooks**, not three separate projects. They form a progressive development sequence: **waveguide eigenmodes → absorbing boundaries → guided-mode excitation and transmission**. Discrete adjoints and inverse design are the **long-term goals** of AdjointPIC-Lite and are **not yet included in this repository snapshot**. The code is exploratory and will be cleaned, tested, and expanded gradually.

## Overview

**AdjointPIC-Lite** is an independent computational-photonics project to build a two-dimensional **scalar finite-difference frequency-domain (FDFD)** simulation and optimization workflow using Python and free scientific libraries. The project emphasizes understanding the numerical methods, checking them against reference solutions, and documenting limitations rather than relying on commercial simulation tools.

The three notebooks are **stages of the same solver**:

```text
Slab-waveguide eigenmode + effective index
                  ↓
       2D FDFD with PML
                  ↓
 Guided-mode source + modal transmission
                  ↓
    Planned: discrete adjoint gradients
                  ↓
    Planned: photonic inverse design
```

## Repository structure

```text
AdjointPIC-Lite/
├── README.md
├── requirements.txt
├── .gitignore
└── notebooks/
    ├── 01_effective_index_mesh_convergence.ipynb
    ├── 02_pml_validation.ipynb
    └── 03_guided_mode_excitation_transmission.ipynb
```

## Current notebooks — one connected workflow

### 01 · Effective-index mesh convergence

[`notebooks/01_effective_index_mesh_convergence.ipynb`](notebooks/01_effective_index_mesh_convergence.ipynb)

**Purpose:** Establish a numerical guided mode before attempting a 2D propagation simulation. The notebook constructs a transverse finite-difference eigenvalue problem for a dielectric slab, computes effective indices and eigenvectors, compares the fundamental mode against a slab dispersion relation, studies mesh refinement, and examines **subpixel permittivity averaging** at material interfaces.

**Role in the overall project:** The calculated transverse mode profile and propagation constant provide the basis for exciting and measuring guided waves in Notebook 03.

**Representative development results** (from the notebook experiments, not a comprehensive validation suite):

- Fundamental mode: numerical \(n_{\mathrm{eff}} \approx 3.275044\), analytical \(n_{\mathrm{eff}} \approx 3.275615\), relative error \(\approx 1.74\times10^{-4}\) for a 400-point example.
- In the 400-point interface study, subpixel averaging reduced the reported relative effective-index error from \(1.74\times10^{-4}\) to \(4.07\times10^{-5}\).

### 02 · Perfectly matched layers (PML)

[`notebooks/02_pml_validation.ipynb`](notebooks/02_pml_validation.ipynb)

**Purpose:** Introduce absorbing boundaries for finite computational domains. The notebook implements complex coordinate-stretching profiles, assembles a sparse 2D FDFD operator, solves a point-source problem, compares fields with and without PML against a larger-domain numerical reference, and tests PML thickness and reference-domain sensitivity.

**Role in the overall project:** The PML operator provides an approximate open-domain boundary treatment for the guided-wave simulations in Notebook 03.

**Representative development result:** In one point-source comparison, the reported relative field difference against a larger-domain reference was approximately \(7.07\times10^{-3}\) with PML versus \(1.47\) without PML. This is a **specific reference-dependent field error**, not a universal PML reflection coefficient.

### 03 · Guided-mode excitation and transmission

[`notebooks/03_guided_mode_excitation_transmission.ipynb`](notebooks/03_guided_mode_excitation_transmission.ipynb)

**Purpose:** Combine the eigenmode and PML foundations to launch a guided mode into a 2D dielectric waveguide. The notebook investigates the discrete longitudinal propagation constant, localized/equivalent modal sources, forward/backward modal decomposition, power ratios, and source-consistency diagnostics.

**Role in the overall project:** This is the bridge from a passive field solver to a simulator with a controllable input mode and a measurable output objective—prerequisites for later adjoint optimization.

**Representative development results:** A corrected straight-waveguide test reported a forward modal-power ratio close to 1 between interior monitoring planes, a local backward/forward modal-power ratio of approximately \(9.54\times10^{-6}\), and a source-equivalence discrepancy of approximately \(4.94\times10^{-9}\). These checks do **not** constitute a complete energy-conservation or device-accuracy proof.

**Important reproducibility note:** Notebook 03 is currently a **development log**. It contains earlier point-source/PML experiments and debugging cells that redefine grid dimensions (`nx`, `ny`) and related state. It should **not** be treated as a clean, run-all production notebook. The next maintenance priority is to split reusable functions from diagnostics and make execution order deterministic.

## How to run

### Google Colab (recommended)

1. Open [Google Colab](https://colab.research.google.com/).
2. Upload a notebook from the `notebooks/` folder, or open it from your GitHub repository.
3. Use a standard Python CPU runtime; no GPU, lab equipment, or commercial solver is required.
4. Start with **01**, then **02**. Read the reproducibility warning above before running **03**.

The notebooks are intended as sequential **conceptual stages**, but each contains its own exploratory code; they do not yet form an importable Python package or an automated end-to-end pipeline.

### Local environment

```bash
python -m venv .venv
source .venv/bin/activate   # Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -r requirements.txt
jupyter lab
```

## Numerical methods and scientific scope

- Sparse finite-difference operators and sparse linear algebra using **NumPy** and **SciPy**.
- Dielectric slab eigenmodes and effective-index validation.
- Two-dimensional **scalar** Helmholtz/FDFD simulations with complex-stretched PML.
- Guided-mode excitation, discrete phase propagation, and modal overlap measurements.
- Visualization and convergence diagnostics using **Matplotlib**.

**Limitations:** This is a scalar 2D model, **not** a full-vector 3D Maxwell simulation or a fabrication-validated silicon photonic device. A small linear-system residual does not guarantee physical accuracy. Reported transmission quantities depend on mode normalization, monitor placement, mesh resolution, and boundary treatment. No fabricated or experimentally measured performance is claimed.

## Development roadmap

The repository will be extended **incrementally**, with each addition documented and checked before being presented as a validated result.

- [x] Slab-waveguide eigenmode calculation and effective-index comparison
- [x] Mesh-refinement and subpixel-interface experiments
- [x] PML implementation and reference-domain experiments
- [x] Exploratory guided-mode excitation and modal decomposition
- [ ] Refactor Notebook 03 into a reproducible, clean-run notebook
- [ ] Add automated eigenmode, PML, and source validation tests
- [ ] Consolidate reusable FDFD code into a small Python module
- [ ] Publish discrete-adjoint gradients with finite-difference checks
- [ ] Add gradient-based dielectric topology optimization
- [ ] Add fabrication-aware filtering, projection, and binary evaluation
- [ ] Benchmark nontrivial photonic devices and numerical robustness

The title **AdjointPIC-Lite** describes the **intended complete framework**; the current release documents its numerical foundations.

## Project status and citation

**Early development / work in progress.** Notebook outputs and interfaces may change. Please refer to the specific notebook and commit when discussing a result. This repository is an independent computational learning and research effort, not a peer-reviewed or experimentally validated software release.

## License

No license has been selected yet. Public availability does not by itself grant permission to reuse or redistribute the code; a license may be added in a future release.
