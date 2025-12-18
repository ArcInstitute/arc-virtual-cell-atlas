Virtual Cell Challenge
======================

# Overview

For Arc’s Virtual Cell Challenge, we generated a dedicated dataset measuring single-cell responses to perturbations in a human embryonic stem cell line (H1 hESC). This set of perturbations was carefully curated to span a broad range of phenotypic responses, and experimental parameters were optimized to maximize the reproducibility of observed effects. The Atlas includes the training, validation, and held-out test perturbation datasets leveraged throughout the Challenge.

* **Format:**
  * Count matrices: h5ad (AnnData)
  * Metadata: Parquet & CSV
* **Data host:**
  * Google Marketplace bucket: `gs://arc-institute-virtual-cell-atlas/virtual-cell-challenge/`
* **Statistics**
  * Cell count: ~300,000 cells
  * Target genes: 300

## External Links

* [Virtual Cell Challenge Datasets](https://virtualcellchallenge.org/datasets)
* [Behind the Data of the Virtual Cell Challenge (Arc Blog)](https://arcinstitute.org/news/behind-the-data-virtual-cell-challenge)

## Data Structure

The data is organized by release year and split into training, validation, and test sets.

```
virtual-cell-challenge/
└── 2025/
    ├── test/
    ├── train/
    │   ├── adata_Training.h5ad
    │   └── pert_counts_Training.csv
    └── validation/
```

## Metrics

The challenge evaluation framework centers around three metrics:

* **Differential Expression Score (DES)**: Captures whether the model recovers the correct set of differentially expressed genes.
* **Perturbation Discrimination Score (PDS)**: Measures whether the model assigns the correct effect to the correct perturbation.
* **Mean Absolute Error (MAE)**: Assesses global expression accuracy across all genes.

# Tutorials

* [Python](./tutorial-py.ipynb)
* [R](./tutorial-R.ipynb)

# Contact

* [GitHub Issues](https://github.com/ArcInstitute/arc-virtual-cell-atlas/issues)

