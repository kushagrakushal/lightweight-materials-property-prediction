# Reliable Composition-Based Property Prediction for Lightweight Inorganic Crystalline Materials

Code for the paper presented at **INDISCON 2026, MNIT Jaipur**.

**Authors:** Kushagra Kanodia, Bhavya Singla, Daksh Rautela, Alok Kumar Srivastava
(School of Artificial Intelligence, Amrita Vishwa Vidyapeetham, Faridabad)

## What this does

Predicts **density** and **formation energy per atom** of lightweight inorganic materials
(density <= 4 g/cm<sup>3</sup>) from chemical composition alone, using 132 MAGPIE descriptors,
and reports how much each prediction can be trusted:

1. **LOCO evaluation** - Leave-One-Cluster-Out testing (50 K-Means clusters) to measure generalisation to unseen chemistries.
2. **Conformal prediction** - distribution-free 90% prediction intervals.
3. **Applicability domain** - flags predictions on compositions far from the training data.
4. **Virtual screening + inverse design** - proposes new charge-balanced, solid-state lightweight compositions and checks them against the Materials Project.

## Results (held-out test set, 2,000 compounds)

| Target | Model | R<sup>2</sup> | MAE | RMSE |
|---|---|---|---|---|
| Density (g/cm<sup>3</sup>) | Linear Regression | 0.6259 | 0.3802 | 0.5397 |
| | Random Forest | 0.8202 | 0.2345 | 0.3741 |
| | **XGBoost** | **0.8241** | **0.2337** | **0.3700** |
| Formation energy (eV/atom) | Linear Regression | 0.8751 | 0.3310 | 0.4785 |
| | Random Forest | 0.9289 | 0.1968 | 0.3611 |
| | **XGBoost** | **0.9295** | **0.1930** | **0.3595** |

- **LOCO (pooled R<sup>2</sup>, XGBoost):** 0.667 for density, 0.890 for formation energy.
- **Conformal 90% intervals:** +/-0.590 g/cm<sup>3</sup> (92.6% coverage) and +/-0.472 eV/atom (89.5% coverage).
- **Applicability domain:** out-of-domain errors are 1.60-1.64x higher than in-domain errors.

## Repository contents

| File | Description |
|---|---|
| `lightweight_materials_prediction.ipynb` | Full pipeline, with outputs and figures |
| `lightweight_materials.csv` | 10,000 Materials Project entries (density <= 4 g/cm<sup>3</sup>) |
| `requirements.txt` | Python packages needed |

## How to run

```bash
pip install -r requirements.txt
jupyter notebook lightweight_materials_prediction.ipynb
```

Then run all cells. Figures are saved to a `figures/` folder.

Section 16 (checking candidates against the Materials Project) needs a free
[Materials Project API key](https://next-gen.materialsproject.org/api), set as an environment variable:

```bash
# Linux / macOS
export MP_API_KEY="your_key_here"
# Windows PowerShell
$env:MP_API_KEY="your_key_here"
```

Without a key, that section is skipped and everything else still runs.

## Data

`lightweight_materials.csv` is a snapshot of [Materials Project](https://next-gen.materialsproject.org/) data,
included for reproducibility and shared under the Materials Project's CC BY 4.0 terms.
For the latest data, query the Materials Project directly. If you use this data, please cite:

> A. Jain et al., "Commentary: The Materials Project: A materials genome approach to accelerating materials innovation," *APL Materials*, 1(1), 011002, 2013.
