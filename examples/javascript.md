# JavaScript / Node.js — B2B Lead Scraper

Call the hosted [B2B Lead Scraper](https://apify.com/logiover/b2b-lead-scraper) with the official [`apify-client`](https://docs.apify.com/api/client/js/) for JavaScript. These are **client-side** usage examples for the Actor on Apify.

## Install

```bash
npm install apify-client
```

## Run the Actor and read results

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'YOUR_APIFY_TOKEN' });

const input = {
    sector: 'dentists',
    country: 'Germany',
    maxLeads: 2000,
    verifyEmails: true,
};

const run = await client.actor('logiover/b2b-lead-scraper').call(input);

const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(`Fetched ${items.length} company leads`);

// Companies that have at least one MX-valid email
const emailable = items.filter((c) =>
    (c.companyEmails || []).some((e) => e.status?.endsWith('mx_valid')) ||
    (c.persons || []).some((p) => p.status?.endsWith('mx_valid'))
);
console.log(`${emailable.length} companies with a deliverable email`);
```

## Flatten decision-makers into an outreach list

```javascript
const { items } = await client.dataset(run.defaultDatasetId).listItems();

const contacts = [];
for (const c of items) {
    for (const p of c.persons || []) {
        if (p.email) contacts.push({ company: c.companyName, name: p.name, title: p.title, email: p.email, status: p.status });
    }
}
console.log(`${contacts.length} named contacts`);
console.log(contacts.slice(0, 5));
```

## Stream large datasets page by page

```javascript
let offset = 0;
const limit = 1000;
while (true) {
    const { items } = await client
        .dataset(run.defaultDatasetId)
        .listItems({ offset, limit });
    if (items.length === 0) break;
    // process(items)
    offset += items.length;
}
```

Client docs: https://docs.apify.com/api/client/js/
