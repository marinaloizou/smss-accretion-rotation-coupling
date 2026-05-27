# Baseline — Cantiello+2021 prescription

This directory contains the unmodified starting point for the project: the MESA `accreted_material_j` test suite (r24.08.1) configured for primordial-composition SMSS accretion as in Cantiello+2021.

Files inherited from `~/research/accreted_material_j_copiedversion/` as of project start. Do not modify these files — the whole point of this directory is to be the *exact* reproducible baseline against which the coupled-prescription modifications in `../coupled_prescription/` are compared.

## Files

| File | Purpose |
|------|---------|
| `inlist` | Top-level inlist; routes to inlist_zams or inlist_accreted_material_j |
| `inlist_zams` | Pre-MS → ZAMS phase |
| `inlist_zams_header` | star_job/eos/kap/controls header for ZAMS phase |
| `inlist_accreted_material_j` | Accretion phase with Cantiello+2021 prescription |
| `inlist_accreted_material_j_header` | Header for accretion phase |
| `inlist_pgstar` | pgstar visualization controls |
| `history_columns.list` | History output columns (rotation diagnostics mostly disabled — see TODO below) |
| `profile_columns.list` | Profile output columns |
| `src/run_star_extras.f90` | Custom Fortran: `accretor_adjust_mdot` implements the decoupled Cantiello+2021 accretion |
| `ORIGINAL_TEST_SUITE_README.rst` | Upstream MESA test suite README for reference |

## TODO before first production run

- [ ] Uncomment rotation diagnostics in `history_columns.list` (around line 848 onward — currently almost all rotation columns are commented out)
- [ ] Add to history_columns: `compactness_parameter`, `m4`, `mu4`, `surf_avg_omega_div_omega_crit`, `surf_avg_v_rot`, `surf_avg_v_crit`, `center_omega_div_omega_crit`, `total_angular_momentum`, `i_rot_total`
- [ ] Confirm MESA version is r24.08.1 (`./star --version` after `./mk`)
- [ ] Compile: `./clean && ./mk` (check for errors)
- [ ] One short test run (e.g., reduce `star_mass_max_limit` temporarily) to verify the setup runs end-to-end on this machine

## Running

```bash
./clean && ./mk    # compile run_star_extras.f90 against installed MESA
./rn               # execute the run
```

Output appears in `LOGS/` (ignored by git) and `photos/` (ignored by git).
