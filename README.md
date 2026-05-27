# Coupled Mass and Angular Momentum Accretion in Supermassive Stellar Evolution

MESA models of accreting Population III supermassive stars (SMSS) for direct-collapse black hole (DCBH) progenitor studies, with a focus on the coupling between mass accretion rate and accreted specific angular momentum.

This repository accompanies a forthcoming paper extending the [Cantiello+2021](https://ui.adsabs.harvard.edu/abs/2021ApJ...910...94C/abstract) framework with a physically self-consistent accretion prescription, where the angular momentum of accreted material is set by the same cosmological reservoir that sets the mass accretion rate, rather than being prescribed as an independent parameter.

## Motivation

The standard Cantiello+2021 prescription for SMSS accretion sets:
- Mass accretion rate: Ṁ_in = Ṁ₀ [1 − tanh(L*/L_Edd)]
- Specific angular momentum of accreted material: independent parameter (here, x_ctrl(1) × Keplerian j at photosphere)

This decoupling is unphysical — the gas falling onto an SMSS originates from a specific cosmological reservoir (e.g. the LWH core of [Wise+2019](https://ui.adsabs.harvard.edu/abs/2019Natur.566...85W/abstract)) with a well-defined specific angular momentum. Cranking Ṁ₀ while holding j_accreted fixed at a Keplerian fraction produces numerically unstable runs and unphysical evolution in the high-accretion regime.

This work:
1. Demonstrates the failure mode of the decoupled prescription systematically across a 2D parameter sweep in (Ṁ₀, x_ctrl(1))
2. Proposes a coupled prescription parameterized by the specific angular momentum of the accreting reservoir
3. Re-runs the parameter sweep and compares pre-collapse profile structure, stability, and outcomes
4. Releases full inlists, run_star_extras.f90, and post-processing scripts for community reproducibility

## Repository structure

```
.
├── baseline/                 # Cantiello+2021 prescription (starting point)
│   ├── inlist*               # MESA inlists as inherited from accreted_material_j test suite
│   ├── history_columns.list
│   ├── profile_columns.list
│   └── src/
│       └── run_star_extras.f90
│
├── coupled_prescription/     # Modified version with coupled mass-J accretion (in progress)
│
├── analysis/                 # Jupyter notebooks for post-processing and figures
├── figures/                  # Final paper figures
├── docs/                     # Methodology notes, parameter documentation
└── paper/                    # LaTeX source (if using same repo for paper)
```

## Methodology summary

- **MESA version**: r24.08.1
- **Initial seed**: 20 M☉, primordial composition (X = 0.7516, Y = 0.2484, Z ≈ 0)
- **Equation of state**: Skye ([Jermyn+2021](https://ui.adsabs.harvard.edu/abs/2021ApJ...913...72J/abstract))
- **Nuclear network**: approx21
- **Gravity**: post-Newtonian rescaling of G ([Paxton+2013](https://ui.adsabs.harvard.edu/abs/2013ApJS..208....4P/abstract))
- **Mass loss**: super-Eddington wind prescription ([Cantiello+2021](https://ui.adsabs.harvard.edu/abs/2021ApJ...910...94C/abstract))
- **Convection**: Henyey MLT, α_MLT = 2.5 (from converged α-paper sensitivity study)
- **Stopping criterion**: infall velocity threshold (under investigation)

## Reproducibility

Every parameter that affects the simulation is set explicitly in the inlists. Inherited MESA defaults are noted in `docs/methodology.md` and re-specified explicitly to insulate against version drift.

To reproduce a baseline run:

```bash
cd baseline/
./clean && ./mk    # compile run_star_extras.f90
./rn               # run
```

## Citation

Forthcoming paper — citation will appear here once the arXiv ID is assigned.

## Acknowledgments

This work is supported by NASA LISA Preparatory Science Grant 80NSSC24K0360, under CRESST II Cooperative Agreement 80GSFC24M0006. Simulations carried out on the Penelope cluster at Hofstra University.

## Author

Marina Loizou, Department of Physics and Astronomy, Hofstra University.
Advisor: Dr. Sarah Gossan.

## License

Code is released under the MIT License (see `LICENSE`). Pre-collapse profile data and figures are released under CC-BY 4.0.
