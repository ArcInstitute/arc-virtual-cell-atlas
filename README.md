Arc Virtual Cell Atlas
======================

**Data access**: The atlas is hosted on the [Google Cloud Marketplace](https://console.cloud.google.com/marketplace/product/bigquery-public-data/arc-institute?project=gcp-public-data-arc-institute) in the public bucket `gs://arc-institute-virtual-cell-atlas`. See [Accessing the data](#accessing-the-data).

The Arc Virtual Cell Atlas is a collection of high quality, curated, open datasets assembled for the purpose of accelerating the creation of virtual cell models.
The atlas includes both observational and perturbational data from over 602 million cells (and growing).

The atlas is bootstrapped with [Tahoe’s](https://www.tahoebio.ai/) Tahoe-100M and [Arc’s](https://arcinstitute.org/) AI agent-curated scBaseCount dataset.

To explore the atlas via a UI, you can take a look at its LaminDB mirror: https://lamin.ai/laminlabs/arc-virtual-cell-atlas

# Accessing the data

`gs://arc-institute-virtual-cell-atlas` is publicly readable and doesn't require a Google Cloud project or billing account. Download with `gcloud storage` from the [Google Cloud CLI](https://docs.cloud.google.com/sdk/docs/install-sdk), or read files directly from Python with `gcsfs`:

| Tool | Example |
|------|---------|
| [`gcloud storage`](https://docs.cloud.google.com/sdk/gcloud/reference/storage) | `gcloud storage rsync -r gs://arc-institute-virtual-cell-atlas/virtual-cell-challenge/2025/validation/ <DEST>` |
| [`gsutil`](https://docs.cloud.google.com/storage/docs/gsutil) (legacy) | `gsutil -m rsync -r gs://arc-institute-virtual-cell-atlas/scbasecount/2026-01-12/h5ad/GeneFull_Ex50pAS/Callithrix_jacchus/ <DEST>` |
| Python ([`gcsfs`](https://gcsfs.readthedocs.io/en/latest/)) | `pd.read_parquet("gs://arc-institute-virtual-cell-atlas/scbasecount/2026-01-12/metadata/GeneFull_Ex50pAS/Homo_sapiens/sample_metadata.parquet", storage_options={"token": "anon"})` |

Full Python and R walkthroughs are in the tutorial notebooks: scBaseCount ([Python](./scBaseCount/tutorial-py.ipynb), [R](./scBaseCount/tutorial-R.ipynb)), Tahoe-100M ([Python](./tahoe-100M/tutorial-py.ipynb)), Virtual Cell Challenge ([Python](./virtual-cell-challenge/tutorial-py.ipynb), [R](./virtual-cell-challenge/tutorial-R.ipynb)).

Scope every transfer to the dataset prefix you actually need — syncing the bucket root pulls the entire atlas:

| Prefix | Contents | Size |
|--------|----------|------|
| `gs://arc-institute-virtual-cell-atlas/scbasecount/` | scBaseCount, partitioned by release, quantification, and species | 39.5 TiB (e.g. `scbasecount/2026-01-12/h5ad/GeneFull_Ex50pAS/Homo_sapiens/` is 3.1 TiB) |
| `gs://arc-institute-virtual-cell-atlas/tahoe100M/` | Tahoe-100M | 316 GiB |
| `gs://arc-institute-virtual-cell-atlas/virtual-cell-challenge/` | Virtual Cell Challenge data | 5.3 TiB (5.28 TiB is raw FASTQ under `2025/FASTQ/`) |

Check a prefix's size before transferring it, e.g. `gcloud storage du -s --readable-sizes gs://arc-institute-virtual-cell-atlas/tahoe100M/`

If you see download charges on your own Google Cloud project, please [open an issue](https://github.com/ArcInstitute/arc-virtual-cell-atlas/issues) with the billing line items (SKU, project, dates) so we can investigate.

# Tahoe-100M

[Documentation](./tahoe-100M/README.md)

# scBaseCount

[Documentation](./scBaseCount/README.md)

# Virtual Cell Challenge

[Documentation](./virtual-cell-challenge/README.md)
