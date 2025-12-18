Tahoe-100M
==========

> [!IMPORTANT]
> **Data Migration Notice**: This dataset is now available on the [Google Cloud Marketplace](https://console.cloud.google.com/marketplace/product/bigquery-public-data/arc-institute?project=gcp-public-data-arc-institute). 
> 
> **Note**: The new bucket is subject to [Requester Pays](https://docs.cloud.google.com/storage/docs/requester-pays). Users can access up to 2TB of data per month for free before fees apply.
> 
> Access to the current GCS bucket (`gs://arc-ctc-tahoe100/`) will be deprecated on **March 31, 2026**. Please update your workflows to use the Google Marketplace bucket `gs://arc-institute-virtual-cell-atlas`.

# Overview

* **Format:** 
  * Count matrices: h5ad (AnnData)
  * Metadata: Parquet
* **Data host:** 
  * [Google Cloud Marketplace](https://console.cloud.google.com/marketplace/product/bigquery-public-data/arc-institute?project=gcp-public-data-arc-institute) (Recommended)
  * Google Cloud Storage (Google Marketplace bucket: `gs://arc-institute-virtual-cell-atlas`)
  * Google Cloud Storage (Deprecated: access ends March 31, 2026)
    * Path: `gs://arc-ctc-tahoe100/`
* **Statistics**
  * Sample count: 1344
  * Cell count: 100648790

## Manuscript

[Tahoe-100M: A Giga-Scale Single-Cell Perturbation Atlas for Context-Dependent Gene Function and Cellular Modeling](https://doi.org/10.1101/2025.02.20.639398)

## obs (cell) metadata

| Column Name            | Description                                                            |
|------------------------|------------------------------------------------------------------------|
| **plate**              | Plate identifier                                                       |
| **BARCODE_SUB_LIB_ID** | Cell identifier                                                        |
| **sample**             | Unique treatment identifier, distinguishes replicated treatments       |
| **gene_count**         | Number of genes with at least one count                                |
| **tscp_count**         | Number of transcripts, aka UMI count                                   |
| **mread_count**        | Number of reads per cell                                               |
| **drugname_drugconc**  | Drug name, concentration, and concentration unit                       |
| **drug**               | Drug name, parsed out from the `drugname_drugconc` field               |
| **cell_line**          | Cell line Cellosaurus identifier                                       |
| **sublibrary**         | Sublibrary ID (related to library prep and sequencing)                 |
| **BARCODE**            | Barcode ID                                                             |
| **pcnt_mito**          | Percentage of mitochondrial reads                                      |
| **S_score**            | Inferred S phase score                                                 |
| **G2M_score**          | Inferred G2M score                                                     |
| **phase**              | Inferred cell cycle phase                                              |
| **pass_filter**        | "Full" filters are more stringent on `gene_count` and `tscp_count`     |
| **cell_name**          | Commonly-used cell name (related to the `cell_line` field)             |


# Tutorials

* [Python](./tutorial-py.ipynb)

# Notes

* `.h5ad.gz` file extensions denote internal gzip compression. 
  * See the [Python tutorial](./tutorial-py.ipynb) on reading in the anndata objects.


# Resources

* [Tahoe](https://www.tahoebio.ai/)


# Contact

* [GitHub Issues](https://github.com/ArcInstitute/arc-virtual-cell-atlas/issues)
