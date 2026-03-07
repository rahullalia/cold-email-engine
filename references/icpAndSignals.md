# ICP, Qualification & Signals Reference

Consolidated from ICP Matrix Builder, Account Qualification (FITS), Intent Signals, Trigger Mapping, Lead Prioritization, and Persona Development. This is the reference for Phase 1 of the Cold Email Engine.

---

## The ICP Matrix: 5 Dimensions

### Dimension 1: Firmographic Fit

| Attribute | Tier 1 (Best) | Tier 2 (Good) | Tier 3 (OK) | DQ |
|-----------|---------------|---------------|-------------|-----|
| Company size | [define] | [define] | [define] | Too small/large |
| Revenue | [define] | [define] | [define] | Outside range |
| Industry | Primary verticals | Adjacent | Emerging | No fit |
| Geography | Primary markets | Secondary | Expansion | Restricted |
| Business model | Exact match | Similar | Adaptable | Incompatible |
| Growth stage | Ideal stage | Adjacent | Possible | Wrong stage |

**How to fill this in:**
1. Start with your best 10 customers — what do they share?
2. Look at your worst 10 — what disqualified them in hindsight?
3. Patterns in best but NOT worst = Tier 1 criteria

### Dimension 2: Technographic Fit

| Signal | Strength | Why |
|--------|----------|-----|
| Uses your integration partners | Strong | Lower friction |
| Uses a competitor | Strong | Active buyer, proven budget |
| Tech stack complexity matches | Medium | Right sophistication |
| Recently adopted adjacent tools | Medium | Active buying cycle |

### Dimension 3: Buying Intent Signals

| Signal | Intent Level | Weight |
|--------|-------------|--------|
| Hiring for role your service supports | Very High | 5x |
| Recent funding | High | 4x |
| Competitor evaluation (G2, reviews) | Very High | 5x |
| Content consumption | Medium-High | 3x |
| Website visits | High | 4x |
| Leadership change in relevant dept | Medium | 2x |

### Dimension 4: Persona Mapping

For each ICP tier, define 2-3 target personas:

| Attribute | What to Define |
|-----------|---------------|
| Title patterns | All variants (VP Sales, Vice President Revenue, Head of Sales) |
| Department | Sales, Marketing, RevOps, Growth |
| Seniority | C-Suite, VP, Director, Manager |
| Decision role | Economic buyer, Champion, Influencer, Blocker |
| Pain points | 3-5 specific, measurable pains |
| Buying triggers | Events that push them to evaluate |
| Common objections | Top 3-5 reasons they say no |

### Dimension 5: Engagement Readiness

| Factor | High Readiness | Low Readiness |
|--------|---------------|---------------|
| Previous interactions | Engaged before | Cold |
| Outbound receptiveness | SMB/mid-market, growth-stage | Enterprise, gatekept |
| Channel preference | Active on LinkedIn, responsive | Dark social |
| Sales cycle | < 60 days | 6+ months |
| Committee size | 1-3 decision makers | 5+ stakeholders |

---

## The FITS Qualification Framework

Score every lead across 4 dimensions (100 points total):

| Dimension | Weight | Points |
|-----------|--------|--------|
| **F** — Firmographic Fit | 25% | 0-25 |
| **I** — Intent Signals | 35% | 0-35 |
| **T** — Technographic Match | 20% | 0-20 |
| **S** — Structural Readiness | 20% | 0-20 |

### Tier Assignment

| Score | Tier | Action |
|-------|------|--------|
| 80-100 | Tier 1: Bullseye | Hyper-personalized, multi-channel |
| 60-79 | Tier 2: Strong Fit | Signal-based personalization |
| 40-59 | Tier 3: Good Fit | Bucket personalization |
| 20-39 | Tier 4: Stretch | Small batch test only |
| 0-19 | DQ | Do not contact |

### Hard Disqualifiers (instant remove)

- Outside serviceable company size range
- Excluded industry
- Restricted geography
- Already a customer
- Direct competitor
- Invalid email
- On do-not-contact list
- Duplicate in active campaign

---

## Intent Signal Taxonomy

### Category 1: First-Party (Highest Reliability)
Signals involving YOUR brand: website visits, content downloads, webinar attendance, previous outreach replies, demo requests.

### Category 2: Third-Party (High Reliability)
Signals on other platforms: G2/Capterra research, competitor alternative searches, topic-based intent data, community questions.

### Category 3: Public (Medium Reliability, Free)
Observable events anyone can detect:

| Signal | Reliability | Urgency | Source |
|--------|------------|---------|--------|
| Hiring for target role | High | High | LinkedIn Jobs, Indeed, careers page |
| Funding | High | Medium-High | Crunchbase, press |
| Leadership changes | Medium-High | Medium | LinkedIn, press |
| Product launches | Medium | Medium | Blog, Product Hunt, PR |
| Tech stack changes | Medium | Medium-High | BuiltWith, job descriptions |
| M&A activity | High | Medium | News, SEC filings |
| Office expansion | Medium | Medium | News, job listings |

### Category 4: Social (Variable)
Content published or engaged with: LinkedIn posts about pain, competitor content shares, vendor recommendation requests, role changes.

