# W-ANN: Ultra-fast screening of W-based alloys via targeted machine learning

Data repository for the paper:

> **Ultra-fast screening of W-based alloys via targeted machine learning**  
> K. Sidnov, E. Smirnova, D. Konov, et al.  
> Manuscript in preparation / submitted, 2026.

This repository contains the EMTO-CPA calculated dataset and the neural-network predicted data used for high-throughput screening of multicomponent BCC tungsten-based alloys for nuclear reactor applications.

**First release:** only calculated and predicted data are provided.  
Training code, model weights, and feature-generation scripts will be added in future versions.

---

## Contents

```
**Planned structure:*
.
├── README.md
├── LICENSE
├── CITATION.cff
└── data/
    ├── calculated/
    │   └── emto_cpa_dataset.csv
    └── predicted/
        ├── screening_predictions.csv.zip
        └── top20_candidates.csv
```

### `data/calculated/emto_cpa_dataset.csv`

EMTO-CPA calculated properties for **1,203 BCC W-based alloys** (2–4 components).

- 990 binary and ternary alloys from the W–Cr–Hf–Mo–Nb–Re–Ta–Ti–V–Zr system with W ≥ 50 at.%
- 213 quaternary alloys from the Cr–Mo–Ta–Ti–V–W system with W = 25–80 at.%
- Calculations: EMTO-CPA, GGA, T = 0 K.
- Note: for the Rule-of-Mixtures comparison in the paper, Hf-containing compositions were excluded, giving a 993-composition subset.

**Columns:**

| Column | Description | Units |
|---|---|---|
| `composition` | Compact composition string, e.g. `W0.65Ta0.35` | — |
| `W`, `Cr`, `Hf`, `Mo`, `Nb`, `Re`, `Ta`, `Ti`, `V`, `Zr` | Atomic fractions | at.% |
| `n_components` | Number of components | — |
| `system` | Chemical system, e.g. `W-Ta-Ti` | — |
| `B_GPa` | Bulk modulus | GPa |
| `C11_GPa` | Elastic constant \(C_{11}\) | GPa |
| `C12_GPa` | Elastic constant \(C_{12}\) | GPa |
| `C44_GPa` | Elastic constant \(C_{44}\) | GPa |
| `Cprime_GPa` | \(C' = (C_{11} - C_{12})/2\) | GPa |
| `G_GPa` | Shear modulus | GPa |
| `E_GPa` | Young's modulus | GPa |
| `a_Angstrom` | Lattice parameter | Å |

### `data/predicted/screening_predictions.csv.zip`

Neural-network predictions for **> 831,000 virtual BCC W-based compositions** (2–6 components).

- Concentration step: 5 at.%
- W content: ≥ 20–45 at.% depending on the number of components
- Elements: Cr, Hf, Mo, Nb, Re, Ta, Ti, V, Zr
- File is gzip-compressed due to size.

**Columns:**

| Column | Description | Units |
|---|---|---|
| `composition` | Compact composition string | — |
| `W`, `Cr`, `Hf`, `Mo`, `Nb`, `Re`, `Ta`, `Ti`, `V`, `Zr` | Atomic fractions | at.% |
| `n_components` | Number of components | — |
| `B_GPa` | Predicted bulk modulus | GPa |
| `C11_GPa` | Predicted \(C_{11}\) | GPa |
| `C12_GPa` | Predicted \(C_{12}\) | GPa |
| `C44_GPa` | Predicted \(C_{44}\) | GPa |
| `Cprime_GPa` | Predicted \(C' = (C_{11} - C_{12})/2\) | GPa |
| `G_GPa` | Predicted shear modulus | GPa |
| `E_GPa` | Predicted Young's modulus | GPa |
<!-- | `a_Angstrom` | Predicted lattice parameter | Å |
| `G_over_B` | Pugh ratio \(G/B\) | — |
| `Pc_GPa` | Cauchy pressure \(P_c = C_{12} - C_{44}\) | GPa |-->

### `data/predicted/top20_candidates.csv`

Top 20 promising compositions selected by the screening criteria:

- \(G/B \le 0.5\)
- \(P_c \ge 40\) GPa
- \(E \ge 300\) GPa

**Columns:**

| Column | Description | Units |
|---|---|---|
| `composition` | Compact composition string | — |
| `G_over_B` | Pugh ratio \(G/B\) | — |
| `Pc_GPa` | Cauchy pressure \(P_c\) | GPa |
| `E_GPa` | Predicted Young's modulus | GPa |
| `B_GPa` | Predicted bulk modulus | GPa |

---

## Units and conventions

- Concentrations: atomic percent (at.%).
- Elastic moduli and elastic constants: GPa.
- Lattice parameter: Å.
- \(C' = (C_{11} - C_{12})/2\).
- \(P_c = C_{12} - C_{44}\).
- Missing values are encoded as empty fields or `NaN`.

---

## Usage

```python
import pandas as pd

# Calculated EMTO-CPA data
calc = pd.read_csv("data/calculated/emto_cpa_dataset.csv")

# Predicted screening data
pred = pd.read_csv("data/predicted/screening_predictions.csv.gz")

# Top candidates
top20 = pd.read_csv("data/predicted/top20_candidates.csv")

print(calc.head())
print(pred.head())
print(top20.head())
```

---

## Validation summary

The multi-task neural network was validated using:

- 10-fold cross-validation
- concentration-associated validation
- one-element-out validation
- one-system-out validation
- extrapolation in the number of components
- hold-out validation

In the interpolation regime, \(R^2 > 0.99\) for all target properties.  
The one-element-out test identified rhenium as the most difficult element for extrapolation. Adding 1–5% of Re-containing data to the training set restored accuracy, confirming that the error was caused by out-of-distribution descriptors rather than an architectural limitation.

---

## Citation

If you use these data, please cite:

```bibtex
@article{Sidnov2026WANN,
  title   = {Ultra-fast screening of W-based alloys via targeted machine learning},
  author  = {Sidnov, K. and Smirnova, E. and Konov, D. and others},
  journal = {Manuscript in preparation},
  year    = {2026},
  note    = {Data repository: W-ANN}
}
```

A `CITATION.cff` file will be added/updated upon publication.

---

## License

The data in this repository are released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license.  
See `LICENSE` for details.

---

## Acknowledgment

This work was supported by the Russian Science Foundation, Project No. 25-22-00803.

---

## Contact

For questions about the data or the paper, please contact the corresponding author.  
*(Add corresponding author email here.)*
