---
name: referral-program
version: 1.0.0
description: "When the user wants to create, optimize, or analyze a referral program, affiliate program, or word-of-mouth strategy. Also use when the user mentions 'referral,' 'affiliate,' 'ambassador,' 'word of mouth,' 'viral loop,' 'refer a friend,' or 'partner program.' This skill covers program design, incentive structure, and growth optimization."
---

# Referral & Affiliate Programs

You are an expert in viral growth and referral marketing. Your goal is to help design and optimize programs that turn customers into growth engines.

## Before Starting

**Check for product marketing context first:**
If `.claude/product-marketing-context.md` exists, read it before asking questions. Use that context and only ask for information not already covered or specific to this task.

Gather this context (ask if not provided):

### 1. Program Type
- Customer referral program, affiliate program, or both?
- B2B or B2C?
- What's the average customer LTV?
- What's your current CAC from other channels?

### 2. Current State
- Existing referral/affiliate program?
- Current referral rate (% who refer)?
- What incentives have you tried?

### 3. Product Fit
- Is your product shareable?
- Does it have network effects?
- Do customers naturally talk about it?

### 4. Resources
- Tools/platforms you use or consider?
- Budget for referral incentives?

---

## Referral vs. Affiliate

### Customer Referral Programs

**Best for:**
- Existing customers recommending to their network
- Products with natural word-of-mouth
- Lower-ticket or self-serve products

**Characteristics:**
- Referrer is an existing customer
- One-time or limited rewards
- Higher trust, lower volume

### Affiliate Programs

**Best for:**
- Reaching audiences you don't have access to
- Content creators, influencers, bloggers
- Higher-ticket products that justify commissions

**Characteristics:**
- Affiliates may not be customers
- Ongoing commission relationship
- Higher volume, variable trust

---

## Referral Program Design

### The Referral Loop

```
Trigger Moment → Share Action → Convert Referred → Reward → (Loop)
```

### Step 1: Identify Trigger Moments

**High-intent moments:**
- Right after first "aha" moment
- After achieving a milestone
- After exceptional support
- After renewing or upgrading

### Step 2: Design Share Mechanism

**Ranked by effectiveness:**
1. In-product sharing (highest conversion)
2. Personalized link
3. Email invitation
4. Social sharing
5. Referral code (works offline)

### Step 3: Choose Incentive Structure

**Single-sided rewards** (referrer only): Simpler, works for high-value products

**Double-sided rewards** (both parties): Higher conversion, win-win framing

**Tiered rewards**: Gamifies referral process, increases engagement

**For examples and incentive sizing**: See [references/program-examples.md](references/program-examples.md)

---

## Program Optimization

### Improving Referral Rate

**If few customers are referring:**
- Ask at better moments
- Simplify sharing process
- Test different incentive types
- Make referral prominent in product

**If referrals aren't converting:**
- Improve landing experience for referred users
- Strengthen incentive for new users
- Ensure referrer's endorsement is visible

### A/B Tests to Run

**Incentive tests:** Amount, type, single vs. double-sided, timing

**Messaging tests:** Program description, CTA copy, landing page copy

**Placement tests:** Where and when the referral prompt appears

### Common Problems & Fixes

| Problem | Fix |
|---------|-----|
| Low awareness | Add prominent in-app prompts |
| Low share rate | Simplify to one click |
| Low conversion | Optimize referred user experience |
| Fraud/abuse | Add verification, limits |
| One-time referrers | Add tiered/gamified rewards |

---

## Measuring Success

### Key Metrics

**Program health:**
- Active referrers (referred someone in last 30 days)
- Referral conversion rate
- Rewards earned/paid

**Business impact:**
- % of new customers from referrals
- CAC via referral vs. other channels
- LTV of referred customers
- Referral program ROI

### Typical Findings

- Referred customers have 16-25% higher LTV
- Referred customers have 18-37% lower churn
- Referred customers refer others at 2-3x rate

---

## Launch Checklist

### Before Launch
- [ ] Define program goals and success metrics
- [ ] Design incentive structure
- [ ] Build or configure referral tool
- [ ] Create referral landing page
- [ ] Set up tracking and attribution
- [ ] Define fraud prevention rules
- [ ] Create terms and conditions
- [ ] Test complete referral flow

### Launch
- [ ] Announce to existing customers
- [ ] Add in-app referral prompts
- [ ] Update website with program details
- [ ] Brief support team

### Post-Launch (First 30 Days)
- [ ] Review conversion funnel
- [ ] Identify top referrers
- [ ] Gather feedback
- [ ] Fix friction points
- [ ] Send reminder emails to non-referrers

---

## Email Sequences

### Referral Program Launch

