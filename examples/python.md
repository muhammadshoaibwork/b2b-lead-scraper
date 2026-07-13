# Python — B2B Lead Scraper

Call the hosted [B2B Lead Scraper](https://apify.com/logiover/b2b-lead-scraper) with the official [`apify-client`](https://docs.apify.com/api/client/python/) for Python. These are **client-side** usage examples for the Actor on Apify.

## Install

```bash
pip install apify-client
```

## Run the Actor and read results

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_TOKEN")

run_input = {
    "sector": "dentists",
    "country": "Germany",
    "maxLeads": 2000,
    "verifyEmails": True,
}

run = client.actor("logiover/b2b-lead-scraper").call(run_input=run_input)

items = list(client.dataset(run["defaultDatasetId"]).iterate_items())
print(f"Fetched {len(items)} company leads")

# Companies with at least one deliverable (MX-valid) email
def has_mx(c):
    emails = (c.get("companyEmails") or []) + (c.get("persons") or [])
    return any(str(e.get("status", "")).endswith("mx_valid") for e in emails)

print(f"{sum(1 for c in items if has_mx(c))} companies with a deliverable email")
```

## Flatten decision-makers into an outreach list

```python
items = list(client.dataset(run["defaultDatasetId"]).iterate_items())

contacts = []
for c in items:
    for p in c.get("persons") or []:
        if p.get("email"):
            contacts.append({
                "company": c.get("companyName"),
                "name": p.get("name"),
                "title": p.get("title"),
                "email": p.get("email"),
                "status": p.get("status"),
            })
print(f"{len(contacts)} named contacts")
print(contacts[:5])
```

## Export to a pandas DataFrame / CSV

```python
import pandas as pd

items = list(client.dataset(run["defaultDatasetId"]).iterate_items())
df = pd.DataFrame(items)
df.to_csv("leads.csv", index=False)
print(df[["companyName", "domain", "phones", "address"]].head())
```

Client docs: https://docs.apify.com/api/client/python/
