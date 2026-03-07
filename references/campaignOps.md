# Campaign Operations Reference

Instantly configuration, deliverability management, performance analysis, and optimization. This is the reference for Phases 5-6 of the Cold Email Engine.

---

## Instantly Configuration

### Inbox Strategy

- Ask the user how many inboxes they have and what providers (Microsoft, Google, etc.)
- **Microsoft inboxes** — conservative limit ~50 sends/day each
- **Google inboxes** — conservative limit ~30 sends/day each
- **Calculate theoretical daily capacity** based on inbox count and provider limits
- **Practical recommendation:** Start at 30-40% capacity, scale based on deliverability

### Sending Distribution

Rotate across all active inboxes. Don't send from the same inbox to the same domain twice. Instantly handles rotation automatically, but verify:

- All inboxes are connected and active
- Warmup is running on unused inboxes
- Each inbox has proper SPF, DKIM, DMARC configured
- Custom tracking domain set up (if using any tracking)

### Campaign Settings

| Setting | Value | Reason |
|---------|-------|--------|
| Stop on reply | ON | Never email someone who responded |
| Stop on auto-reply | OFF | OOO shouldn't kill the sequence |
| Text only | ON | HTML triggers spam filters |
| Link tracking | OFF | Tracking links destroy deliverability |
| Open tracking | OFF | Tracking pixels = spam flags |
| Email gap | 10-20 min | Avoids burst send patterns |
| Random delay | 5-10 min | Makes sending look human |
| Match lead ESP | ON | Gmail to Gmail, Outlook to Outlook |
| Send days | Tue-Thu | Mon = overload, Fri = checked out |
| Send window | 8-11 AM prospect TZ | Morning email triage window |

### Sending Limits by Provider

| Provider | Safe Daily Limit per Inbox | With Good Reputation |
|----------|---------------------------|---------------------|
| Microsoft 365 | 30-50 | Up to 80 |
| Google Workspace | 20-30 | Up to 50 |

These are for cold email specifically. Transactional limits are higher but irrelevant here.

---

## Deliverability Management

### Pre-Launch Requirements

1. **Domain setup:** SPF, DKIM, DMARC properly configured on ALL sending domains
2. **Inbox warmup:** Minimum 2 weeks of warmup at 30+ emails/day before cold sending
3. **List verification:** Every email verified through ZeroBounce. Target < 3% bounce.
4. **Content check:** No spam trigger words, no HTML, no tracking pixels/links

### Monitoring (Ongoing)

Check these daily for the first week, then weekly:

| Metric | Healthy | Warning | Critical |
|--------|---------|---------|----------|
| Bounce rate | < 2% | 2-5% | > 5% → PAUSE |
| Spam complaints | 0 | Any | > 0.1% → STOP |
| Open rate | 60%+ | 40-60% | < 40% → investigate |
| Unsubscribe | < 0.3% | 0.3-1% | > 1% → review targeting |

### Deliverability Triage

```
Bounce rate > 5%?
├── YES → Pause campaign. Re-verify list. Remove bounced emails. Resume.
└── NO ↓

Spam complaints > 0?
├── YES → Check content for spam triggers. Review targeting.
│         Reduce daily volume. Check domain reputation.
└── NO ↓

Open rate < 40%?
├── YES → Check domain reputation (Google Postmaster, MXToolbox)
│         Reduce daily sends. Rotate sending domains.
│         Review subject lines for spam triggers.
└── NO → Deliverability healthy. Focus on copy optimization.
```

### Domain Rotation Strategy

With multiple sending domains:
- Don't send more than 50 emails/day from any single domain
- If a domain's reputation drops, pull it from cold sending and put on warmup-only for 2-4 weeks
- Keep backup domains warm and ready to rotate in
- Never use your primary business domain for cold outreach

### When Inboxes Decline

Don't troubleshoot. Don't run spam tests. Just swap inboxes, change copy, and pull a new list. Troubleshooting adds time when you'll end up swapping anyway. Any 550 bounce error means the receiving server thinks you're a spammer — drastically change copy and swap inboxes immediately.

---

## Pre-Launch Checklist

### List Quality
- [ ] All emails verified via ZeroBounce
- [ ] Catch-all emails flagged or removed
- [ ] Duplicates removed (across all active campaigns)
- [ ] Estimated bounce rate < 3%
- [ ] No personal emails unless targeting solopreneurs

### Email Accounts
- [ ] All inboxes connected to Instantly
- [ ] Warmup running for 2+ weeks on new inboxes
- [ ] SPF/DKIM/DMARC verified on all sending domains
- [ ] No inbox has deliverability warnings

### Campaign Content
- [ ] Sequence written (Steps 1-4 minimum)
- [ ] 2-3 variants per step
- [ ] All {{variables}} resolve correctly for every lead
- [ ] Text-only (no HTML)
- [ ] No spam trigger words
- [ ] No tracking links or pixels
- [ ] Mobile preview looks clean
- [ ] Under 150 words per email
- [ ] CTA is a question, not a demand

### Settings
- [ ] Tracking OFF (links and opens)
- [ ] Stop on reply ON
- [ ] Stop on auto-reply OFF
- [ ] Daily limits set per inbox
- [ ] Sending window matches prospect timezone
- [ ] Test email sent to yourself

---

## Performance Analysis

### The 6-Layer Metrics Stack