```
Subject: You can now earn [reward] for sharing [Product]

We just launched our referral program!

Share [Product] with friends and earn [reward] for each signup.
They get [their reward] too.

[Unique referral link]

1. Share your link
2. Friend signs up
3. You both get [reward]
```

### Referral Nurture Sequence

- Day 7: Remind about referral program
- Day 30: "Know anyone who'd benefit?"
- Day 60: Success story + referral prompt
- After milestone: "You achieved [X]—know others who'd want this?"

---

## Affiliate Programs

**For detailed affiliate program design, commission structures, recruitment, and tools**: See [references/affiliate-programs.md](references/affiliate-programs.md)

---

## Task-Specific Questions

1. What type of program (referral, affiliate, or both)?
2. What's your customer LTV and current CAC?
3. Existing program or starting from scratch?
4. What tools/platforms are you considering?
5. What's your budget for rewards/commissions?
6. Is your product naturally shareable?

---

## Tool Integrations

For implementation, see the [tools registry](../../tools/REGISTRY.md). Key tools for referral programs:

| Tool | Best For | Guide |
|------|----------|-------|
| **Rewardful** | Stripe-native affiliate programs | [rewardful.md](../../tools/integrations/rewardful.md) |
| **Tolt** | SaaS affiliate programs | [tolt.md](../../tools/integrations/tolt.md) |
| **Mention Me** | Enterprise referral programs | [mention-me.md](../../tools/integrations/mention-me.md) |
| **Dub.co** | Link tracking and attribution | [dub-co.md](../../tools/integrations/dub-co.md) |
| **Stripe** | Payment processing (for commission tracking) | [stripe.md](../../tools/integrations/stripe.md) |

---

## GSG APPLICATION (Confirmed Intel — HIGH Confidence)

### Why Referrals Are GSG's #1 Growth Lever

| Metric | Confirmed Data | Source |
|--------|---------------|--------|
| Referral conversion rate | 42% | CRM cohort data |
| Cold lead conversion rate | 4% | CRM cohort data |
| Referral vs cold lead advantage | 10.5x | Cohort analysis |
| Budget shift: ₹5L paid → referral | +12 enrollments | Altair modelling |
| Current referral share | 62% | COMMAND CENTER |
| Target referral share | 80% | GSG strategy |
| Gap to close | 18 percentage points | Active priority |

**Implication:** Every ₹1 shifted from Meta spend to referral activation = 10.5x more enrollment per rupee.

### Highest-Value Referral Trigger (Confirmed)
**Summer camp enrollment → referral ask = best timing**
- Summer camp attendees have 2.1x higher retention
- These parents are the most satisfied → highest referral propensity
- **Action:** Always fire referral ask within 2 weeks of summer camp completion

### GSG Referral Channels (Ranked by Effectiveness)
1. **WhatsApp (primary)** — confirmed as #1 parent communication channel
   - Individual forward > broadcast (feels personal)
   - Peer validation in existing school WhatsApp groups
2. **Personal conversation at campus events** — high trust, in-person ask
3. **Email with personalized link** — trackable, lower conversion
4. **School portal prompt** — visible but lower engagement rate

### GSG Referral Incentive Structure (Recommended)
**Double-sided, tiered:**
- Referrer: Fee credit (₹15-25K) per successful enrollment
- New enrollee: Registration waiver (₹5-10K) — removes friction
- Tier 2: Referrer gets bonus if referred family also refers (compound loop)

**Trigger timing:**
- Ask #1: After first parent-teacher conference (30 days post-enrollment)
- Ask #2: Summer camp completion (highest satisfaction moment)
- Ask #3: Annual renewal confirmation (retention signal = referral ready)

### Active Gap (Feb 2026)
Alumni engagement down 20% — alumni network is high-value referral source.
**Alert:** If alumni engagement continues declining, referral share will fall below 62%.
Priority action: Quarterly alumni touchpoint program to re-activate this cohort.

### Program Health Dashboard (Targets)
| Metric | Current | Target | Status |
|--------|---------|--------|--------|
| Referral share | 62% | 80% | BEHIND |
| Alumni engagement | -20% YoY | +10% | AT RISK |
| WhatsApp referral rate | Unknown | 15% of parents | MEASURE |
| Summer camp referral conversion | Unknown | 30% ask → enroll | MEASURE |

---

## Employee Spawn Triggers

**Pattern 5 — Spawn QUANT when:** Referral program budget decision OR channel ROI comparison (referral vs paid)
- Employee skills: statistical-reasoning, quantitative-analytical-toolkit
- Job: Model ROI of proposed referral budget shift with confidence intervals
- Output: ROI MODEL — ₹ impact, confidence range, break-even point

---

## Related Skills

- **launch-strategy**: For launching referral program effectively
- **email-sequence**: For referral nurture campaigns
- **marketing-psychology**: For understanding referral motivation
- **analytics-tracking**: For tracking referral attribution
