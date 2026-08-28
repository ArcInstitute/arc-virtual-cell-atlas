Arc Virtual Cell Atlas
======================

> [!IMPORTANT]
> **Data Migration Notice**: Arc's Virtual Cell Atlas data has migrated to the [Google Cloud Marketplace](https://console.cloud.google.com/marketplace/product/bigquery-public-data/arc-institute?project=gcp-public-data-arc-institute). 
> 
> **Note**: The new bucket is subject to [Requester Pays](https://docs.cloud.google.com/storage/docs/requester-pays), with up to 2TB of data per month at no cost — but **only from a project subscribed to the dataset on the Marketplace**. See [Accessing the data](#accessing-the-data) before you download.
> 
> Access to the current GCS buckets (`gs://arc-ctc-tahoe100/` and `gs://arc-scbasecount/`) has been removed as of **March 31, 2026**. Please update your workflows to use the Google Marketplace bucket `gs://arc-institute-virtual-cell-atlas`.

The Arc Virtual Cell Atlas is a collection of high quality, curated, open datasets assembled for the purpose of accelerating the creation of virtual cell models.
The atlas includes both observational and perturbational data from over 602 million cells (and growing).

The atlas is bootstrapped with [Tahoe’s](https://www.tahoebio.ai/) Tahoe-100M and [Arc’s](https://arcinstitute.org/) AI agent-curated scBaseCount dataset.

To explore the atlas via a UI, you can take a look at its LaminDB mirror: https://lamin.ai/laminlabs/arc-virtual-cell-atlas

# Accessing the data

The Marketplace bucket `gs://arc-institute-virtual-cell-atlas` uses [Requester Pays](https://docs.cloud.google.com/storage/docs/requester-pays) and includes up to **2TB of egress per month at no cost**.

> [!WARNING]
> The free tier is **not** automatic. It applies only to a Google Cloud project that is subscribed to the dataset on the Marketplace. Reading from an unsubscribed project — even with the `-u` / `--billing-project` flag set — is billed at standard GCS egress and operation rates, because the billing system has no way to know you are eligible.

| Step | What to do |
|------|------------|
| 1. Subscribe | Open the [Virtual Cell Atlas listing](https://console.cloud.google.com/marketplace/product/bigquery-public-data/arc-institute?project=gcp-public-data-arc-institute) on the GCP Marketplace and click **Subscribe** (may appear as **Enroll** or **Get Started**) |
| 2. Choose the billing project | During subscription, select the exact Google Cloud project you will use to read the data |
| 3. Use that same project | Pass that project ID on **every** command — `-u` for `gsutil`, `--billing-project` for `gcloud storage`. A different project ID forfeits the free tier |

Scope every transfer to the dataset prefix you actually need — syncing the bucket root pulls the entire atlas:

| Prefix | Contents |
|--------|----------|
| `gs://arc-institute-virtual-cell-atlas/scbasecount/` | scBaseCount, partitioned by release, quantification, and species |
| `gs://arc-institute-virtual-cell-atlas/tahoe100M/` | Tahoe-100M |
| `gs://arc-institute-virtual-cell-atlas/virtual-cell-challenge/` | Virtual Cell Challenge data |

```bash
# gsutil — one species of one scBaseCount release, not the whole atlas
gsutil -u <SUBSCRIBED_PROJECT_ID> rsync -r \
  gs://arc-institute-virtual-cell-atlas/scbasecount/2026-01-12/h5ad/GeneFull_Ex50pAS/Homo_sapiens \
  /path/to/local/destination

# gcloud storage
gcloud storage rsync -r --billing-project=<SUBSCRIBED_PROJECT_ID> \
  gs://arc-institute-virtual-cell-atlas/scbasecount/2026-01-12/h5ad/GeneFull_Ex50pAS/Homo_sapiens \
  /path/to/local/destination
```

Note that 2TB/month is easily exceeded even without syncing the whole atlas — a single species within a single scBaseCount release and quantification can run to several TB. Check the size of a prefix before transferring it:

```bash
gcloud storage du -s --readable-sizes --billing-project=<SUBSCRIBED_PROJECT_ID> \
  gs://arc-institute-virtual-cell-atlas/<PREFIX>
```

**Credits are applied asynchronously**, so a charge appearing on your dashboard does not by itself mean the free tier failed. Egress and operation charges are written to the billing log first; the offsetting Marketplace credits can take **24–48 hours** to show up, and under some billing configurations are applied as an end-of-cycle promotional discount rather than in real time. Check your Cloud Billing reports grouped by **Credits** to see whether an offsetting credit has posted or is pending.

If you were charged for a download made from a project that was never subscribed, subscribe now to prevent further charges, then file a ticket with [Google Cloud Support](https://cloud.google.com/support) to request a one-time courtesy billing adjustment.

# Tahoe-100M

[Documentation](./tahoe-100M/README.md)

# scBaseCount

[Documentation](./scBaseCount/README.md)

# Virtual Cell Challenge

[Documentation](./virtual-cell-challenge/README.md)
