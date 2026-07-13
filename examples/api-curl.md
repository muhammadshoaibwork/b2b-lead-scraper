# REST API (curl) — B2B Lead Scraper

Call the hosted [B2B Lead Scraper](https://apify.com/logiover/b2b-lead-scraper) over the Apify REST API. These are **client-side** examples — replace `<YOUR_APIFY_TOKEN>` with your token from [Apify → Integrations](https://console.apify.com/account/integrations).

The Actor ID in the API is `logiover~b2b-lead-scraper` (the `/` becomes `~`).

## 1. Run and get dataset items in one call (recommended)

`run-sync-get-dataset-items` starts a run and returns the dataset items in the same HTTP response.

```bash
curl -X POST "https://api.apify.com/v2/acts/logiover~b2b-lead-scraper/run-sync-get-dataset-items?token=<YOUR_APIFY_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "sector": "dentists",
    "country": "Germany",
    "maxLeads": 1000,
    "verifyEmails": true
  }'
```

Get CSV instead of JSON by adding `&format=csv` to the URL.

## 2. Start a run asynchronously

```bash
curl -X POST "https://api.apify.com/v2/acts/logiover~b2b-lead-scraper/runs?token=<YOUR_APIFY_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "sector": "marketing agencies",
    "country": "United States",
    "maxLeads": 5000
  }'
```

The response includes the run `id` and `defaultDatasetId`.

## 3. Poll run status

```bash
curl "https://api.apify.com/v2/actor-runs/<RUN_ID>?token=<YOUR_APIFY_TOKEN>"
```

Wait until `"status": "SUCCEEDED"`.

## 4. Fetch dataset items

```bash
# JSON
curl "https://api.apify.com/v2/datasets/<DATASET_ID>/items?token=<YOUR_APIFY_TOKEN>"

# CSV export
curl "https://api.apify.com/v2/datasets/<DATASET_ID>/items?format=csv&token=<YOUR_APIFY_TOKEN>" -o leads.csv

# Excel export
curl "https://api.apify.com/v2/datasets/<DATASET_ID>/items?format=xlsx&token=<YOUR_APIFY_TOKEN>" -o leads.xlsx
```

Pagination with `&limit=` and `&offset=`, and field filtering with `&fields=companyName,domain,phones,companyEmails` are supported.

## 5. Enrichment mode — crawl your own domains

```bash
curl -X POST "https://api.apify.com/v2/acts/logiover~b2b-lead-scraper/run-sync-get-dataset-items?token=<YOUR_APIFY_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "extraStartUrls": ["https://acme.com", "example-agency.co.uk"],
    "verifyEmails": true
  }'
```

API reference: https://docs.apify.com/api/v2
