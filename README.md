# OnTheMarket Scraper [💰$0.72]

Extract structured data from [onthemarket.com](https://onthemarket.com) — onthemarket.com property listings for sale or to rent anywhere in the UK. Every record has the price, bedrooms and full address with map coordinates plus photos and the listing agent's name and phone. Filter by place, price and bedrooms or paste a search URL.

**[OnTheMarket Scraper [💰$0.72] on Apify →](https://apify.com/blackfalcondata/onthemarket-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword. Filter by 🏷️ for sale or to rent, 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N listings (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from onthemarket.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from onthemarket.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "london",
  "maxResults": 100,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | OnTheMarket location to search — a town, city, borough or area, e.g. `london`, `manchester`, `bath`. Paste a JSON array for several at once, e.g. `["london","brighton"]`. Required if no Start URLs are provided. |
| `searchType` | enum | `"for-sale"` | Search properties for sale (default) or to rent. A start URL keeps its own type. |
| `minPrice` | integer | — | Only properties at or above this price. For rentals this is the monthly rent. Leave empty for no minimum. |
| `maxPrice` | integer | — | Only properties at or below this price. For rentals this is the monthly rent. Leave empty for no maximum. |
| `minBedrooms` | integer | — | Only properties with at least this many bedrooms (0 = studio). Leave empty for no minimum. |
| `maxBedrooms` | integer | — | Only properties with at most this many bedrooms. Leave empty for no maximum. |
| `maxResults` | integer | `100` | Maximum total properties to return (0 = unlimited). |
| `includeDetails` | boolean | `false` | Open each property page for the full description, EPC rating, floor area, council tax band, ground rent, service charge, nearby stations and schools, and floorplans. Off by default: the run then delivers the search-results record (address, price, bedrooms, bathrooms, agent, photos, location) faster and cheaper. Pasted property page URLs always include the details. |
| `startUrls` | array | `[]` | Paste onthemarket.com search-results URLs (their place, filters and sort order are kept) or property page URLs (https://www.onthemarket.com/details/<id>/). Each search URL becomes its own search task; results are merged and deduped by property ID across all URLs. When provided, startUrls REPLACE the search field (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from your search inputs (searches, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip properties whose content matches a listing that disappeared in a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit properties whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit properties that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED properties. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of properties included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "447700900123"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every listing returns the same 47-field schema. Missing values are `null` — never omitted.

- `site`
- `listingId`
- `propertyId`
- `url`
- `portalUrl`
- `title`
- `propertyType`
- `transactionType`
- `price`
- `currency`
- `priceText`
- `priceQualifier`
- `shortPrice`
- `bedrooms`
- `bathrooms`
- `address`
- `outcode`
- `latitude`
- `longitude`
- `features`
- `tenure`
- `images`
- `coverImage`
- `addedOrReduced`
- `label`
- `reduced`
- `recentlyAdded`
- `onlyOnOnTheMarket`
- `spotlight`
- `premium`
- `virtualTour`
- `agentName`
- `agentId`
- `agentPhone`
- `agentLogoUrl`
- `agentUrl`
- `developmentFeatures`
- `searchLocation`
- `position`
- `detailsFetched`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `changeType`
- `contentHash`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "site": "onthemarket.com",
  "listingId": "d76bf1fd1a2e3b19802668b18dd1bc8066f255404559c2cf1805b524e630669a",
  "propertyId": "19929394",
  "url": "https://www.onthemarket.com/details/19929394/",
  "portalUrl": "https://www.onthemarket.com/details/19929394/",
  "title": "5 bedroom terraced house for sale",
  "propertyType": "Terraced house",
  "transactionType": "for-sale",
  "price": 1750000,
  "currency": "GBP",
  "priceText": "£1,750,000",
  "priceQualifier": "Guide price"
}
```

*Truncated — full records contain 47 fields. See Output fields for the complete schema.*

**[Try OnTheMarket Scraper [💰$0.72] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/onthemarket-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.000045 |
| Result | $0.00072 |

See the [actor on Apify](https://apify.com/blackfalcondata/onthemarket-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape onthemarket.com?**
Use this actor on Apify to extract structured data from onthemarket.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get onthemarket.com data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape onthemarket.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check onthemarket.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [OnTheMarket Scraper [💰$0.72]](https://apify.com/blackfalcondata/onthemarket-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
