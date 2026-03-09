---
name: gtm
description: Create a go-to-market plan with positioning, messaging, channels, and launch checklist. Use when user says "go-to-market plan", "GTM strategy", "launch plan", "how do we launch this", "marketing plan", "launch checklist", or needs to plan how a product or feature reaches users.
user-invokable: true
args:
  - name: target
    description: The product or feature to create a GTM plan for (optional)
    required: false
metadata:
  author: builder-skills
  version: "1.0.0"
---

Build a go-to-market plan that gets a product or feature from "built" to "adopted." Work through each step in order. Skip nothing.

## Step 1: Understand What's Launching

Gather these before doing anything else:

| Question | Why it matters |
|----------|---------------|
| What exactly is shipping? | Scope the plan — a whole product vs. a settings toggle need different launches |
| Who is the target audience? | Channels, messaging, and tier all depend on this |
| What's the timeline? | Determines what's realistic |
| What exists already? | Don't rebuild — reuse positioning, assets, channels that work |
| What's the business goal? | Growth, retention, expansion, awareness — each changes the plan |

If the user hasn't provided this context, ask. Don't guess.

## Step 2: Positioning

Answer three questions clearly:

1. **Who is this for?** Be specific. "Small business owners who manage their own payroll" not "SMBs."
2. **What job does it serve?** Use the JTBD frame — what progress is the user trying to make? (Reference the jtbd skill if available.)
3. **How is it different?** State the alternative the user has today and why this is better. If you can't articulate the difference, the positioning isn't ready.

Write it as a positioning statement:

```
For [target audience] who [situation/need],
[product/feature] is a [category]
that [key benefit].
Unlike [alternative], it [differentiator].
```

## Step 3: Messaging

Build a messaging hierarchy:

| Layer | What it is | Example |
|-------|-----------|---------|
| **Headline** | The one sentence a stranger reads | "Ship invoices in 30 seconds, not 30 minutes" |
| **Value proposition** | The promise — what changes for the user | "Automated invoicing that pulls line items from your project tracker" |
| **Proof points** (3 max) | Evidence the promise is real | "Cuts invoice creation time by 90%" / "Syncs with Jira, Linear, Asana" / "Used by 2,000 teams in beta" |

**Messaging template:**

```
HEADLINE:
[One clear sentence. Lead with the outcome, not the feature.]

VALUE PROP:
[What the user gets. Focus on the job, not the technology.]

PROOF POINTS:
1. [Quantified result or social proof]
2. [Capability that matters most]
3. [Trust signal — security, reliability, adoption]

OBJECTION HANDLING:
- "Why should I switch?" → [Answer]
- "Is it reliable?" → [Answer]
- "What does it cost me to try?" → [Answer]
```

## Step 4: Channels

List where the target audience already spends time. Rank each channel:

| Channel | Reach | Fit | Effort | Use it? |
|---------|-------|-----|--------|---------|
| In-app announcement | High (existing users) | High | Low | Yes — always |
| Email to existing users | High | High | Low | Yes — always |
| Blog post | Medium | Medium | Medium | Tier 1-2 |
| Social (specify platform) | Varies | Varies | Low | Match to audience |
| Product Hunt / HN | High for dev tools | Niche | Medium | Only if audience fits |
| Paid ads | High | Varies | High | Tier 1 only, with budget |
| Partner co-marketing | Medium | High | High | When there's a natural partner |
| Press / analyst briefing | High | Low-Medium | High | Tier 1 only, major news |
| Community / forum posts | Low-Medium | High | Low | When community exists |
| Sales enablement | N/A (B2B) | High | Medium | B2B Tier 1-2 |

Pick 3-5 channels. More than that dilutes effort.

## Step 5: Launch Checklist

### Pre-launch (2-4 weeks before)

- [ ] Positioning and messaging finalized
- [ ] Landing page or feature page live (or drafted)
- [ ] Internal team briefed — support, sales, CS all know what's coming
- [ ] Beta feedback incorporated
- [ ] Analytics and tracking instrumented (reference the metrics skill if available)
- [ ] Launch email drafted
- [ ] Social posts drafted
- [ ] Blog post drafted (Tier 1-2)
- [ ] Documentation / help articles updated
- [ ] Rollback plan exists if something breaks

### Launch day

- [ ] Feature flag flipped / deploy confirmed
- [ ] Announcements sent (email, in-app, social)
- [ ] Blog post published
- [ ] Team monitoring dashboards and support queue
- [ ] Respond to early feedback within hours, not days

### Post-launch (1-4 weeks after)

- [ ] Track success metrics daily for first week
- [ ] Gather qualitative feedback (support tickets, user interviews, social mentions)
- [ ] Ship fast-follow fixes for issues surfaced at launch
- [ ] Internal retro on what went well / what didn't (reference the retro skill if available)
- [ ] Report results to stakeholders
- [ ] Decide: double down, iterate, or move on

## Step 6: Success Metrics

Define before launch, not after:

| Metric | Why | Target |
|--------|-----|--------|
| **Adoption** — % of eligible users who try it | Did the launch reach people? | Set based on tier |
| **Activation** — % who complete the core action | Did they get value? | Higher bar than adoption |
| **Retention** — % still using after 7/30 days | Is it sticky? | Baseline from similar features |
| **Sentiment** — NPS, support tickets, social | How do people feel about it? | Qualitative + quantitative |

Pick one primary metric. Track the rest as supporting signals.

## Launch Tier Framework

Classify every launch before planning it. The tier determines scope of effort.

| | Tier 1 — Major | Tier 2 — Medium | Tier 3 — Minor |
|---|---|---|---|
| **What** | New product, major platform change, new market entry | Significant feature, new integration, pricing change | Bug fix, small improvement, setting change |
| **Audience** | Everyone + prospects | Relevant segment | Active users of that area |
| **Channels** | All: email, blog, social, press, in-app, sales | Email, blog, in-app, targeted social | Changelog, in-app tooltip, maybe email |
| **Timeline** | 4-8 weeks prep | 2-4 weeks prep | Ship and announce same day |
| **Assets** | Landing page, demo video, blog, email sequence, sales deck | Blog post, email, updated docs | Changelog entry, tooltip |
| **Team** | Cross-functional: product, marketing, sales, CS, eng | Product + marketing | Product + eng |

**Default to a lower tier.** Most launches are Tier 2 or 3. Overcommunicating minor changes trains users to ignore you.

## Output

Deliver the GTM plan as a single document with these sections:

1. **Overview** — what's launching, when, what tier
2. **Positioning statement**
3. **Messaging** — headline, value prop, proof points
4. **Channels** — ranked list with rationale
5. **Checklist** — pre-launch, launch day, post-launch (customized, not generic)
6. **Success metrics** — primary + supporting, with targets
