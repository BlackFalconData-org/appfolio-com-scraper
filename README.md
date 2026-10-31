# AppFolio Rental Listings Scraper

Extract structured data from [appfolio.com](https://appfolio.com) — rental listings from any property manager on AppFolio: rent, beds, baths and square feet, full address with coordinates, photos, pet policy and amenities as clean rows. Filter by radius around a point, or paste a listings page. No browser needed.

**[AppFolio Rental Listings Scraper on Apify →](https://apify.com/blackfalcondata/appfolio-com-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword and location. Filter by 🐈 cats, 🐕 dogs, ↕️ sort by, and more.

**Detail enrichment** — Fetch full descriptions, contact information for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N listings (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from appfolio.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from appfolio.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "apg",
  "maxResults": 100,
  "includeDetails": false,
  "descriptionMaxLength": 0,
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
| `query` | string | — | The property manager's AppFolio site: subdomain (`apg`), host (`apg.appfolio.com`), or full listings URL. One per run, or several via Start URLs. Required if no startUrls provided. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste an AppFolio listings page (https://<site>.appfolio.com/listings), a single listing page (.../listings/detail/<id>), or the property manager's own website address when it forwards to its AppFolio listings. Each URL becomes its own task; results are merged and deduped. When provided, startUrls REPLACE the query field; unsupported URLs are skipped, and if none is supported the run stops with an input error. |
| `location` | string | — | Keep only listings whose city, state, ZIP or address contains this text (e.g. `Phoenix` or `85004`). Leave empty for the site's whole portfolio. |
| `includeKeywords` | array | — | Keep only listings whose title, description, address, appliances, pet policy or amenities mention at least one of these words (case-insensitive), e.g. `garage` or `washer`. |
| `excludeKeywords` | array | — | Drop listings that mention any of these words (case-insensitive), e.g. `shared` or `senior`. |
| `minRent` | integer | — | Minimum monthly rent in USD. Listings without a published rent are dropped while a rent bound is set. |
| `maxRent` | integer | — | Maximum monthly rent in USD. |
| `minBedrooms` | integer | — | Minimum bedrooms (0 = studio). |
| `maxBedrooms` | integer | — | Maximum bedrooms. |
| `minBathrooms` | number | — | Minimum bathrooms (half baths allowed, e.g. 1.5). |
| `maxBathrooms` | number | — | Maximum bathrooms. |
| `minSqft` | integer | — | Minimum floor area in square feet. Listings without a published size are dropped while a size bound is set. |
| `maxSqft` | integer | — | Maximum floor area in square feet. |
| `cities` | array | — | Keep only listings in these cities (exact city name, case-insensitive). |
| `postalCodes` | array | — | Keep only listings in these 5-digit ZIP codes. |
| `catPolicy` | enum | `"any"` | Keep only listings whose pet policy allows cats, or says no cats/no pets. |
| `dogPolicy` | enum | `"any"` | Keep only listings whose pet policy allows dogs, allows small dogs, or says no dogs/no pets. |
| `desiredMoveIn` | string | — | Keep only listings available on or before this date (listings marked NOW, and listings without a date, are kept). |
| `requireApplicationUrl` | boolean | `false` | Keep only listings that link to an online rental application. |
| `sortBy` | enum | `"site_order"` | Order of the results: as the site lists them, or by rent. |
| `includeDetails` | boolean | `false` | Also open each listing's own page for the full description, photo gallery, office contact, rental terms and amenities. Off by default: the site asks crawlers to wait 10 seconds between requests, so this adds about 10 seconds per listing. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |
| `lat` | number | — | Latitude of the centre point. Set together with longitude to keep only listings within the radius; listings without coordinates are dropped. |
| `lon` | number | — | Longitude of the centre point. Needs latitude as well, otherwise the run stops with an input error. |
| `radiusKm` | integer | — | Search radius in kilometres around latitude/longitude. Defaults to 10 when a centre point is set. |

---

## Output fields

Every listing returns the same 49-field schema. Missing values are `null` — never omitted.

- `platform`
- `id`
- `tenant`
- `scrapedAt`
- `listingId`
- `url`
- `portalUrl`
- `sourceUrl`
- `sourceListingId`
- `title`
- `description`
- `address`
- `street`
- `city`
- `state`
- `zip`
- `latitude`
- `longitude`
- `rent`
- `rentFormatted`
- `currency`
- `beds`
- `baths`
- `sqft`
- `available`
- `appliances`
- `petPolicy`
- `image`
- `images`
- `applyUrl`
- `amenities`
- `utilities`
- `location`
- `unitSpecs`
- `summary`
- `officeName`
- `phone`
- `officeLogo`
- `rentalTerms`
- `applicationFee`
- `sections`
- `showingAvailable`
- `contactUrl`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "platform": "appfolio",
  "id": "apg.appfolio.com:df123dcd-550b-4a8e-91f0-103cf454bdc4",
  "tenant": "apg.appfolio.com",
  "scrapedAt": "2026-10-31T00:50:55.873Z",
  "listingId": "df123dcd-550b-4a8e-91f0-103cf454bdc4",
  "url": "https://apg.appfolio.com/listings/detail/df123dcd-550b-4a8e-91f0-103cf454bdc4",
  "portalUrl": "https://apg.appfolio.com/listings/detail/df123dcd-550b-4a8e-91f0-103cf454bdc4",
  "sourceUrl": "https://apg.appfolio.com/listings",
  "sourceListingId": "676",
  "title": "Sunny 5 Bedroom 3 Bath with W/D and Private Deck in the Mission!",
  "description": "1196 Treat Ave. offers a good space 5-bedroom, 3-bathroom residence located in San Francisco's vibrant Mission District. Featuring a generous layout, comfortable living spaces, and…",
  "address": "1196 Treat Ave, San Francisco, CA 94110"
}
```

*Truncated — full records contain 49 fields. See Output fields for the complete schema.*

**[Try AppFolio Rental Listings Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/appfolio-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.0009 |

See the [actor on Apify](https://apify.com/blackfalcondata/appfolio-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape appfolio.com?**
Use this actor on Apify to extract structured data from appfolio.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get appfolio.com data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape appfolio.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check appfolio.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [Djinni Scraper](https://apify.com/blackfalcondata/djinni-scraper?fpr=1h3gvi) — Ukraine & CEE tech jobs with salary and English level
- [Work.ua Scraper](https://apify.com/blackfalcondata/work-ua-scraper?fpr=1h3gvi) — Ukraine's work.ua job listings
- [NoFluffJobs Scraper](https://apify.com/blackfalcondata/nofluffjobs-scraper?fpr=1h3gvi) — Poland & CEE IT jobs with transparent salary ranges
- [OLX Jobs Scraper](https://apify.com/blackfalcondata/olx-jobs-scraper?fpr=1h3gvi) — OLX job listings across 7 CEE countries
- [Prace.cz Scraper](https://apify.com/blackfalcondata/prace-scraper?fpr=1h3gvi) — Czech Republic's prace.cz job board
- [Profession.hu Scraper](https://apify.com/blackfalcondata/profession-scraper?fpr=1h3gvi) — Hungary's profession.hu job board

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [AppFolio Rental Listings Scraper](https://apify.com/blackfalcondata/appfolio-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
