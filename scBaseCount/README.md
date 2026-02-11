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

scBaseCount is a continuously updated single-cell RNA-seq database that employs an AI-driven, 
hierarchical agent workflow to automate discovery, metadata extraction, and standardized preprocessing of Sequence Read Archive (SRA) data.

Currently the largest public repository of single-cell data comprising over 502 million cells (and expanding), 
spanning 27 organisms and 75 tissues.

By continually discovering, annotating, and reprocessing raw single-cell RNA-seq data, 
scBaseCount offers an expansive and harmonized repository that can serve as a foundation for AI-driven modeling and integrative meta-analyses.


## Manuscript

**scBaseCount: An AI agent-curated, uniformly processed, and continually expanding single cell data repository**
Nicholas D. Youngblut, Christopher Carpenter, Arshia Nayebnazar, Abhinav Adduri, Rohan Shah, Chiara Ricci-Tam, Jaanak Prashar, Rajesh Ilango,
Noam Teyssier, Silvana Konermann, Patrick D. Hsu, Alexander Dobin, Dave P. Burke, Hani Goodarzi, Yusuf H Roohani.
bioRxiv 2025.02.27.640494; doi: [https://doi.org/10.1101/2025.02.27.640494](https://doi.org/10.1101/2025.02.27.640494)


# Dataset location

* [Google Cloud Marketplace](https://console.cloud.google.com/marketplace/product/bigquery-public-data/arc-institute?project=gcp-public-data-arc-institute) (Recommended)
* Google Cloud Storage (Google Marketplace bucket: `gs://arc-institute-virtual-cell-atlas`)
* Google Cloud Storage (Deprecated: access ends March 31, 2026)
  * Path: `gs://arc-scbasecount`

See the tutorials below on programmatically accessing the data.

You can also directly download the data with the [gsutil tool](https://cloud.google.com/storage/docs/gsutil).
Please be mindful of the egress costs associated with downloading data from Google Cloud Storage (costs to the Arc Institute).


# Releases

## 2026-01-12: Publication release

### Statistics

* \>502 million cells
  * Note: the number of cells differs by STARsolo feature type
* 27 organisms
* 75 tissues
* h5ad files include all STARsolo mapping approaches:
  * `Gene*` feature types:
    * `Unique`: Unique molecular identifiers (UMIs) are assigned to each read based on the read sequence and the barcode sequence.
      * `adata.X` matrix
    * `EM`: Equivalent molecular identifiers (EMs) are assigned to each read based on the read sequence and the barcode sequence.
      * `UniqueAndMult-EM` layer
    * `Uniform`: Uniform molecular identifiers (UMIs) are assigned to each read based on the read sequence and the barcode sequence.
      * `UniqueAndMult-Uniform` layer
  * `Velocyto` feature type:
    * `spliced`: Spliced reads are assigned to each read based on the read sequence and the barcode sequence.
      * `adata.X` matrix and `spliced` layer
    * `unspliced`: Unspliced reads are assigned to each read based on the read sequence and the barcode sequence.
      * `unspliced` layer
    * `ambiguous`: Ambiguous reads are assigned to each read based on the read sequence and the barcode sequence.
      * `ambiguous` layer

### Added metadata (see initial release for other metadata fields)

#### Sample Metadata (SRX accession level)

* `tissue_ontology_term_id`: Tissue ontology term ID (if applicable)
* `disease_ontology_term_id`: Disease ontology term ID (if applicable)
* `antibody_derived_tag` : SRAgent was used to determine whether the SRX accession metadata in the SRA/ENA included any mention of "antibody derived tag". Such SRX accessions were labeled as "maybe" for ADT status.

#### Cell-level Metadata (per observation)

* UMI count per feature type
* Gene count per feature type
* `cell_type`: Cell type annotation (if available)
* `cell_type_ontology_term_id`: Cell type ontology term ID (if available)

### Important notes

#### `NRX` accessions

We created the `NRX` accession prefix and use it to identify datasets not in the SRA/ENA.

`NRX` accessions are datasets that do not have any SRX/ERX accession ID. 
These datasets are associated with CZI CELLxGENE collections and were made public but never uploaded to the SRA/ENA.
We manually processed these datasets outside of the SRAgent/scRecounter workflow.
In order to associate an NRX accession with a CZI collection, you can use the sample metadata tables (parquet files).  

#### Disease annotations

Disease annotations are extracted at the **study level**, not the sample or cell level. SRAgent derives disease labels from author-supplied abstracts in the SRA, which describe the overall study design (e.g., "A study of COVID-19 infection") rather than the status of individual donors or samples. As a result, a single disease label (e.g., "COVID-19") may be propagated to all samples within a study, including healthy controls.

## 2025-02-01: Initial release

### Statistics

* \>230 million cells
* 21 organisms
* Multiple STARsolo count features (e.g., `Gene`, `GeneFull_Ex50pAS`, and `Velocyto`)
  * Note: only `filtered` count tables are provided (no `raw`)

### Metadata

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

## STARsolo Count Features

* **Gene:** Counts reads mapping entirely to exonic regions of annotated transcripts, capturing fully spliced
transcripts. This procedure represents traditional gene expression analyses and was the default in earlier
CellRanger versions.
* **GeneFull:** Counts reads overlapping entire gene locus, i.e. including exonic and intronic regions. This
captures both unspliced (primary) and spliced transcripts and provides a more comprehensive view of
gene activity.
* **GeneFull_ExonOverIntron:** Counts reads overlapping exonic and intronic regions, but assigns higher
priority to exonic overlaps. This option helps to resolve reads that map to overlapping genes.
* **GeneFull_Ex50pAS:** Similar to the above option, but with more sophisticated priority scheme, which
prioritizes partial and antisense exonic overlap over intronic reads.
* **Velocyto:** Generates separate count matrices for spliced, unspliced, and ambiguous reads, following
the rules from [La Manno et al., 2018](https://doi.org/10.1038/s41586-018-0414-6). 
This enables RNA velocity analyses to infer dynamic cellular processes.

## Notes

* STARsolo count features (e.g., `Gene`, `GeneFull_Ex50pAS`, and `Velocyto`)
  * `GeneFull*` will have more counts than `Gene` due to the mapping algorithm.
  * `Velocyto` h5ad files include 3 layers: `spliced`, `unspliced`, and `ambiguous`.
     * The `X` matrix is `spliced` matrix, in addition to the `spliced` layer.
* We are currently not using [TileDB-SOMA](https://tiledbsoma.readthedocs.io/en/latest/index.html)
  due to some challenges with scaling.


# Tutorials

* [Python tutorial](./tutorial-py.ipynb)
* [R tutorial](./tutorial-R.ipynb)


# Resources

* [Arc Institute](https://arcinstitute.org)


# Contact

* [GitHub Issues](https://github.com/ArcInstitute/arc-virtual-cell-atlas/issues)


