# A learning log toward numerical relativity of neutron star mergers

>
>Preparation and familiarization with the MHDuet code and the Simflowny infrastructure for the 196/2026 pre-doctoral contract on the project: 'Magneto-Radiation-Hydrodynamics of Compact Object Collisions to Decode Gravity and Astrophysics.'

---

## 1. Why this repo

My goal with this preparation repository is to reflect what I have learned already about this project:
(1) understand the physics and methods used in the group's code MHDuet,
(2) get hands-on with its code-generation environment, Simflowny, and
(3) build a small Python demo of the stiff-term problem.

The thesis lines of the project are advanced methods for **neutrino transport**
and **non-ideal effects in MHD**. I am familiar with both topics,
although I have worked a lot more with the latter in my BSc thesis: "Magnetohydrodynamic waves in a bounded system using Dedalus".

%I would also like you to consider that this repository has been created in order to reflect my interest in the project and to show the preparation carried out for this offer.


## 2. My background (and the gaps)

- **BSc thesis:** MHD waves in a bounded system, simulated with **Dedalus**
  (spectral methods, Python).
- **MSc thesis:** detecting μ-distortion imprints in the CMB using **ILC / CILC**
  (signal extraction from noisy, multi-component data).
- **Strengths:** Python, numerical methods for PDEs, MHD wave physics, data analysis.
- **Gaps:** I have not yet used C++/Fortran, AMR frameworks,
  GPU programming, or run production-scale HPC jobs.

This repo is a self taught first step with the help of AI tools (Claude).

## 3. Literature study — **Done**

I read the two MHDuet papers closely and the abstracts/conclusions of related work.

**Palenzuela, Liebling & Miñano (2022), arXiv:2204.02721** — *Large Eddy Simulations of Magnetized Mergers of Neutron Stars with Neutrinos*.

**Palenzuela et al. (2025), arXiv:2510.13965** — *MHDuet: a high-order General Relativistic Radiation MHD code for CPU and GPU architectures*.

**Also skimmed (abstract level only):** A Simflowny-based high-performance 3D code for the generalized induction equation,
Turbulent magnetic field amplification in binary neutron star mergers,
Large Eddy Simulations of Magnetized Mergers of Black Holes and Neutron Stars, amongst many more.

## 4. Exploring Simflowny — **Done (up to compilation)**

Simflowny is a platform that generates parallel simulation code from a
high-level specification of the equations. I ran it locally with the official
Docker images (`iac3/simflowny:app` and `:datastore`) and opened the web IDE.

**Workflow I followed:**

1. Choose an existing example in the sample database: *Simple Wave Equation 2D*
   (problem: Gaussian profile, flat boundary conditions).
2. Validate the PDE discretization policy.
3. *Discretize Problem* → creates the discretized-problem document.
4. *Generate SAMRAI Code* → creates the code document.
5. Open a shell in the container and inspect the code in
   `/Simflowny/codeFiles/...`.

**What the generated code contains**
- C++ sources plus a `Makefile`, `problem.input` (run parameters), a `problem.pdf`
  summary and the exported Simflowny documents (`.xml`: model, problem,
  discretization policy/schema, spatial-operator discretization).
- `problem.input` exposes: end time, Gaussian-profile parameters, numerical
  dissipation factors, **Berger–Oliger sub-cycling**, and **adaptive mesh
  refinement** (regridding where the field φ exceeds a threshold, with a
  quintic interpolator).
- The `Makefile` links SAMRAI, HDF5, MPI and GSL.

**Where I stopped, and why**
- In my container, `make` was not available at first; after installing the
  basic build tools the compile advanced until it needed GSL, then failed on the
  missing SAMRAI headers. `/opt` was empty and no MPI was installed.
- Building OpenMPI, HDF5, Silo and SAMRAI from source was out of scope for my
  laptop and time budget, so I did **not** compile or run the generated code.
  Full-size runs would need a cluster anyway.

**What I learned**
- The separation between *physical model*, *problem*, *numerical discretization*
  and *target framework* is what lets one set of equations target different
  infrastructures (SAMRAI or AMReX/GPU).
- Parameters like sub-cycling and regridding thresholds are exposed as input
  options, as variables.
- A large part of real HPC work is dependency management and builds, not only
  numerics. As always!

## 5. Python demo: stiff relaxation in a hyperbolic system — **In progress**

**Goal:** a minimal 1D toy of what M1 transport and non-ideal MHD have in common.

Model (a P1/telegraph-type system, not full M1):

```
∂t E + ∂x F = 0
∂t F + (1/3) ∂x E = -κ F
```

- For small κ it propagates like a wave (free-streaming analogue).
- For large κ it should reduce to **diffusion** with coefficient 1/(3κ)
  (optically thick analogue).

**Planned tests**
1. Explicit scheme: show the timestep restriction as κ grows (instability when
   Δt exceeds the relaxation timescale).
2. IMEX scheme (implicit for the −κF term): stable at the CFL timestep for any κ.
3. Compare the large-κ solution with the analytic diffusion solution; check
   convergence order.
4. *(Optional)* Sod shock tube with a finite-volume scheme as a warm-up.

**Status:** [fill in: not started / code written / tests passing]

## 6. Environment

Conda environment `bns-relativity` (Python 3.11): NumPy, SciPy, SymPy,
Matplotlib, pandas, h5py, Numba, pytest, JupyterLab.

```bash
mamba create -n bns-relativity python=3.11 numpy scipy sympy matplotlib pandas h5py numba pytest jupyterlab -c conda-forge
mamba activate bns-relativity
```

## 7. Next steps

- Deeply learn C++ and the AMReX/SAMRAI infrastructure through MHDuet and Simflowny.
- Work on stiff-term schemes for neutrino transport and non-ideal MHD.
- Build experience with GPU/HPC workflows on a cluster.

## 8. Repo layout

```
.
├── README.md
├── notes/             # reading notes on the papers (optional)
├── simflowny/         # screenshots + short notes on the pipeline
└── stiff_relaxation/  # Python demo and tests (when ready)
```

## 9. References

- C. Palenzuela, S. Liebling, B. Miñano, *Large Eddy Simulations of Magnetized Mergers of Neutron Stars with Neutrinos*, arXiv:2204.02721 (Phys. Rev. D 105, 103020).
- C. Palenzuela et al., *MHDuet: a high-order General Relativistic Radiation MHD code for CPU and GPU architectures*, arXiv:2510.13965.
- Simflowny wiki documentation is not available at bitbucket.org/iac3/simflowny.
