# B2B Lead Scraper — Business Emails, Phones & Decision-Makers by Industry + Country (No API, No Login)

[![▶️ Run on Apify](https://img.shields.io/badge/▶️_Run_on_Apify-00A67E?style=for-the-badge&logo=apify&logoColor=white)](https://apify.com/logiover/b2b-lead-scraper)
[![Apify Actor](https://img.shields.io/badge/Apify-Actor-blue?logo=apify)](https://apify.com/logiover/b2b-lead-scraper)
[![No API key required](https://img.shields.io/badge/API_key-not_required-brightgreen)](https://apify.com/logiover/b2b-lead-scraper)
[![No login](https://img.shields.io/badge/login-not_required-brightgreen)](https://apify.com/logiover/b2b-lead-scraper)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

> **▶️ [Run the B2B Lead Scraper on Apify »](https://apify.com/logiover/b2b-lead-scraper)** — no code, no API key, no login, free to try.

**Pick a sector. Pick a country. Get thousands of fresh, verified B2B leads — with no API key and no login.** This is the documentation and client-usage guide for the [**B2B Lead Scraper**](https://apify.com/logiover/b2b-lead-scraper) hosted on the [Apify](https://apify.com) platform.

The Actor builds *brand-new* prospect lists in real time: it targets companies by **industry and location**, crawls each company's website, and extracts **business emails (verified by MX), decision-maker names & job titles, phone numbers, postal addresses and social profiles** — then returns one clean record per company. No cookies, no Apollo / ZoomInfo / Lusha / Hunter subscription and no per-credit paywall, so it's a smart, affordable **Apollo.io / ZoomInfo alternative** for teams who want *their own* proprietary lead database, generated on demand. Export to **JSON, CSV, Excel, JSONL or XML**.

- ✅ **Verified business emails** — found on-page and validated by MX + syntax, each labeled by confidence.
- 🧑‍💼 **Decision-maker contacts** — real names + job titles (CEO, Founder, Owner, Partner, Manager…) tied to their email.
- 🔤 **Email-pattern detection** — infers each company's format (`first.last@`, `flast@`…) so you can reconstruct outreach emails at scale.
- ☎️ **Phone numbers & 📮 postal address** — normalized phones plus street/city/postcode.
- 🌐 **Social profiles** — LinkedIn, Facebook, Instagram and X (Twitter) URLs.
- 🎯 **Sector + Country targeting** — precise firmographic focus, with optional city lists for local lead gen.
- 🔑 **No API key, no login, no cookies** — nothing to configure but an Apify account.
- 📤 **Export anywhere** — JSON, CSV, Excel, JSONL, XML, plus REST API, webhooks and scheduling.

---

## ▶️ Try it now

**[Open the Actor on Apify Store → apify.com/logiover/b2b-lead-scraper](https://apify.com/logiover/b2b-lead-scraper)**

Click **Try for free**, set a `sector` (e.g. `dentists`) and a `country` (e.g. `Germany`), and hit **Start**. In minutes you'll have a dataset of companies with contacts you can download as **CSV, JSON or Excel**. No credit card, no API key, no login.

---

## What you get / Output fields

Every run produces one clean **record per company** with these fields:

| Field | Type | Description |
| --- | --- | --- |
| `companyName` | string | Company / website name. |
| `domain` | string | Root domain. |
| `website` | string | Homepage URL. |
| `country` | string | Target country. |
| `sector` | string | Target sector. |
| `address` | string | Street address (from OpenStreetMap). |
| `searchQuery` | string | Search query that discovered this company. |
| `emailPattern` | string | Detected email format, e.g. `first.last`. |
| `companyEmails` | array | Generic / company-level emails, each with verification status. |
| `persons` | array | Decision-makers: `name`, `title`, `email`, `status`. |
| `phones` | array | Phone numbers (normalized & de-duplicated). |
| `socials.linkedin` | string | LinkedIn URL. |
| `socials.facebook` | string | Facebook URL. |
| `socials.instagram` | string | Instagram URL. |
| `socials.twitter` | string | X (Twitter) URL. |
| `pagesScraped` | number | Pages crawled for this company. |
| `scrapedAt` | string | ISO 8601 timestamp. |

**Email status labels:** `found_mx_valid` (found on site, domain accepts mail), `guessed_mx_valid` (built from pattern, domain accepts mail), `found` / `guessed` (verification off), `no_mx` (no mail server — filtered out by default). Pattern-built emails are always clearly labeled `guessed`, so you stay in control of deliverability.

## 💡 Use cases

- **Cold email & outbound sales** — fresh, targeted prospect lists for your SDRs.
- **Local lead generation** — find every dentist, lawyer, plumber or agency in a city / country.
- **Agency lead gen** — build client prospect lists on demand.
- **Recruiting** — source companies and decision-makers in any niche.
- **Market research & TAM mapping** — map an entire industry in a region.
- **CRM enrichment** — feed verified emails, phones & addresses into HubSpot, Salesforce or Pipedrive.
- **Direct mail & telemarketing** — high address & phone coverage even where email is scarce.

## 🚀 Quick start — four ways to run

### 1) Apify Console (no code)

Open **[the Actor page](https://apify.com/logiover/b2b-lead-scraper)**, click **Try for free**, set a `sector` (e.g. `law firms`) and a `country` (e.g. `United Kingdom`), add `cities` or bump `maxLeads` for volume, then **Start**. Results stream into the **Output** tab; export as CSV, JSON or Excel.

### 2) Apify CLI

```bash
npm install -g apify-cli
apify login   # paste your token from https://console.apify.com/account/integrations
apify call logiover/b2b-lead-scraper --input '{
  "sector": "dentists",
  "country": "Germany",
  "maxLeads": 2000,
  "verifyEmails": true
}'
```

More CLI recipes → [examples/cli.md](./examples/cli.md).

### 3) REST API (curl)

```bash
curl -X POST "https://api.apify.com/v2/acts/logiover~b2b-lead-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "sector": "marketing agencies",
    "country": "United States",
    "maxLeads": 500
  }'
```

More API recipes (async runs, polling, CSV/Excel export) → [examples/api-curl.md](./examples/api-curl.md).

### 4) apify-client (JavaScript & Python)

**JavaScript** — see [examples/javascript.md](./examples/javascript.md):

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'YOUR_APIFY_TOKEN' });
const run = await client.actor('logiover/b2b-lead-scraper').call({
    sector: 'law firms',
    country: 'United Kingdom',
    maxLeads: 1000,
});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(`Fetched ${items.length} company leads`);
```

**Python** — see [examples/python.md](./examples/python.md):

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_TOKEN")
run = client.actor("logiover/b2b-lead-scraper").call(run_input={
    "sector": "law firms",
    "country": "United Kingdom",
    "maxLeads": 1000,
})
items = list(client.dataset(run["defaultDatasetId"]).iterate_items())
print(f"Fetched {len(items)} company leads")
```

## 📥 Input

All fields are optional — just two to start: **sector** and **country**.

| Field | Type | Description |
| --- | --- | --- |
| `sector` | string (dropdown) | Industry/niche — `dentists`, `law firms`, `marketing agencies`, `plumbers`, `software companies`, and 20+ more. Need something not listed? Use `extraQueries`. |
| `country` | string (dropdown) | Target country — `United States`, `Germany`, `United Kingdom`, and 30+ more with OpenStreetMap coverage. |
| `cities` | array | *(optional)* Specific cities. If empty, major cities of the country are used automatically for volume. |
| `maxLeads` | integer | Max distinct companies to return (1–10,000). Scale up for thousands. |
| `resultsPerQuery` | integer | Search results taken per query before moving on (higher = more volume per city). |
| `discovery` | string (dropdown) | `osm` (recommended — high volume, never blocked, free) or `search` (search engines, flexible). |
| `maxPagesPerSite` | integer | Home + contact/about/team pages crawled per company. Keep low for speed & cost. |
| `maxProfilesPerSite` | integer | Individual people-profile pages to crawl (where named emails live). Higher = more person-level leads. |
| `requireEmail` | boolean | Keep only leads with at least one email. Default **off** for maximum volume. |
| `guessEmails` | boolean | Reconstruct likely personal emails from name + detected pattern (labeled `guessed`). Default **off** for precision. |
| `verifyEmails` | boolean | MX + syntax verification. Default **on**. |
| `smtpCheck` | boolean | Best-effort live SMTP RCPT check (port 25 is usually blocked on cloud, so this often returns `unknown`). |
| `extraQueries` | array | *(optional)* Extra raw search queries, e.g. `SaaS startups Berlin`, for niches not in the dropdown. |
| `extraStartUrls` | array | *(optional)* Skip discovery and crawl these company websites directly (enrichment mode). |
| `proxyConfiguration` | object | Apify Proxy settings (recommended to avoid search-engine rate limits). |

> **Tip — sharper targeting = better data.** Specific sectors beat broad ones (`personal injury lawyer` beats `lawyer`). Leave `cities` empty to auto-fan-out across a country's major cities for volume, or set them to focus a local campaign. Both `sector` and `country` are dropdowns in the Console.

### Example inputs

**Dentists in Germany (local lead gen)**

```json
{
  "sector": "dentists",
  "country": "Germany",
  "maxLeads": 2000,
  "verifyEmails": true
}
```

**Law firms in the UK, named-contact focus**

```json
{
  "sector": "law firms",
  "country": "United Kingdom",
  "maxProfilesPerSite": 15,
  "requireEmail": true,
  "guessEmails": true,
  "maxLeads": 1000
}
```

**Enrich your own list of company websites**

```json
{
  "extraStartUrls": ["https://acme.com", "example-agency.co.uk", "https://widgetco.de"],
  "verifyEmails": true
}
```

## 📤 Output — sample JSON item

```json
{
  "companyName": "Wolf Law, LLC",
  "domain": "wolflawcolorado.com",
  "website": "https://wolflawcolorado.com/",
  "country": "United States",
  "sector": "law firms",
  "address": "South Colorado Boulevard, 1385, Denver, 80222",
  "searchQuery": "law firms Denver",
  "emailPattern": "first",
  "companyEmails": [
    { "email": "info@wolflawcolorado.com", "source": "page", "status": "found_mx_valid" }
  ],
  "persons": [
    { "name": "Jeffrey Wolf", "title": "Attorney", "email": "jeffrey@wolflawcolorado.com", "emailSource": "page", "status": "found_mx_valid" }
  ],
  "phones": ["+17207898926"],
  "socials": { "linkedin": "https://linkedin.com/company/wolf-law" },
  "pagesScraped": 4,
  "scrapedAt": "2026-07-06T10:00:00.000Z"
}
```

> ℹ️ Coverage depends on the niche and how each business publishes its data — some return rich email + decision-maker data, others only phone + address. About 15–28% of B2B domains are "catch-all", where individual mailboxes can't be confirmed; those stay at `*_mx_valid`.

## 🔌 Integrations & automation

- **Schedules** — top up your prospect database weekly from the Apify Console.
- **Webhooks** — fire a webhook on run completion to trigger dedupe/verification and hand-off to your sequencer.
- **Google Sheets / Drive, Amazon S3, databases, HubSpot, Salesforce, Pipedrive** — push results through Apify integrations.
- **Zapier, Make, n8n, Pipedream** — connect via the Apify app or REST API to build automated outbound pipelines.
- **REST API** — start runs, poll status and pull the dataset programmatically (see [examples/api-curl.md](./examples/api-curl.md)).

## 📦 Export formats

Every run's dataset can be exported as **CSV, JSON, JSONL, Excel (XLSX) or XML**, from the Apify Console or via the API by adding `&format=csv` (or `xlsx`, `xml`, `jsonl`) to the dataset items URL.

## ❓ FAQ

### How do I scrape business emails by industry and country?
Open the [B2B Lead Scraper](https://apify.com/logiover/b2b-lead-scraper), set a `sector` (e.g. `dentists`, `law firms`) and a `country`, then run. The Actor targets matching companies, crawls their sites and extracts verified business emails, phones and named decision-makers by sector + country — no API key required.

### Is this a free Apollo.io / ZoomInfo alternative without a subscription?
It builds your own B2B lead list — verified emails, phones, decision-maker names — without an Apollo, ZoomInfo, Lusha or Hunter account, no cookies and no export credits. You pay only for your Apify usage.

### Can I scrape leads without an API key or login?
Yes. There are no logins, no cookies and no captchas to solve — only an Apify account.

### Can I enrich my own list of company websites?
Yes. Paste your domains into `extraStartUrls` to skip discovery and crawl those sites directly, pulling emails, phone numbers and named contacts for CRM enrichment.

### Will every company have an email?
No — coverage depends on the niche and how each business publishes its data. Turn `requireEmail` on to keep only email-bearing leads, or off for maximum volume (name, phone, address, website).

### How fresh is the data?
100% real-time. Every lead is crawled live at the moment you run the Actor — never a recycled list.

### How many leads can I get?
Increase `maxLeads` — the engine scales to thousands of companies per run. Use specific sectors for sharper targeting, and add `cities` or `extraQueries` for more reach.

### How do I export verified business leads to Excel or CSV?
Every run's dataset exports straight to **CSV, JSON, JSONL, Excel or XML**, or pushes to your CRM via the Apify API and integrations. Add `&format=csv` / `&format=xlsx` to the dataset API URL.

### Is web scraping legal?
The Actor collects publicly available business information. You are responsible for using the data in compliance with GDPR, CAN-SPAM, CASL and other applicable regulations and the target sites' terms.

### Is it free? How much does it cost?
The Actor runs on a pay-per-result model with a free tier to try it. See the **Pricing** tab on the [Actor page](https://apify.com/logiover/b2b-lead-scraper) for the current rate.

## 🔗 Related actors by logiover

- **[Website Contact Scraper](https://apify.com/logiover/website-contact-scraper)** — emails, phones & socials from any list of websites.
- **[Bulk Email Verifier](https://apify.com/logiover/bulk-email-verifier)** — validate email lists by syntax, MX & deliverability.

👉 Browse all **[logiover scrapers on Apify Store](https://apify.com/logiover)** — 180+ actors across real estate, jobs, crypto, social media & B2B data.

---

📄 **Documentation only** — this repository contains no scraper source code. The Actor runs on the Apify platform. **▶️ Run it: [apify.com/logiover/b2b-lead-scraper](https://apify.com/logiover/b2b-lead-scraper)**

Licensed under the [MIT License](./LICENSE) · © 2026 logiover
