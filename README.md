Arc Virtual Cell Atlas
======================

> [!IMPORTANT]
> **Data Migration Notice**: Arc's Virtual Cell Atlas data has migrated to the [Google Cloud Marketplace](https://console.cloud.google.com/marketplace/product/bigquery-public-data/arc-institute?project=gcp-public-data-arc-institute). 
> 
> **Note**: The new bucket is subject to [Requester Pays](https://docs.cloud.google.com/storage/docs/requester-pays). Users can access up to 2TB of data per month for free before fees apply.
> 
> Access to the current GCS buckets (`gs://arc-ctc-tahoe100/` and `gs://arc-scbasecount/`) will be deprecated on **March 31, 2026**. Please update your workflows to use the Google Marketplace bucket `gs://arc-institute-virtual-cell-atlas`.

The Arc Virtual Cell Atlas is a collection of high quality, curated, open datasets assembled for the purpose of accelerating the creation of virtual cell models.
The atlas includes both observational and perturbational data from over 330 million cells (and growing).

The atlas is bootstrapped with [Tahoe’s](https://www.tahoebio.ai/) Tahoe-100M and [Arc’s](https://arcinstitute.org/) AI agent-curated scBaseCount dataset.


# Tahoe-100M

[Documentation](./tahoe-100M/README.md)

# scBaseCount

[Documentation](./scBaseCount/README.md)

# Virtual Cell Challenge

[Documentation](./virtual-cell-challenge/README.md)

> Note `scBaseCount` is the new name for `scBaseCamp`