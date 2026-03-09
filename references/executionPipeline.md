# Execution Pipeline Reference

API details, actor IDs, endpoints, request/response formats, and error handling for the cold email execution pipeline. This is the technical backbone of Phase 2.

Credentials are loaded from `~/lalia/4-Resources/mcp/coldEmail.env`.

---

## Step 1: Apify

### Actor Selection by ICP

| ICP Type | Actor ID | Best For |
|----------|----------|----------|
| Local service businesses | `compass/google-maps-scraper` | Restaurants, contractors, salons, gyms, cleaning companies — anything on Google Maps |
| Yelp-listed businesses | `tri_angle/yelp-scraper` | Businesses where Yelp is the primary review source |
| Agencies / SaaS | `apify/website-content-crawler` | B2B companies, tech companies |
| Any domain (email extraction) | `apify/website-content-crawler` | Follow-up pass on a list of domains to extract contact emails |
| General web scraping | `apify/web-scraper` | Custom use cases with specific page structure |

**Standard workflow for local service businesses (two-step):**
1. Run `compass/google-maps-scraper` → get business profiles with websites
2. Run `apify/website-content-crawler` on each website URL → extract contact emails

### Apify API Calls

**Run an actor:**
```
POST https://api.apify.com/v2/acts/{actorId}/runs
Authorization: Bearer {APIFY_API_TOKEN}
Content-Type: application/json

Body: { actor input JSON }
```

**Check run status:**
```
GET https://api.apify.com/v2/actor-runs/{runId}
Authorization: Bearer {APIFY_API_TOKEN}
```
Poll every 15 seconds. Status values: `RUNNING`, `SUCCEEDED`, `FAILED`, `ABORTED`.

**Download dataset:**
```
GET https://api.apify.com/v2/actor-runs/{runId}/dataset/items?format=json
Authorization: Bearer {APIFY_API_TOKEN}
```

### Actor Input Schemas

**Google Maps Scraper (`compass/google-maps-scraper`):**
```json
{
  "searchStringsArray": ["plumbers in Sacramento CA", "plumbing companies Sacramento"],
  "maxCrawledPlacesPerSearch": 200,
  "language": "en",
  "countryCode": "us",
  "includeWebResults": false,
  "scrapeDirectories": false,
  "deeperCityScrape": false
}
```
Key output fields: `title`, `website`, `phone`, `address`, `reviewsCount`, `totalScore`, `categoryName`, `email` (if on GMB listing)

**Website Content Crawler (`apify/website-content-crawler`):**
```json
{
  "startUrls": [{"url": "https://example.com"}],
  "maxCrawlPages": 5,
  "crawlerType": "cheerio",
  "includeUrlGlobs": ["**/contact*", "**/about*"],
  "removeCookieWarnings": true
}
```
For email extraction from multiple domains, batch the `startUrls` array (up to 100 at a time).

**Yelp Scraper (`tri_angle/yelp-scraper`):**
```json
{
  "searchTerms": ["cleaning services"],
  "locations": ["Sacramento, CA"],
  "maxItems": 200,
  "scrapeReviews": true,
  "maxReviews": 10
}
```

### Credit Estimation

Before running, estimate credits:
- Google Maps Scraper: ~1 credit per result (200 results = ~200 credits)
- Website Content Crawler: ~1-3 credits per domain (depends on pages crawled)
- Yelp Scraper: ~2 credits per result

Tell the user: "This run will use approximately X Apify credits. Continue?"

### Error Handling

- `FAILED` run → Check the run log via `GET /v2/actor-runs/{runId}/log`. Common issues: rate limiting, search query too broad, location not found.
- Empty dataset → Broaden the search query or try a different actor.
- Partial results → Run may have hit a timeout. Download what's available and re-run for remaining.

---

## Step 3: ZeroBounce

### Batch Verification API

**Endpoint:**
```
POST https://bulkapi.zerobounce.net/v2/validatebatch
Content-Type: application/json

{
  "api_key": "{ZEROBOUNCE_API_KEY}",
  "email_batch": [
    {"email_address": "test@example.com", "ip_address": ""},
    {"email_address": "another@example.com", "ip_address": ""}
  ]
}
```

Max 100 emails per batch request. For larger lists, chunk into batches of 100 and run sequentially.

**Response format:**
```json
{
  "email_batch": [
    {
      "address": "test@example.com",
      "status": "valid",
      "sub_status": "",
      "free_email": false,
      "did_you_mean": "",
      "account": "test",
      "domain": "example.com",
      "domain_age_days": "9692",
      "smtp_provider": "google",
      "mx_found": "true",
      "mx_record": "aspmx.l.google.com",
      "firstname": "",
      "lastname": "",
      "gender": "",
      "country": null,
      "region": null,
      "city": null,
      "zipcode": null,
      "processed_at": "2024-01-01 00:00:00.000"
    }
  ],
  "errors": []
}
```

**Status values and actions:**

| Status | Action |
|--------|--------|
| `valid` | Keep — primary list |
| `invalid` | Remove |
| `catch-all` | Keep — secondary "risky" segment |
| `unknown` | Remove |
| `spamtrap` | Remove immediately |
| `abuse` | Remove |
| `do_not_mail` | Remove |

