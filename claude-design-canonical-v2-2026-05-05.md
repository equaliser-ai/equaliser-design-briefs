# Equaliser AI — Canonical Design Brief v2

**Date:** 2026-05-05 (supersedes every prior `claude-design-*` and every `pkg-c1` through `pkg-c6` package)
**For:** Claude Design — to brief Claude Code on UX/UI best practice, then collaborate page-by-page on the redesign
**Status:** This is the only brief to use. Every prior brief is historical reference.

---

## 0 · What went wrong before, what changes now

You've drifted three times — C2 used stale nav vocabulary, C3 invented agent personas, C6 forced templates onto pages whose data shape didn't fit. Root cause: you were generating speculative pixel-perfect mockups against assumptions about content, then we'd discover the content didn't fit the visual when implementation hit real data.

**The new mode is reversed.** You don't draw pixel-perfect pages first. You **brief Claude Code (me) on best-practice UX/UI principles for paid-media intelligence platforms**. I implement the principles against the real data shapes. Per-page redesigns happen one at a time, each on a preview deploy, with James signing off before merge. No batch redraws.

**Three-way roles:**
- **You (Claude Design):** principles, pattern library, per-page design briefs (one at a time, after I've shown you the data shape). Voice + visual taste partner.
- **Me (Claude Code):** implementation, data integrity, technical reality. I'll show you what data each page actually has.
- **James:** product decisions, sign-off per page, voice + brand owner.

---

## 1 · Reading rules (please follow strictly)

**Read these only:**
1. This brief
2. `docs/design-system.md` — current tokens (your starting reference, **not** a lock — you're free to redesign)
3. The actual built code in `app/accounts/[client]/`, `components/`, `lib/agent-tone.ts` to see the real content + agent attribution

**Do NOT read these — all stale proposals or wrong vocab:**
- `docs/briefs/nav-v2.md` (stale; SUPERSEDED at the top of the file)
- `docs/agent_personas.md` (stale; the persona overlay never landed)
- `docs/briefs/consolidated-master-brief-v1.2.md`
- `docs/briefs/2026-04-18-*` (pre-architectural lock)
- `docs/briefs/claude-design-update-2026-05-04.md` (SUPERSEDED)
- `docs/briefs/claude-design-round2-attachments-2026-05-04.md` (SUPERSEDED)
- `docs/briefs/claude-design-canonical-2026-05-04.md` (SUPERSEDED by THIS file — v2)

If you find a doc that contradicts this brief, **this brief wins**. Flag the contradiction back; don't reconcile.

---

## 2 · What's fixed vs what's open

### Fixed (the platform reality you're designing FOR)

- **Tier-1 nav:** `Dashboard / Analyse / Optimise / Equalise / Deliver + Settings`. Verb-led. Locked 2 May 2026.
- **17 typed agents** seeded in `supabase/migrations/20260505000006_seed_agent_registry.sql`: Sentinel, Investment Director, Bid Strategist, Creative Director, Fatigue Referee, Auction Strategist, PMax Intelligence, Product Director, Audience Optimiser, Narrator, Forecaster, Trend Spotter, Competitive Intel, Structure Auditor, Budget Pilot, Budget Optimiser, Pacing Agent. **No persona overlay** — they're called by their role title in the UI.
- **Typed ontology entities** — Account, Campaign, AdGroup, Ad, Audience, Creative, Keyword, Product, LandingPage, Anomaly, Analysis, Finding, Recommendation, Action, Memory, Question. Real Postgres tables.
- **Voice rules:** first person, plain English, action-oriented, en-GB. Numbers in `tabular-nums slashed-zero`, ROAS without the `×`, money rounds to £k thousands. See `lib/agent-tone.ts` `softenAgentTone()`.
- **Brand name:** "Equaliser AI". Wordmark + AI-suffix treatment is the only legitimate brand exploration.

### Open (yours to redesign)

- Colour palette (current cream + Spark Blue is a reference, not a lock — challenge it if a B2B-analytics audience reads better in cool near-white)
- Typography scale + weights
- Layout density, spacing, radius, motion
- Component primitives (cards, buttons, tiles, chips, charts)
- Iconography
- Logo + AI-suffix treatment
- Sub-page IA below Tier-1 verbs (you can group, drill, split — just keep the verb labels themselves)
- Visual form of universal components (`WhyButton` / `OutcomeChip` / `EntityPanel` — function fixed, form open)
- Mode toggle pattern (Agency vs Client — placement + visual)
- Whether agent attribution is a chip, avatar, badge, line of text — your call

---

## 3 · First deliverable from you — UX/UI principles brief for me

**Before any page redesign, produce a short doc** (~1500 words, max 2500) titled `Equaliser — UX/UI Principles v1` covering these eight topics. Each topic = principle + 2-3 anti-patterns + 1-2 reference platforms (GA4, Looker, Stripe, Linear, Notion, Monday — name which and why).

**Eight topics:**

1. **Information density for daily-use B2B analytics.** When does whitespace earn its place vs cost a screenful? When is a chart better than a table, and when is the inverse true? How dense should one card be?

2. **Decision-loop UX.** Every page should have a path: data → diagnosis → action → outcome. How do we visually signal where the user is in that loop? How do we close it?

3. **Agent attribution patterns.** A finding has one author + zero-or-more chained collaborators. How does this render? When is "Investment Director recommends…" enough; when do we need the chain visible?

4. **Mode switching (Agency vs Client).** Same data, two audiences. What changes between modes — agent count, copy register, density, granularity? Where does the toggle live? Three placement options + your recommendation.

5. **Empty / loading / no-data / error states per page.** A pilot client on day one has no data; the page can't be a wall of zeros. What's the right pattern?

6. **Mobile + accessibility floor.** Strategists check the platform on phones. WCAG AA non-negotiables. Which pages are mobile-first vs desktop-first?

7. **Trust + provenance treatment.** Every claim needs a working trail (data source, agent author, prompt version, model). How does this surface without overwhelming the page? What's the "Why" affordance?

8. **Quiet by default, loud when it matters.** Severity, urgency, action — visual hierarchy without garish. How does a "fix now" finding look different from a "watch" finding?

**Format:** plain markdown. No code, no JSX. Ship it as `docs/briefs/equaliser-ux-principles-v1.md` so we can both reference it.

This is your first deliverable. Once shipped, I read it, clarify anything I disagree with or need detail on, then we move to per-page work.

---

## 4 · Per-page content contract — every page in the product

This is the spec you design against. **Data points listed under each page are sacred.** Visual treatment can change; content survives. If your design proposal would lose a data point or quiet a load-bearing emphasis, flag it back — don't silently drop it.

**Format per page:**
- **Path** — actual route
- **Purpose** — one sentence
- **Core question** — what user opens this to answer
- **Data points (sacred)** — what must appear, irrespective of treatment
- **Agents speaking** — which named agents author content here
- **Emphasis** — the loudest thing on the page
- **States** — loading / empty / error / no-data

### Tier-1 verbs (5 pages)

#### Dashboard — `/accounts/[client]/dashboard`
- **Purpose:** the morning check-in. "Is this account OK?"
- **Core question:** anything need my attention before I start work?
- **Data points (sacred):** RAG state pill (green / amber / red), MTD pacing vs target, headline KPI vs target (ROAS or CPA depending on `is_lead_gen`), top 3 findings by £ impact, agent-authored verdict (one sentence), trend direction (vs prior period)
- **Agents speaking:** Sentinel (verdict), Pacing Agent (pacing line), Investment Director (top finding)
- **Emphasis:** RAG state + verdict — visible from across the room
- **States:** new account = "no data yet, agents start tomorrow morning"; partial data = render what's there + flag what's missing

#### Analyse — `/accounts/[client]/analyse`
- **Purpose:** account headline. Click any tile to drill into the data layer.
- **Core question:** what does the period look like vs prior?
- **Data points (sacred):** spend / revenue / ROAS / conversions for selected date range + WoW Δ% sub-line, channel split, daily roll-up chart, period-compare CTA
- **Agents speaking:** Sentinel (anomaly callouts), Trend Spotter (period verdict)
- **Emphasis:** the period verdict + the WoW % deltas

#### Optimise — `/accounts/[client]/optimise`
- **Purpose:** the action surface. Ranked recommendations to approve, snooze, or send to plan.
- **Core question:** what should I do this week, in priority order?
- **Data points (sacred):** severity verb (Fix / Investigate / Scale / Review), agent author, soft-toned headline, lever badge, £ forecast impact, evidence excerpt, inline action row (Approve / Send to plan / Snooze / Dismiss), confidence chip, related-finding link
- **Agents speaking:** Investment Director, Bid Strategist, Creative Director, Audience Optimiser, Budget Optimiser, Product Director
- **Emphasis:** £ forecast impact (top right of each card, large) + the verb chip
- **States:** zero findings = "Queue clear — agents found nothing material this period"

#### Equalise — `/accounts/[client]/equalise`
- **Purpose:** strategy hub. Audit, scenarios, forecasts, opportunities, competition.
- **Core question:** what's the bigger picture story I'm telling about this account?
- **Data points (sacred):** SCALE 5-pillar score visual, Bronze/Silver/Gold maturity tier, 3 strategic shortcuts (audit / scenarios / forecasts), recent strategist-tone narrative (Narrator agent)
- **Agents speaking:** Narrator, Structure Auditor, Forecaster, Investment Director (synthesis)
- **Emphasis:** SCALE pillar visual — this is the lead-hook for client decks

#### Deliver — `/accounts/[client]/deliver`
- **Purpose:** client outputs hub. What's going to the client this period.
- **Core question:** what's due, what's been sent, what's being prepared?
- **Data points (sacred):** verdict banner (in-flight count + most-recent), tile grid: Reports / Activity / Meetings / Strategy decks / Templates / Commentary
- **Agents speaking:** Narrator (commentary), Investment Director (deck framing)
- **Emphasis:** what's due this week — top of page

### Analyse children (10 pages)

#### Analyse → Channels — `/accounts/[client]/analyse/channels`
- **Purpose:** performance by ad platform.
- **Core question:** which channels are pulling weight; which are dragging?
- **Data points (sacred):** spend / revenue / cost / impressions / clicks / CVR / ROAS / CPA per channel, brand vs generic split toggle, click-through to /campaigns drill, period-compare WoW deltas, agent commentary chip per channel (Sentinel-authored interpretation)
- **Agents speaking:** Sentinel, Investment Director
- **Emphasis:** the strategist-tone one-liner per top channel — "Search ROAS holding 4.2 vs 3.8 last week, scale candidate"

#### Analyse → Campaigns — `/accounts/[client]/analyse/campaigns`
- **Purpose:** cross-channel campaign performance.
- **Core question:** which campaigns are winning, which are leaking?
- **Data points (sacred):** all campaign columns (cost, revenue, ROAS, CPA, CTR, CVR, conversions, impressions), channel filter chip, status pip, period-compare deltas, action verb per row (Scale / Reduce / Plan), inline link to campaign optimise
- **Agents speaking:** Investment Director, Bid Strategist
- **Emphasis:** ROAS-vs-target column with red/amber/green colouring

#### Analyse → Creatives — `/accounts/[client]/analyse/creatives`
- **Purpose:** which ads are working / fatiguing.
- **Core question:** what's tired, what's tearing it up?
- **Data points (sacred):** ad thumbnail, format/objective filters, CTR / CVR / spend / frequency, fatigue %fill chip, fatigue status (CRITICAL/HIGH/MODERATE/WATCH/HEALTHY), retire/scale verb per row
- **Agents speaking:** Creative Director, Fatigue Referee
- **Emphasis:** fatigue chip — instantly visible on the row

#### Analyse → Audiences — `/accounts/[client]/analyse/audiences`
- **Purpose:** audience segment performance.
- **Core question:** which segments to bid up / down on?
- **Data points (sacred):** segment_type tag (demographics / geo / hourly / devices / audience_type / geo_city / prospecting / meta_placement), CPA / CVR / volume per segment, bid-modifier inline buttons (-20% / -10% / +10% / +20%), Apply banner with batch count
- **Agents speaking:** Audience Optimiser
- **Emphasis:** the bid-modifier buttons — inline action without leaving page
- **NOTE:** segment types are NOT homogeneous; ratio-bar-per-row across mixed types is incoherent. Group or filter by segment_type first.

#### Analyse → Products — `/accounts/[client]/analyse/products`
- **Purpose:** SKU performance for ecom clients.
- **Core question:** what's a hero, what's dead?
- **Data points (sacred):** image + GTIN, revenue, units, ROAS, feed-health pip, tier (Hero / Supporting / Underperforming / Dead), tier-aware action verb (Scale / Maintain / Investigate / Pause)
- **Agents speaking:** Product Director, Feed Optimiser
- **Emphasis:** tier badge + action verb per row
- **NOTE:** lead-gen clients (HPG, Dig) don't have products — this page should redirect or empty-state appropriately

#### Analyse → Keywords — `/accounts/[client]/analyse/keywords`
- **Purpose:** search keyword performance.
- **Core question:** which keywords need attention?
- **Data points (sacred):** keyword text, match type pip (Exact / Phrase / Broad), brand/generic classification, spend / clicks / CVR / CPA, quality score chip, 7-day sparkline, "Add as negative" inline action for waste
- **Agents speaking:** Bid Strategist, Search Term Analyst (legacy agent_type)
- **Emphasis:** quality score colour-coded + match type pip

#### Analyse → Attribution — `/accounts/[client]/analyse/attribution`
- **Purpose:** engine vs GA4 reconciliation, halo, cannibalisation.
- **Core question:** what's the blended truth across measurement systems?
- **Data points (sacred):** verdict banner with engine-vs-GA4 ratio, weekly ratio chart (12-week window), per-channel breakdown table (engine / GA4 / blended truth / halo lift), methodology explainer
- **Agents speaking:** narrator-class agent (analytics framing)
- **Emphasis:** verdict banner — the strategist's interpretation
- **NOTE:** engine and GA4 are tracked deliberately; never merge them. Engine = bid strategy truth, GA4 = business truth.

#### Analyse → Trends — `/accounts/[client]/analyse/trends`
- **Purpose:** WoW / MoM / YoY anomaly detection.
- **Core question:** what changed this week and is it a problem?
- **Data points (sacred):** anomaly chips with strategist commentary ("Spend +45% week, revenue keeping pace, healthy expansion"), 3-most-recent anomaly headline cards, severity pip per anomaly, trend chart with anomaly markers
- **Agents speaking:** Trend Spotter, Sentinel
- **Emphasis:** anomaly chips with their commentary — the page is narrated, not mechanical

#### Analyse → Website — `/accounts/[client]/analyse/website`
- **Purpose:** landing page intent + funnel health.
- **Core question:** where are we losing customers?
- **Data points (sacred):** per-LP intent score (0-100 composite), drop-off step in funnel, hot/warm/cold tier, "Optimise →" verb on hot+low-CVR rows, 6-step funnel visual (impressions → clicks → sessions → addToCart → checkout → purchase), highlighted worst-drop step
- **Agents speaking:** Landing Page Auditor (legacy agent_type), Conversion Rate agent
- **Emphasis:** worst-drop callout — "biggest leak: 38% lost between Add to cart and Checkout"
- **NOTE:** lead-gen clients have no Add to cart → drop those rows from the funnel

### Optimise children (5 pages)

#### Optimise → Plan — `/accounts/[client]/optimise/plan`
- **Purpose:** kanban for findings lifecycle.
- **Core question:** what's queued, in flight, done?
- **Data points (sacred):** five columns (New / Reviewed / Assigned / Approved / Applied), per-card: verb / soft-toned title / owner avatar / deadline countdown / £ forecast / agent + lever chips / aged-7d red border / progress button / WhyButton + OutcomeChip on Applied
- **Agents speaking:** all
- **Emphasis:** Applied column with OutcomeChip = the proof-point for clients

#### Optimise → Budgets — `/accounts/[client]/optimise/budgets`
- **Purpose:** scenario sliders for budget reallocation.
- **Core question:** if I shift £X from A to B, what happens?
- **Data points (sacred):** per-channel slider (-50% to +100%), saturation curve (diminishing returns), agent recommendation overlay (green dotted marker), total reallocation tally, projected ROAS delta, Apply button → /optimise/plan queue
- **Agents speaking:** Budget Optimiser, Investment Director
- **Emphasis:** projected ROAS delta in the header — the answer to "should I do this"

#### Optimise → Creative — `/accounts/[client]/optimise/creative`
- **Purpose:** creative segmentation + retire/scale.
- **Core question:** what creative needs intervention now?
- **Data points (sacred):** segmentation tabs (All / Top CTR / Bottom CTR / Fatigued / Healthy), format/objective chips, retire + scale tables with per-row buttons, fatigue status colour-coded
- **Agents speaking:** Creative Director, Fatigue Referee
- **Emphasis:** retire table — biggest immediate win

#### Optimise → Automation — `/accounts/[client]/optimise/automation`
- **Purpose:** skill template gallery + run history.
- **Core question:** what scheduled work is running, and what could I add?
- **Data points (sacred):** 6 skill templates with strategist-tone descriptions + agent attribution + Add button, run history feed (status pip, skill name, trigger, duration, output summary), per-skill filter chips
- **Agents speaking:** all (depending on skill)
- **Emphasis:** "what's running right now" panel — operational visibility

#### Optimise → Testing — `/accounts/[client]/optimise/testing`
- **Purpose:** test backlog.
- **Core question:** what experiments are queued, in flight, complete?
- **Data points (sacred):** 6 test idea templates with hypothesis + agent + duration, test backlog table, learnings card (aggregate win rate, avg lift, top win highlight)
- **Agents speaking:** Investment Director, Creative Director, Audience Optimiser, Bid Strategist
- **Emphasis:** learnings card — proof the platform learns

### Equalise children (6 pages)

#### Equalise → Audit — `/accounts/[client]/equalise/audit`
- **Purpose:** SCALE 5-pillar maturity audit.
- **Core question:** how mature is this account, what moves take it from current → aspirational?
- **Data points (sacred):** SCALE pillar scores (Structure / Creative / Audience / Levers / Effectiveness, each 0-100), Bronze/Silver/Gold tier badge, 30/60/90-day roadmap drill per pillar with projected score lift, "Stack the moves above and SCALE goes from X → Y" aggregate banner, Queue → buttons
- **Agents speaking:** Structure Auditor, Investment Director (synthesis)
- **Emphasis:** SCALE pillar visual + tier badge — the client-deck hero

#### Equalise → Forecasts — `/accounts/[client]/equalise/forecasts`
- **Purpose:** multi-scenario projections.
- **Core question:** what does the next 8 weeks look like under different decisions?
- **Data points (sacred):** three projection lines (current pace / agent-recommended / aggressive growth) on one chart with confidence bands, three scenario cards below (spend / revenue / ROAS tallies + delta vs anchor)
- **Agents speaking:** Forecaster, Investment Director
- **Emphasis:** confidence bands — visible uncertainty

#### Equalise → Opportunities — `/accounts/[client]/equalise/opportunities`
- **Purpose:** AI-ranked growth opportunities.
- **Core question:** where's the next £ of growth coming from?
- **Data points (sacred):** opportunity headline + £ headroom + lever, action column with verb based on type (Scale / Test / Investigate), Review → button per row, agent attribution
- **Agents speaking:** Investment Director, Audience Optimiser, Product Director
- **Emphasis:** £ headroom column — sortable

#### Equalise → Scenarios — `/accounts/[client]/equalise/scenarios`
- **Purpose:** save-and-compare budget scenarios.
- **Core question:** which budget shape wins for next month?
- **Data points (sacred):** multi-select up to 4 scenarios for side-by-side panel, per-scenario: budget / forecast revenue / forecast ROAS / conversions + delta vs anchor + stacked channel-mix bar, Pin to plan button
- **Agents speaking:** Investment Director, Forecaster
- **Emphasis:** delta vs anchor — comparison is the value

#### Equalise → Competition — `/accounts/[client]/equalise/competition`
- **Purpose:** share-of-voice + competitive intel.
- **Core question:** are we gaining or losing share?
- **Data points (sacred):** SoV trend chart (your IS line vs top 3 competitors, weekly), three momentum cards (gaining / biggest gainer / biggest faller)
- **Agents speaking:** Competitive Intel
- **Emphasis:** the trend chart — momentum > snapshot

#### Equalise → Strategy — `/accounts/[client]/equalise` (root)
- See Tier-1 spec above.

### Deliver children (6 pages)

#### Deliver → Reports — `/accounts/[client]/deliver/reports`
- **Purpose:** report editor + history.
- **Data points (sacred):** filter chips (due this week / awaiting send / sent / archived), per-row: title / kind / week_start / status / updated_at, edit + send actions
- **Agents speaking:** Narrator
- **Emphasis:** "due this week" filter prominent

#### Deliver → Activity — `/accounts/[client]/deliver/activity`
- **Purpose:** chronological log of agent + human activity.
- **Data points (sacred):** per-row: kind / timestamp / agent (if agent-authored) / summary / severity, filter by agent / kind / date
- **Emphasis:** agent attribution per row

#### Deliver → Meetings — `/accounts/[client]/deliver/meetings`
- **Purpose:** meeting prep.
- **Data points (sacred):** per-meeting: anticipated issues list with Review → button, talking points, recent decisions
- **Agents speaking:** Investment Director (issue framing)
- **Emphasis:** anticipated issues — the prep value-add

#### Deliver → Strategy decks — `/accounts/[client]/deliver/strategy-decks`
- **Purpose:** generate / browse client-facing decks.
- **Data points (sacred):** deck templates, recent decks, generate button, share link

#### Deliver → Templates — `/accounts/[client]/deliver/templates`
- **Purpose:** report + deck templates.
- **Data points (sacred):** template list, edit + duplicate actions

#### Deliver → Commentary — `/accounts/[client]/deliver/commentary`
- **Purpose:** strategist-tone narrative drafting.
- **Data points (sacred):** per-period commentary, agent draft + human edit history, save-final action
- **Agents speaking:** Narrator
- **Emphasis:** edit history — provenance for the client

---

## 5 · Anti-patterns (already learned)

- ❌ **Force-fitting templates onto pages** when their data shape doesn't fit (C6's mistake on Audiences)
- ❌ **Inventing agents** not in the registry (C2's "Scout/Pace/Strategist" persona overlay)
- ❌ **Using stale nav vocab** (C2/C3 used Performance/Intelligence — production is Analyse/Optimise/Equalise/Deliver)
- ❌ **Cluttered cards** (C3 had 12 chips per Finding card — kills the design)
- ❌ **Time-of-day framing** ("Monday morning brief", "Today's check-in") — banned per `feedback_no_time_framing`
- ❌ **Bubble charts** — use ratio-bars / split-bars
- ❌ **ROAS rendered with `×`** — never
- ❌ **Numbers without slashed zero / tabular nums** — non-negotiable
- ❌ **Recommendations without a named agent** — every claim needs attribution
- ❌ **Generic "Loading…" copy** — agent-aware shimmer skeletons instead
- ❌ **Mocking pretty data** that doesn't match the real shape — design against the data points listed above

---

## 6 · Workflow — page-by-page sign-off

For each page (priority order TBD with James):

1. **You produce a per-page design brief** (markdown, ~500 words) covering: layout grid, visual treatment per data point, mode-aware variants, state treatments, universal-component mounts. Reference the page contract above.
2. **I implement on a preview deploy branch.**
3. **James eyeballs the preview**, signs off OR pushes back on specific elements.
4. **Move to the next page only when the previous is signed off.**

No page goes to main without explicit James sign-off. No batch sign-off — one page at a time. Slow but safe.

---

## 7 · What I expect back, in this order

1. **`docs/briefs/equaliser-ux-principles-v1.md`** — your principles brief covering the 8 topics in §3
2. After I've read + agreed: **per-page design briefs** in priority order, one at a time. James will pick the order.

Standing by for your principles brief. If anything in this document is wrong, ambiguous, or contradicts something you find in the codebase, **flag it back** — don't reconcile.
