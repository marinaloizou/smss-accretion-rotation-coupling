# Methodology notes

Working document. Fill in as decisions are made.

## Physical setup

- **Seed mass**: 20 M☉
- **Composition**: primordial — X = 0.7516, Y = 0.2484, Z ≈ 1e-9
- **Initial temperature**: T = 10⁵ K (to preclude initial nuclear burning)
- **Nuclear network**: `approx21` (revisit if late-stage central conditions matter for collapse readiness)
- **EOS**: Skye (Jermyn+2021)
- **Opacity**: GS98 (revisit for primordial — may want low-Z appropriate tables)

## Gravity

Post-Newtonian rescaling of G following Paxton+2013:

```
G → G · (1 + P/(ρc²) + 4πPr³/(M_r c²)) · (1 − 2GM_r/(rc²))⁻¹
```

This captures GR-instability onset approximately. **Limitation**: this is a non-rotating GR correction; full Kerr-style treatment is left to future work / handoff to GRMHD collaborators.

## Convection

- `MLT_option = 'Henyey'` — confirmed in α-paper to be more stable than TDC for this regime
- `mixing_length_alpha = 2.5` — middle of the (2.0, 2.5, 3.0) range tested in the α paper, which showed minimal sensitivity to α in the SMSS regime

## Mass loss

Super-Eddington wind prescription from Cantiello+2021:

```
Ṁ_out = −(L*/v_esc²) · [1 + tanh((L* − L_Edd)/(0.1 L_Edd))]
```

## Accretion — BASELINE (Cantiello+2021, decoupled)

Mass accretion rate:
```
Ṁ_in = Ṁ_0 · [1 − tanh(L*/L_Edd)]
```
with `Ṁ_0 = 5×10⁻² M☉/yr` (from LWH core, Wise+2019).

Specific angular momentum of accreted material:
```
j_accreted = x_ctrl(1) · sqrt(G · M_* · R_photosphere)
```
applied each timestep in `run_star_extras.f90:accretor_adjust_mdot`. Default x_ctrl(1) = 0.1.

**Problem**: Ṁ_in and j_accreted are independently parameterized. Cranking Ṁ_0 without scaling j produces unphysical runs (observed: meeting notes Dec 8 2025).

## Accretion — COUPLED PRESCRIPTION (this paper)

*To be implemented. Sketch:*

Parameterize the accreting reservoir by a single physical specific angular momentum `j_res` (set by the cosmological IC, e.g. the LWH core angular momentum profile from Wise+2019). Both Ṁ_in and j_accreted derive from `j_res` and the current star state, eliminating the artificial decoupling.

Concrete form TBD — options include:
1. Direct: j_accreted = j_res (fixed), Ṁ_in modulated by L/L_Edd as before
2. Bondi-Hoyle motivated: both Ṁ_in and j_accreted scale with reservoir properties and stellar gravitational sphere of influence
3. Reservoir-depletion: j_res evolves as accretion proceeds, draining the cosmological reservoir

Decision deferred to internship phase 2 (mid-July).

## Stopping criterion

**Currently**: infall velocity threshold. Value to be confirmed — meeting notes show 100 km/s used at one point, poster claims 10⁴ km/s. Sort out the inconsistency.

**Alternative criteria to investigate** (from meeting notes and Naseri+):
- Newtonian potential threshold: GM_enc(r)/(r c²) exceeds some value
- T/W rotational energy ratio (Naseri+ UIUC threshold)
- Mass-weighted ⟨Γ_1⟩_M crossing 4/3 in the inner mass

## Parameter sweep design

**Baseline grid** (Cantiello+2021 prescription, decoupled):

| Run ID | Ṁ_0 [M☉/yr] | x_ctrl(1) [j/j_Kepler] | Expected outcome |
|--------|-------------|------------------------|------------------|
| B-00   | 1e-2        | 0.0  | non-rotating control |
| B-01   | 1e-2        | 0.1  | nominal |
| B-02   | 1e-2        | 0.4  | high-j |
| B-10   | 5e-2        | 0.0  | nominal Ṁ, no rotation |
| B-11   | 5e-2        | 0.1  | nominal both |
| B-12   | 5e-2        | 0.4  | high-j, nominal Ṁ |
| B-20   | 1e-1        | 0.0  | high Ṁ, no rotation |
| B-21   | 1e-1        | 0.1  | "the mess" predicted here |
| B-22   | 1e-1        | 0.4  | "the mess" predicted here |

**Coupled grid**: same 3×3 with the coupled prescription replacing the decoupled one. Compare outcomes pairwise.

## Diagnostics to record per run

- HR track (log Teff, log L)
- M_*(t)
- log ρ_c(t), log T_c(t)
- Total angular momentum J(t)
- Surface ω/ω_crit (t)
- Final pre-collapse profile (full)
- At collapse trigger: compactness, Γ_1 profile, T/W, j(m)