### Signal Scoring

```
Signal Score = (Recency x 3) + (Directness x 3) + (Frequency x 2) + (Specificity x 1) + (Source x 1)
```

Each factor rated 1-5. Max score: 50.

| Score | Priority | Action |
|-------|----------|--------|
| 40-50 | Critical | Contact within 24 hours |
| 30-39 | High | Contact within 3 days |
| 20-29 | Medium | Next campaign batch |
| 10-19 | Low | Monitor |
| < 10 | Noise | Ignore |

### Signal Decay

Signals lose value over time. A funding round from last week is urgent. From 6 months ago, it's a fact, not a signal.

- Hiring signals: stale after 4 weeks
- Funding: stale after 30 days (useful context up to 90)
- Leadership change: window is day 14-60 of new role
- Content publication: within 48 hours or it's creepy
- Competitor churn: immediate (highest urgency trigger)

---

## 12 Core Trigger Events

| # | Trigger | Urgency Window | Message Angle | Target Persona |
|---|---------|---------------|---------------|----------------|
| 1 | New funding | Within 2 weeks | "Post-{{round}}, most teams invest in [category] within 90 days" | CEO (Seed), VP Sales (A+) |
| 2 | Hiring target role | Within 1 week | "Hiring {{role}} usually means you're building the engine" | Hiring manager or their boss |
| 3 | Leadership change | Day 14-60 | "First 90 days in a new role..." | The new executive |
| 4 | Tech stack change | Within 2 weeks | "Teams migrating to {{tool}} usually rethink {{category}}" | Stack owner |
| 5 | Competitor churn | Within 48 hours | "Moving off {{competitor}}? Here's how we handle that" | Original purchase decision-maker |
| 6 | Product launch | Week 1-4 after | "New product = new GTM needs" | VP Marketing/Sales |
| 7 | Earnings/revenue | 1-3 weeks after | Revenue pressure or growth opportunity angle | Depends on situation |
| 8 | Conference attendance | 1-2 weeks before or 1 week after | "Ahead of {{event}}..." or "Hope {{event}} was great" | Attendee/speaker |
| 9 | Content publication | Within 48 hours | "Your post about {{topic}} resonated" | The author |
| 10 | Award/recognition | Within 1-2 weeks | "Congrats on {{award}}" | Company leadership |
| 11 | M&A activity | 30-90 days post | "Post-acquisition teams need to consolidate and scale" | Integration leads |
| 12 | Regulatory change | 2-8 weeks after | "With {{change}}, most teams rethink {{category}}" | Compliance/ops leads |

### Trigger Priority (when multiple fire)

1. Competitor churn (active buyer, proven budget)
2. Hiring for target role (budget allocated)
3. First-party signals (direct engagement)
4. Leadership change < 60 days
5. Funding < 30 days
6. Tech stack change
7. Content/social signals
8. Events/conferences

---

## Lead Priority Score

```
Priority = (Fit x 0.30) + (Intent x 0.45) + (Engagement x 0.25)
```

Intent weighted highest because a moderately-fit company with strong intent beats a perfect-fit company with no intent.

| Score | Tier | Action | SLA |
|-------|------|--------|-----|
| 80-100 | P1 Hot | Multi-channel outreach | Within 24 hours |
| 60-79 | P2 Warm | Priority email sequence | Within 3 days |
| 40-59 | P3 Active | Standard campaign | Within 1 week |
| 20-39 | P4 Passive | Low-priority/nurture | Monthly |
| 0-19 | P5 Cold | Do not contact | Quarterly review |

### Campaign-Type Adjustments

| Type | Fit | Intent | Engagement |
|------|-----|--------|------------|
| ABM (1-50 accounts) | 0.40 | 0.40 | 0.20 |
| Trigger-based | 0.20 | 0.60 | 0.20 |
| Re-engagement | 0.20 | 0.30 | 0.50 |
| Scaled outbound | 0.35 | 0.40 | 0.25 |

---

## Persona Blueprint (Quick Reference)

For each target persona, capture:

1. **Identity:** Title patterns, seniority, department, reports to, team size
2. **Pain Hierarchy (ranked):** P1 (critical, full budget authority) through P5 (low priority). Lead outreach with P1-P2.
3. **Motivation Map:** Career goals, team goals, personal drivers. People buy for personal reasons and justify with business reasons.
4. **Language Profile:** Their words for the problem, the goal, the frustration. Use THEIR language, not yours.
5. **Buying Behavior:** Research style, decision speed, risk tolerance, champion vs buyer, preferred channels
6. **Anti-Persona Signals:** Title includes "Interim," company in hiring freeze, just implemented a competitor, too senior/junior for your product

### Multi-Persona Account Strategy

```
Economic Buyer (signs check) — lead with ROI
Champion (sells internally) — lead with ease and efficiency
Influencer (shapes evaluation) — lead with day-to-day pain relief
Blocker (can kill deal) — lead with compliance and risk reduction
```

Start with the Champion. Get them to introduce you to the Economic Buyer. Address the Blocker proactively.
