# Apify CLI — B2B Lead Scraper

Run the [B2B Lead Scraper](https://apify.com/logiover/b2b-lead-scraper) from your terminal with the Apify CLI. These are **client-side** usage examples for the hosted Actor.

## Install & authenticate

```bash
npm install -g apify-cli
apify login   # paste your token from https://console.apify.com/account/integrations
```

## Sector + country (local lead gen)

```bash
apify call logiover/b2b-lead-scraper --input '{
  "sector": "dentists",
  "country": "Germany",
  "maxLeads": 2000,
  "verifyEmails": true
}'
```

## Named-contact focus (people-profile crawl)

```bash
apify call logiover/b2b-lead-scraper --input '{
  "sector": "law firms",
  "country": "United Kingdom",
  "maxProfilesPerSite": 15,
  "requireEmail": true,
  "guessEmails": true,
  "maxLeads": 1000
}'
```

## Enrichment mode — crawl your own domains

Create `input.json`:

```json
{
  "extraStartUrls": ["https://acme.com", "example-agency.co.uk", "https://widgetco.de"],
  "verifyEmails": true
}
```

Run it:

```bash
apify call logiover/b2b-lead-scraper --input-file input.json
```

## Export the dataset of a finished run

`apify call` prints the run and dataset IDs. Fetch the results:

```bash
# CSV
curl "https://api.apify.com/v2/datasets/<DATASET_ID>/items?format=csv&token=<YOUR_APIFY_TOKEN>" -o leads.csv

# Excel
curl "https://api.apify.com/v2/datasets/<DATASET_ID>/items?format=xlsx&token=<YOUR_APIFY_TOKEN>" -o leads.xlsx
```

Common flags:

- `--input '<json>'` — inline input JSON.
- `--input-file <path>` — read input from a file.
- `--silent` — suppress progress logs.

Docs: https://docs.apify.com/cli
