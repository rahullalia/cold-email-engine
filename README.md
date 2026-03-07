# Cold Email Engine

A Claude Code skill covering the full cold email pipeline — from defining your target market to running and optimizing live campaigns.

## What It Covers

1. **ICP & Market Validation** — Define who to target and confirm the market is worth pursuing
2. **Offer Creation** — Build an offer positioned for cold traffic, not just warm referrals
3. **Lead Sourcing** — Scrape, verify, and enrich contact lists using Apify and similar tools
4. **Email Copywriting** — SPARK framework, pattern-breaking openers, two-thread sequences
5. **Campaign Setup** — Configure Instantly for maximum deliverability
6. **Analysis & Optimization** — 6-layer metrics stack, diagnosis tree, and scaling playbook

## How to Install

Drop the `coldEmailEngine/` folder into your `.claude/skills/` directory.

The skill auto-triggers in Claude Code when you mention cold email, outbound, lead generation, prospecting, ICP building, offer positioning, email sequences, reply rates, deliverability, or anything related to generating business through outbound email.

## Structure

```
coldEmailEngine/
  SKILL.md              # Main orchestrator
  references/
    icpAndSignals.md    # ICP Matrix, FITS scoring, signal taxonomy
    offerCreation.md    # Value Equation, 8-step offer builder
    copyFrameworks.md   # SPARK, archetypes, pattern breaking, sequences
    campaignOps.md      # Sending settings, deliverability, benchmarks
  evals/
    evals.json          # Test prompts and assertions
```

## Usage

Start a conversation about cold email and the skill activates. It's interactive — it asks questions, challenges weak assumptions, and guides you through each phase. You can jump directly into any phase based on where you are in the process.

## License

MIT
