# Analysis notebooks

Jupyter notebooks for post-processing MESA output. Use `mesa_reader` for parsing history/profile files.

Suggested notebooks (to be created):

- `01_hr_diagram.ipynb` — HR tracks across the parameter grid
- `02_mass_evolution.ipynb` — M(t) and Ṁ(t) for all runs
- `03_central_conditions.ipynb` — ρ_c(t), T_c(t), Γ_1(t)
- `04_angular_momentum.ipynb` — total J(t), j(m) profiles, surface ω/ω_crit
- `05_pre_collapse_profiles.ipynb` — final profile structure across runs
- `06_compactness_and_TW.ipynb` — collapse-readiness diagnostics
- `07_baseline_vs_coupled.ipynb` — head-to-head comparison

## Dependencies

```bash
pip install --user mesa_reader matplotlib numpy pandas jupyter scipy
```
