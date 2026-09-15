# Molecular Odor Prediction and Multidimensional Validation Study Based on a Two-Level Stacking Ensemble Model

This repository preserves the molecular dataset used for experiments on odor-profile prediction with a two-level stacking ensemble. It is intended to make data inspection, feature engineering, validation design, and follow-up modeling easier for research collaborators.

## Contents

| File | Description |
|---|---|
| [`pure_holographic_971_dataset.csv`](pure_holographic_971_dataset.csv) | 971 molecular records, encoded as 1,079 columns |
| `SMILES` | Molecular structure string used as the structural identifier |
| `Flavor_Profile` | Odor-profile target label |
| `FG_*` | Functional-group descriptors |
| `Topo_*`, `Phys_*`, `Charge_*` | Topological, physicochemical, and charge-related descriptors |
| `MFP_2D_*` | Two-dimensional molecular fingerprint features |

The supplied target column contains nine odor-profile classes: Fruit, Floral, Odorless, Sulfur, Green, Spice, Herbal, Nuts, and Caramellic. The class distribution is not uniform, so accuracy alone is insufficient for evaluation; report macro-F1, balanced accuracy, per-class recall, and a confusion matrix when appropriate.

## Loading and quality checks

```python
import pandas as pd

df = pd.read_csv("pure_holographic_971_dataset.csv")
print(df.shape)
print(df["Flavor_Profile"].value_counts(dropna=False))
print(df["SMILES"].isna().sum(), df["Flavor_Profile"].isna().sum())
```

Before modeling, preserve the original CSV, check duplicate SMILES and duplicate rows, determine how missing descriptors are handled, and remove the target and identifier fields from the predictor matrix. Any scaling, feature selection, fingerprint reduction, class weighting, or resampling should be fitted within the training folds to avoid leakage.

## Recommended validation protocol

Use a fixed, documented random seed and a stratified train/validation/test split (or repeated stratified cross-validation). If structurally similar molecules may occur in multiple folds, consider a scaffold-aware split and report it separately. The two-level stacking design should generate out-of-fold predictions for the meta-learner; fitting the meta-learner on in-sample base-model predictions will overstate performance.

## Reproducibility and responsible use

This release contains the supplied data file and documentation; it does not assert that a particular preprocessing pipeline or model is universally optimal. Record software versions, descriptor-generation settings, split strategy, random seeds, and class definitions in any derivative study. Odor labels and model outputs are research annotations and should not be treated as a substitute for laboratory sensory or chemical validation.

Please cite the Zenodo DOI for the exact release used, link this repository, and acknowledge any upstream dataset or descriptor software required by your analysis. The repository currently has no blanket license; verify the rights of the data and descriptor definitions before redistribution or commercial use.
