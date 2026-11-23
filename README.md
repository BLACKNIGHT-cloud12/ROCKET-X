# ROCKET-X

**A Multiphysics Computational Framework for Liquid Rocket Propulsion**

---

## 1. Overview
ROCKET-X is an independent computational aerospace engineering and scientific computing platform designed to model and simulate liquid rocket propulsion systems. 

The framework is structured as an incremental, multi-fidelity workflow:
$$\text{Thermochemistry (0D)} \longrightarrow \text{Quasi-1D Compressible Gas Dynamics} \longrightarrow \text{2D/3D Compressible CFD} \longrightarrow \text{Thermal & Structural Multiphysics}$$

> **Verification & Validation Standard:**  
> ROCKET-X distinguishes strictly between:
> - **Code Verification:** Mathematical correctness and order of accuracy against analytical solutions.
> - **Solution Verification:** Iterative convergence, mass/energy conservation, and Grid Convergence Index (GCI).
> - **Physical Validation:** Direct comparison against authoritative aerospace benchmarks (e.g., NASA/JPL Back & Massier supersonic nozzle experimental data, NASA RP-1311 CEA references).

---

## 2. Framework Architecture

```
                    ROCKET-X WORKFLOW
                           │
             Configuration & Input Layer
              (Propellants, Pc, MR, Geometry)
                           │
         ┌─────────────────┴─────────────────┐
         ▼                                   ▼
  Thermochemistry Core             Independent 1D Solver
   (NASA CEA / Cantera)             (Quasi-1D Euler / Shocks)
         │                                   │
         └─────────────────┬─────────────────┘
                           ▼
                  Compressible CFD
             (OpenFOAM rhoCentralFoam)
                           │
         ┌─────────────────┴─────────────────┐
         ▼                                   ▼
   Non-Reacting Nozzle               Reacting Flow Feasibility
   (Shock/Expansion GCI)              (Reduced / Tabulated Kinetics)
                           │
                           ▼
                 Gas-Side Heat Transfer
                  (Bartz & Conjugate HT)
                           │
                           ▼
                 Thermal-Structural FEA
                  (CalculiX / FEniCS)
                           │
                           ▼
             Surrogate Modelling & Design Opt
                 (PyTorch on Kaggle GPU)
```

---

## 3. Directory Layout

- `thermochemistry/`: Python interfaces and parsers for NASA CEA & Cantera thermochemical calculations.
- `reduced_order_models/`: Self-implemented 0D chamber balance and quasi-1D compressible nozzle solvers.
- `cfd/`: OpenFOAM cases (`rhoCentralFoam`, `reactingFoam`), mesh generation scripts, and post-processing tools.
- `data/`: Versioned experimental benchmark data (JPL nozzle profiles, CEA references).
- `docs/`: Governing equations, assumption registers, and verification reports.
- `tests/`: Automated unit and integration tests using `pytest`.
- `environment/`: Environment specifications and reproducibility manifests.

---

## 4. Getting Started

### Prerequisites
- Linux / WSL2 (Ubuntu 24.04 LTS)
- Conda environment: `rocketx` (Python 3.12)
- OpenFOAM v2406
- ParaView

### Installation
```bash
conda activate rocketx
pip install -r environment/requirements.txt
```

### Running Tests
```bash
pytest tests/
```

---

## 5. Research Identity & Integrity
- All third-party tools (NASA CEA, OpenFOAM, Cantera) are explicitly identified as external dependencies.
- No computational prediction is claimed as validated without citing reference experimental uncertainty and numerical convergence evidence.
