# Cold Email Engine

A Claude Code skill that turns Claude into an end-to-end cold email execution agent. It doesn't just advise — it runs the pipeline.

## What It Does

Most cold email tooling is either a thinking framework or a set of automations. This skill combines both. Claude asks the right intake questions, executes the technical pipeline (scrape → verify → qualify → personalize → push), drafts copy for your review, and diagnoses campaigns when they underperform.

**The pipeline:**

1. **Prerequisites check** — warmup gate, domain authentication, Instantly connection
2. **Intake** — tool selection, ICP, offer, targeting
3. **List building** — Apify (Google Maps, Yelp) for local/SMB, or CSV from Apollo / Sales Navigator / Clay for B2B
4. **Verification** — ZeroBounce or NeverBounce batch API, pre-flight credit check
5. **AI qualification** — two-pass: fast scoring on scraped data, then targeted WebFetch on the top candidates (capped at 100)
6. **Personalization** — custom line per T1 lead, bucket openers for T2, industry openers for T3
7. **Campaign setup** — push to Instantly via API, always set to paused for your review
8. **Copy review** — 4-step two-thread sequence drafted and presented for human approval
9. **Analysis** — pull Instantly analytics, run diagnosis, give one clear next action

## Frameworks

The strategy in this skill draws from:

- **Alex Hormozi's $100M Offers** — Value Equation, offer construction, guarantee design, risk reversal, the offer fatigue rotation order
- **ColdIQ's GTM Flywheel** — ICP qualification, signal detection, personalization hierarchy, the SPARK copy framework, two-thread sequence architecture, deliverability benchmarks, 6-layer metrics stack

The execution layer (Apify → ZeroBounce → Instantly pipeline, two-pass qualification, tool-agnostic routing) is original to this skill.

## Prerequisites

**Accounts you need:**

| Tool | Purpose | Required? |
|------|---------|-----------|
| [Apify](https://apify.com) | Lead scraping (Google Maps, Yelp, websites) | If your ICP is local/SMB |
| [Apollo](https://apollo.io) / LinkedIn Sales Navigator / Clay | Lead export for B2B ICPs | If your ICP is B2B |
| [ZeroBounce](https://zerobounce.net) or [NeverBounce](https://neverbounce.com) | Email verification | Yes |
| [Instantly](https://instantly.ai) | Campaign management and sending | Yes |

**Sending infrastructure:**

- Sending inboxes warmed for at least 3 weeks before running any campaign
- SPF, DKIM, and DMARC configured on all sending domains
- All inboxes connected and active in Instantly

The skill enforces these as a hard gate. If your infrastructure isn't ready, it will stop and tell you what to fix.

## Setup

**1. Install the skill**

Drop the `coldEmailEngine/` folder into your Claude Code skills directory:

```
~/.claude/skills/coldEmailEngine/
```

**2. Create a credentials file**

Create a `coldEmail.env` file in your working directory (or wherever you run Claude Code from):

```
APIFY_API_TOKEN=your_apify_token
ZEROBOUNCE_API_KEY=your_zerobounce_key
INSTANTLY_API_KEY=your_instantly_key
```

Add `coldEmail.env` to your `.gitignore`. Never commit credentials.

**3. Start a conversation**

The skill auto-triggers in Claude Code when you mention cold email, outbound, lead generation, prospecting, ICP building, offer positioning, email sequences, reply rates, deliverability, list building, or booking meetings.

## File Structure

```
coldEmailEngine/
  SKILL.md                    # Main orchestrator — phases, routing, interaction rules
  references/
    icpAndSignals.md          # ICP qualification, two-pass scoring, buying signals
    offerCreation.md          # Value Equation, 8-step offer builder, MAGIC naming
    copyFrameworks.md         # SPARK, archetypes, pattern breaking, Email 3 paths
    campaignOps.md            # Sending settings, deliverability thresholds, benchmarks
    executionPipeline.md      # Full API reference: Apify, ZeroBounce, NeverBounce, Instantly
  evals/
    evals.json                # Test prompts and assertions
```

## How Campaigns Work

The skill creates campaigns in Instantly but **never activates them**. Every campaign launches paused. You review the copy, do a test send, and press go yourself.

Copy is always presented for your approval before it touches Instantly. The skill drafts it, you own it.

## License

MIT