### Credit Check

Before running verification:
```
GET https://api.zerobounce.net/v2/getcredits?api_key={ZEROBOUNCE_API_KEY}
```
Response: `{"Credits": "1000"}` — each email verification costs 1 credit.

Tell the user: "Verification will use X ZeroBounce credits. You have Y remaining. Continue?"

### Error Handling

- `402` response → Out of credits. Stop and tell user.
- `400` response → Malformed request. Check email format in batch.
- Network timeout → Retry the failed batch once before raising an error.

---

## Step 6: Instantly

### API Base URL

`https://api.instantly.ai/api/v1`

All requests: `Authorization: Bearer {INSTANTLY_API_KEY}`

### Create Campaign

```
POST /campaign/create
Content-Type: application/json

{
  "name": "HomeSvc Sacramento - Referral Ceiling - v1",
  "campaign_schedule": {
    "schedules": [
      {
        "name": "Default Schedule",
        "days": {
          "1": true,  // Monday
          "2": true,  // Tuesday
          "3": true,  // Wednesday
          "4": true,  // Thursday
          "5": false, // Friday
          "6": false, // Saturday
          "0": false  // Sunday
        },
        "start_hour": "08",
        "start_minute": "00",
        "end_hour": "11",
        "end_minute": "00",
        "timezone": "America/Los_Angeles"
      }
    ]
  }
}
```

Returns: `{ "id": "campaign_id" }`

### Update Campaign Settings

```
POST /campaign/update/settings
Content-Type: application/json

{
  "campaign_id": "{campaign_id}",
  "stop_on_reply": true,
  "stop_on_auto_reply": false,
  "text_only": true,
  "link_tracking": false,
  "open_tracking": false,
  "email_gap": 15,
  "random_wait_max": 10,
  "daily_limit": 30
}
```

### Add Leads

```
POST /lead/add
Content-Type: application/json

{
  "campaign_id": "{campaign_id}",
  "skip_if_in_workspace": true,
  "leads": [
    {
      "email": "owner@example.com",
      "first_name": "John",
      "last_name": "Smith",
      "company_name": "Smith Plumbing",
      "personalization": "Noticed your Google listing mentions same-day service but your last 3 reviews mention 2-3 day wait times — that gap is worth fixing before it tanks your ranking.",
      "custom_variables": {
        "city": "Sacramento",
        "tier": "T1"
      }
    }
  ]
}
```

Max 1000 leads per request. Chunk for larger lists.

### Add Email Sequence

```
POST /campaign/sequence/patch
Content-Type: application/json

{
  "campaign_id": "{campaign_id}",
  "sequence_steps": [
    {
      "type": "email",
      "step_number": 1,
      "delay": 0,
      "variants": [
        {
          "subject": "sacramento plumbers",
          "body": "{{personalization}}\n\n[Body copy here]\n\n[CTA]",
          "weight": 50
        },
        {
          "subject": "quick question",
          "body": "{{personalization}}\n\n[Variant B body]\n\n[CTA]",
          "weight": 50
        }
      ]
    },
    {
      "type": "email",
      "step_number": 2,
      "delay": 3,
      "variants": [...]
    }
  ]
}
```

### Set Campaign Status (Paused)

```
POST /campaign/update/status
Content-Type: application/json

{
  "campaign_id": "{campaign_id}",
  "status": "paused"
}
```

**Always set to paused after setup. Never activate programmatically.**

### Pull Analytics (for Phase 5)

```
GET /campaign/analytics?campaign_id={campaign_id}&start_date=2024-01-01&end_date=2024-12-31
Authorization: Bearer {INSTANTLY_API_KEY}
```

Returns per-step breakdown: sends, opens, replies, bounces.

### Error Handling

- `401` → Invalid API key. Check `INSTANTLY_API_KEY` in coldEmail.env.
- `429` → Rate limited. Wait 60 seconds and retry.
- `400` on lead add → Check for malformed email addresses or missing required fields.
- Campaign not found → Verify campaign_id from the create response.

---

## CSV Format for Instantly Import

If pushing via CSV instead of API (fallback):

| Column | Field |
|--------|-------|
| `Email` | email address |
| `First Name` | first name |
| `Last Name` | last name |
| `Company` | company name |
| `Personalization` | custom opening line |
| `City` | city |
| `Tier` | T1, T2, T3 (for your reference) |

---

## Rate Limits Summary

| Service | Limit |
|---------|-------|
| Apify | No hard limit on API calls; credits-based |
| ZeroBounce | 100 emails per batch request; credits-based |
| Instantly | 10 requests/second |

---

## Credential File Format

`~/lalia/4-Resources/mcp/coldEmail.env`:
```
APIFY_API_TOKEN=apify_api_xxxxxxxxxxxx
ZEROBOUNCE_API_KEY=xxxxxxxxxxxxxxxxxxxx
INSTANTLY_API_KEY=xxxxxxxxxxxxxxxxxxxx
```
