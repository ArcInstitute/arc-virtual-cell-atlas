scBaseCount
===========

> [!IMPORTANT]
> **Data Migration Notice**: This dataset is now available on the [Google Cloud Marketplace](https://console.cloud.google.com/marketplace/product/bigquery-public-data/arc-institute?project=gcp-public-data-arc-institute). 
> 
> **Note**: The new bucket is subject to [Requester Pays](https://docs.cloud.google.com/storage/docs/requester-pays). Users can access up to 2TB of data per month for free before fees apply.
> 
> Access to the current GCS bucket (`gs://arc-scbasecount`) will be deprecated on **March 31, 2026**. Please update your workflows to use the Google Marketplace bucket `gs://arc-institute-virtual-cell-atlas`.

### An AI agent-curated, uniformly processed, and continually expanding single cell data repository

<a href="https://arcinstitute.org/news/news/arc-virtual-cell-atlas-launch">
  <img src="./img/scBaseCount_banner.png" alt="scBaseCount" width="780" height="415">
</a>


# Overview

scBaseCount (formerly "scBaseCamp") is a continuously updated single-cell RNA-seq database that employs an AI-driven, 
hierarchical agent workflow to automate discovery, metadata extraction, and standardized preprocessing of Sequence Read Archive (SRA) data.

Currently the largest public repository of single-cell data comprising over 230 million cells (and expanding), 
spanning 21 organisms and 72 tissues.

By continually discovering, annotating, and reprocessing raw single-cell RNA-seq data, 
scBaseCount offers an expansive and harmonized repository that can serve as a foundation for AI-driven modeling and integrative meta-analyses.


## Manuscript

**scBaseCount: An AI agent-curated, uniformly processed, and continually expanding single cell data repository**
Nicholas D Youngblut, Christopher Carpenter, Jaanak Prashar, Chiara Ricci-Tam, Rajesh Ilango, Noam Teyssier,
Silvana Konermann, Patrick Hsu, Alexander Dobin, David P Burke, Hani Goodarzi, Yusuf H Roohani.
bioRxiv 2025.02.27.640494; doi: [https://doi.org/10.1101/2025.02.27.640494](https://doi.org/10.1101/2025.02.27.640494)


# Dataset location

* [Google Cloud Marketplace](https://console.cloud.google.com/marketplace/product/bigquery-public-data/arc-institute?project=gcp-public-data-arc-institute) (Recommended)
* Google Cloud Storage (Google Marketplace bucket: `gs://arc-institute-virtual-cell-atlas`)
* Google Cloud Storage (Deprecated: access ends March 31, 2026)
  * Path: `gs://arc-scbasecount`

> WARNING: the previous location was `gs://arc-ctc-scbasecamp`, which is now deprecated and will be removed on Sunday, May 11, 2025.

See the tutorials below on progamatically accessing the data.

You can also directly download the data with the [gsutil tool](https://cloud.google.com/storage/docs/gsutil).
Please be mindful of the egress costs associated with downloading data from Google Cloud Storage (costs to the Arc Institute).


# Releases

## 2025-02-01: Initial release

#### Statistics

* \>230 million cells
* 21 organisms
* Multiple STARsolo count features (e.g., `Gene`, `GeneFull_Ex50pAS`, and `Velocyto`)
  * Note: only `filtered` count tables are provided (no `raw`)

#### Metadata

The observation metadata was obtained via [SRAgent](https://github.com/ArcInstitute/SRAgent).

#### Sample Metadata (SRX accession level)

Each sample includes the following metadata fields:

**Core Identifiers**
* `entrez_id`: Entrez database identifier
* `srx_accession`: SRA experiment accession
* `file_path`: Path to h5ad file in Google Cloud Storage
* `obs_count`: Total number of cells/observations

**Technical Information**
* `lib_prep`: Library preparation method (10X Genomics or other)
* `tech_10x`: Specific 10X Genomics technology (e.g. 3' or 5')
* `cell_prep`: Sample preparation type (single nucleus or single cell)

**Biological Information**
* `organism`: Species of origin
* `tissue`: Source tissue
* `disease`: Disease status (if applicable)
* `perturbation`: Experimental perturbations (if applicable)
* `cell_line`: Cell line information (if applicable)

**Collection Information**
* `czi_collection_id`: CZI collection identifier (if applicable)
* `czi_collection_name`: CZI collection name (if applicable)

#### Cell-level Metadata (per observation)

Each individual cell (obs) contains:
* `gene_count`: Number of unique genes detected
* `umi_count`: Total number of Unique Molecular Identifiers (UMIs)

#### STARsolo Count Features

* **Gene:** Counts reads mapping entirely to exonic regions of annotated transcripts, capturing fully spliced
transcripts. This procedure represents traditional gene expression analyses and was the default in earlier
CellRanger versions.
* **GeneFull:** Counts reads overlapping entire gene locus, i.e. including exonic and intronic regions. This
captures both unspliced (primary) and spliced transcripts and providing a more comprehensive view of
gene activity.
* **GeneFull_ExonOverIntron:** Counts reads overlapping exonic and intronic regions, but assigns higher
priority to exonic overlaps. This option helps to resolve reads that map to overlapping genes.
* **GeneFull_Ex50pAS:** Similar to the above option, but with more sophisticated priorities scheme, which
prioritizes partial and antisense exonic overlap over intronice reads.
* **Velocyto:** Generates separate count matrices for spliced, unspliced, and ambiguous reads, following
the rules from ([La Manno et al., 2018](https://doi.org/10.1038/s41586-018-0414-6)). 
This enables RNA velocity analyses to infer dynamic cellular processes.

#### Notes

* Currently, the dataset does NOT include cell type annotations.
  * We are looking into scalable approaches to annotate cell types across the entire dataset.
  * This task is not trivial, given the diversity of the dataset (e.g., organisms and tissues).
* STARsolo count features (e.g., `Gene`, `GeneFull_Ex50pAS`, and `Velocyto`)
  * `GeneFull*` will have more counts than `Gene` due to the mapping algorithm.
  * `Velocyto` h5ad files include 3 layers: `spliced`, `unspliced`, and `ambiguous`.
     * The `X` matrix is `spliced` matrix, in addition to the `spliced` layer.
  * `unique` mapping is used for all count features.
    * `EM` and `uniform` mapping will be added soon.
    * However, we have found that `unique` mapping is the best choice for at least most datasets.
* We are currently not using [TileDB-SOMA](https://tiledbsoma.readthedocs.io/en/latest/index.html)
  due to some challenges with scaling, but we are actively working on this.
* `.h5ad.gz` file extensions denote internal gzip compression. 


# Tutorials

* [Python tutorial](./tutorial-py.ipynb)
* [R tutorial](./tutorial-R.ipynb)


# Resources

* [Arc Institute](https://arcinstitute.org)


# Contact

* [GitHub Issues](https://github.com/ArcInstitute/arc-virtual-cell-atlas/issues)


# Citation
```
@article {Youngblut2025.02.27.640494,
	author = {Youngblut, Nicholas D and Carpenter, Christopher and Prashar, Jaanak and Ricci-Tam, Chiara and Ilango, Rajesh and Teyssier, Noam and Konermann, Silvana and Hsu, Patrick and Dobin, Alexander and Burke, David P and Goodarzi, Hani and Roohani, Yusuf H},
	title = {scBaseCount: An AI agent-curated, uniformly processed, and continually expanding single cell data repository},
	year = {2025},
	doi = {10.1101/2025.02.27.640494},
	publisher = {Cold Spring Harbor Laboratory},
	journal = {bioRxiv}
}
```
- [Paper](https://www.biorxiv.org/content/early/2025/03/04/2025.02.27.640494)
