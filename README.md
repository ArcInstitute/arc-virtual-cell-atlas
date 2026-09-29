Arc Virtual Cell Atlas
======================

> [!IMPORTANT]
> **Data Migration Notice**: Arc's Virtual Cell Atlas data has migrated to the [Google Cloud Marketplace](https://console.cloud.google.com/marketplace/product/bigquery-public-data/arc-institute?project=gcp-public-data-arc-institute). 
> 
> **Note**: The bucket does **not** require a billing project. Do **not** pass `-u` / `--billing-project` — doing so bills the download to your own project. See [Accessing the data](#accessing-the-data).
> 
> Access to the current GCS buckets (`gs://arc-ctc-tahoe100/` and `gs://arc-scbasecount/`) has been removed as of **March 31, 2026**. Please update your workflows to use the Google Marketplace bucket `gs://arc-institute-virtual-cell-atlas`.

The Arc Virtual Cell Atlas is a collection of high quality, curated, open datasets assembled for the purpose of accelerating the creation of virtual cell models.
The atlas includes both observational and perturbational data from over 602 million cells (and growing).

The atlas is bootstrapped with [Tahoe’s](https://www.tahoebio.ai/) Tahoe-100M and [Arc’s](https://arcinstitute.org/) AI agent-curated scBaseCount dataset.

To explore the atlas via a UI, you can take a look at its LaminDB mirror: https://lamin.ai/laminlabs/arc-virtual-cell-atlas

# Accessing the data

The bucket `gs://arc-institute-virtual-cell-atlas` is publicly readable and does **not** have [Requester Pays](https://docs.cloud.google.com/storage/docs/requester-pays) enabled. There is no Marketplace subscription step — the listing only offers **View dataset**.

> [!WARNING]
> **Do not pass a billing project.** Cloud Storage bills network and operation charges to any project you supply with a request, *even on buckets without Requester Pays* ([Google docs](https://docs.cloud.google.com/storage/docs/requester-pays)). `gsutil -u <PROJECT>`, `gcloud storage --billing-project=<PROJECT>`, or `gcsfs` with `requester_pays=True` all charge your project at standard egress rates. There is no free allowance or credit for this bucket.

| Tool | Command |
|------|---------|
| `gsutil` | `gsutil -m rsync -r gs://arc-institute-virtual-cell-atlas/<PREFIX> <DEST>` |
| `gcloud storage` | `gcloud storage rsync -r gs://arc-institute-virtual-cell-atlas/<PREFIX> <DEST>` |
| Python (`gcsfs`) | `gcsfs.GCSFileSystem()` — leave `requester_pays` at its default (`False`) |

If a command ever fails with *"Bucket is a requester pays bucket but no user project provided"*, Requester Pays has been enabled and downloads will be billed to your project — please [open an issue](https://github.com/ArcInstitute/arc-virtual-cell-atlas/issues) before proceeding.

Scope every transfer to the dataset prefix you actually need — syncing the bucket root pulls the entire atlas:

| Prefix | Contents |
|--------|----------|
| `gs://arc-institute-virtual-cell-atlas/scbasecount/` | scBaseCount, partitioned by release, quantification, and species (e.g. `scbasecount/2026-01-12/h5ad/GeneFull_Ex50pAS/Homo_sapiens/` is several TB) |
| `gs://arc-institute-virtual-cell-atlas/tahoe100M/` | Tahoe-100M |
| `gs://arc-institute-virtual-cell-atlas/virtual-cell-challenge/` | Virtual Cell Challenge data |

Check a prefix's size before transferring it: `gcloud storage du -s --readable-sizes gs://arc-institute-virtual-cell-atlas/<PREFIX>`

If you were charged after downloading with a billing project set, the charge was applied to your project by design; contact [Google Cloud Billing Support](https://cloud.google.com/support/billing) to request an adjustment.

# Tahoe-100M

[Documentation](./tahoe-100M/README.md)

# scBaseCount

[Documentation](./scBaseCount/README.md)

# Virtual Cell Challenge

[Documentation](./virtual-cell-challenge/README.md)