```
Layer 1: Deliverability → bounce < 3%, spam = 0, inbox placement > 95%
Layer 2: Engagement     → open rate 60-80%
Layer 3: Response       → total reply rate 8-15%
Layer 4: Quality        → positive reply rate 2-8%
Layer 5: Conversion     → reply-to-meeting rate 40-60%, show rate 80-90%
Layer 6: Revenue        → pipeline generated, revenue closed, cost per meeting
```

**Layer 4 (positive reply rate) is the single most important metric.** Everything else is diagnostic.

### Real-World Benchmarks

Industry average: **1 positive response per 350 contacts.**

| Tier | Leads per Positive | What It Looks Like |
|------|-------------------|-------------------|
| Elite | Under 50 | Free/low-barrier offers, hyper-targeted lists, niche pain |
| Good | 50 to 200 | Strong offer, good targeting, decent personalization |
| Average | 200 to 500 | Standard B2B outbound with reasonable targeting |
| Struggling | 500+ | Commoditized offer, broad targeting, or channel mismatch |

Home services to property managers: ~36 leads per positive (elite). SEO agencies: ~588 (struggling). The difference isn't copy — it's list quality and offer specificity.

**TAM recycling:** Re-contact the same list every 3 months minimum (2 months absolute floor). Nobody remembers a cold email they didn't respond to. If you need 10 meetings/month, plan to email ~2,500 leads/month.

**1% reply rate signal:** If you're getting around 1% total replies (excluding OOO), you're landing in the primary inbox. Even negative replies confirm deliverability is working.

### Reply Classification

| Category | Definition | Action |
|----------|-----------|--------|
| Positive: Meeting | Agreed to a call | Book within 1 hour |
| Positive: Interest | Interested, no commitment | Reply with value, ask for meeting |
| Positive: Referral | Directed to someone else | Contact the referral, thank original |
| Neutral: Not Now | Timing issue | Nurture, follow up in 30-60 days |
| Neutral: OOO | Auto-reply | Re-send after they return |
| Negative: Not Interested | Polite rejection | Remove from sequence |
| Negative: Angry | Hostile | Remove immediately, review targeting |
| Negative: Unsubscribe | Requests removal | Remove immediately (legal requirement) |

### Diagnosis Decision Tree

**Campaign Level:**
```
Positive reply rate > 5%?  → SCALE: increase volume, clone for new segments
Positive reply rate 2-5%?  → OPTIMIZE: test variants, improve personalization
Positive reply rate 1-2%?  → DIAGNOSE: fix one variable (copy, targeting, or timing)
Positive reply rate < 1%?  → KILL: stop. redesign from scratch or abandon the angle
```

**Symptom-Level:**
```
High opens, low replies?
→ Copy problem. Body/CTA not compelling. Test new variants.

Low opens?
→ Subject line or deliverability. Fix this FIRST.

High replies but low meetings?
→ Reply handling. Respond faster. Qualify better.

High total replies but low positive replies?
→ Targeting problem. Wrong people. Review ICP and list.
```

### Step Performance Analysis

Track per step:
| Step | Sends | Opens | Open Rate | Replies | Reply Rate | Positive | Positive Rate |
|------|-------|-------|-----------|---------|------------|----------|---------------|
| 1 | __ | __ | __% | __ | __% | __ | __% |
| 2 | __ | __ | __% | __ | __% | __ | __% |
| 3 | __ | __ | __% | __ | __% | __ | __% |
| 4 | __ | __ | __% | __ | __% | __ | __% |

If Step 1 reply rate < 3%, the opening email needs work.
If replies drop > 20% per step, sequence fatigue.

### Variant Comparison

| Metric | Variant A | Variant B | Variant C | Winner |
|--------|-----------|-----------|-----------|--------|
| Sends | __ | __ | __ | — |
| Open rate | __% | __% | __% | __ |
| Positive reply rate | __% | __% | __% | __ |
| Meetings | __ | __ | __ | __ |

Minimum 200 sends per variant. Winner = highest positive reply rate.

---

## Campaign Report Template

```
CAMPAIGN: {{name}}
Period: {{start}} to {{end}}
Total sends: {{sends}}

DELIVERABILITY
  Bounce rate: __% (target: <3%)
  Spam complaints: __ (target: 0)

ENGAGEMENT
  Open rate: __% (target: 60-80%)

RESPONSE
  Total reply rate: __% (target: 8-15%)
  Positive reply rate: __% (target: 2-8%)

CONVERSION
  Meetings booked: __
  Reply-to-meeting: __%
  Cost per meeting: $__

REVENUE
  Pipeline: $__
  Closed: $__

VERDICT: [Scale / Optimize / Diagnose / Kill]
NEXT ACTION: [specific action]
```

---

## Escalation Logic

```
Standard Sequence (14 days, 4 steps)
    ↓ No reply
Wait 30 days
    ↓
Re-engagement Sequence (different angle, 3 steps)
    ↓ No reply
Wait 60 days
    ↓
Trigger-based Re-entry ONLY (new signal required)
```

**Never:**
- Re-enroll in the same sequence
- Contact someone who said "not interested"
- Send more than 2 sequences to the same person in 6 months without a new signal

---

## Copy Refresh Cadence

| Component | Refresh Every |
|-----------|-------------|
| Subject lines | 4-6 weeks |
| Body copy | 8-12 weeks |
| Full sequence | Quarterly |
| If reply rate drops 30%+ from peak | Immediately |

---

## Campaign Naming Convention

```
{Segment} - {Angle} - {Version}
```

Examples:
- `HomeSvc Owners CA - Referral Ceiling - v1`
- `SaaS VP Sales - Hiring Signal - v2`
- `Agency Founders - Creative Ideas - v1`
