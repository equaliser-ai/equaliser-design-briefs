# Equaliser — Per-Page Design Briefs (every page, nav order)

**Date:** 2026-05-05
**For:** Claude Design — design against this; James signs off per page
**Pairs with:** `claude-design-canonical-v2-2026-05-05.md` + `equaliser-ux-principles-v1.md`

---

## How to use this document

Each page below has a fixed contract. **Sacred data points** must appear; **emphasis** is the loudest thing on the page; **states** must all be designed (not just the success path); **universal components** mount as named.

**Carrier element** is one of the nine from the principles brief — pick the right one per page from: trend line · sparkline-per-row · slider-current-to-target · saturation curve · ratio bar · fan chart · pillars motif · kanban velocity strip · before-after sparkline pair.

**Mode-aware** = Agency vs Client variants. Agency = full density, all 17 agents, technical. Client = 3-4 agents, plain English advisor framing, top items only.

**Number rules** are global — never re-state, just call out per-page exceptions if any.

**Workflow:** Claude Design produces a hi-fi page proposal → I implement on a preview branch → James eyeballs → sign off → merge. One page at a time.

---

# 1 · Dashboard

## 1.1 — Dashboard (the morning page)

**Path:** `/accounts/[client]/dashboard`

**Purpose:** the morning check-in. "Is this account OK before I start work?"
**Core question:** anything urgent before I start optimising?

**Carrier element:** **trend line** (90-day daily roll-up) above the fold.

**Layout regions:**
- *Header:* eyebrow `{Client} · Dashboard`, h1 `{Client display name}`, sub-line `Lead generation` or `E-commerce`, RAG state pill (top right)
- *Hero:* verdict banner — agent-authored one-line take + RAG state + key concern + trend direction
- *Body:* KPI tiles strip (4) + spend trend chart + top 3 findings (priority cards) + agent activity strip
- *Footer:* "Walk into a 9am call prepared" — link to today's meeting prep

**Sacred data points:**
- RAG state pill (green / amber / red / no-data)
- MTD spend vs monthly_budget (pacing %)
- Headline KPI (ROAS for ecom, CPA for lead-gen) + WoW Δ%
- Trend direction (up / flat / down)
- Top 3 findings ranked by £ forecast impact
- Agent verdict — one sentence
- Last data sync timestamp

**Agents speaking:** Sentinel (verdict), Pacing Agent (pacing line), Investment Director (top finding)

**Inline actions:**
- Click any finding card → `/optimise/plan?finding=ID`
- Click RAG pill → expand to show what drives the colour
- Snooze a finding → POST `/api/findings/[id]/transition`

**Emphasis:** RAG state pill + verdict — visible from across the room. The strategist should know the answer to "is this OK" in <2 seconds.

**States:**
- *Loading:* "Sentinel is checking the account…" with skeleton tiles
- *Empty (new account):* "No data yet — agents start tomorrow morning" + connection status
- *No data this period:* render structure with `—` values + amber state pill + "data flowing in" note
- *Error:* render last-good with timestamp + error chip

**Mode-aware:**
- Agency: full agent attribution, all 4 KPI tiles, lever badges on findings
- Client: "Your AI strategist team" attribution, headline KPI only, advisor-framed findings

**Universal components:**
- `<AgentChip>` per finding card
- `<WhyButton entity={{ type: 'Finding', id }} />` on each finding card

**Non-negotiables:**
- RAG colour decision must be explained somewhere on the page (tooltip or inline)
- Verdict must be agent-signed; never anonymous
- No "Welcome back" / "Good morning" / time-of-day framing

---

# 2 · Analyse

## 2.1 — Analyse (root)

**Path:** `/accounts/[client]/analyse`

**Purpose:** account headline. The data-layer entry point.
**Core question:** what does the period look like vs prior?

**Carrier element:** **trend line** (period-aware, switchable date range).

**Layout regions:**
- *Header:* date range picker (top-left, controls all sub-pages too via state), eyebrow + h1 `Overview`
- *Hero:* verdict banner (agent-authored period take) + 4 KPI tiles
- *Body:* spend × revenue dual-axis chart + channel split breakdown + top-3-findings strip
- *Footer:* drill links to Channels / Campaigns / Creatives

**Sacred data points:**
- Date range (selected period)
- Spend / Revenue / ROAS / Conversions (or Spend / Conversions / CPA / Revenue for lead-gen)
- WoW Δ% on each tile
- Period verdict (one sentence, agent-authored)
- Channel split (top 3 channels by spend)
- Top 3 findings by £ impact

**Agents speaking:** Trend Spotter (period verdict), Sentinel (anomaly callouts), Investment Director (finding framing)

**Inline actions:**
- Date range picker drives every sub-page within Analyse
- Click channel → `/analyse/channels`
- Click any finding → drilldown

**Emphasis:** the period verdict + the WoW % deltas

**States:**
- *Loading:* "Trend Spotter is reading the period…"
- *Empty (no spend):* "No spend in this range. Try a wider window." + range presets
- *Mid-period (today):* "Period in progress — figures update overnight" + amber chip
- *Error:* render last-good with timestamp

**Mode-aware:**
- Agency: 4 tiles, dual-axis chart, channel breakdown
- Client: 2 tiles (KPI + spend), single-axis chart, no channel split

**Universal components:**
- `<WhyButton>` on the verdict banner

**Non-negotiables:**
- Date range is the control; everything below recomputes
- Engine and GA4 numbers never merged — show one, link to the other on Attribution
- WoW deltas use proper minus glyph (−), not hyphen (-)

---

## 2.2 — Analyse → Channels

**Path:** `/accounts/[client]/analyse/channels`

**Purpose:** performance by ad platform.
**Core question:** which channels are pulling weight; which are dragging?

**Carrier element:** **ratio bar** per row (spend share + ROAS contribution) — no bubble plots.

**Layout regions:**
- *Header:* eyebrow + h1 `Channels`, brand/generic split toggle, metric set toggle (Key / Engine / GA4 / Combined)
- *Hero:* verdict banner — strategist-tone interpretation per top channel ("Search ROAS holding 4.2 vs 3.8 last week, scale candidate")
- *Body:* `DataTable` rows: channel · spend · revenue · cost · impressions · clicks · CVR · ROAS · CPA + ratio bar visual + agent commentary chip + period-compare WoW deltas
- *Footer:* drill to Campaigns

**Sacred data points:**
- Channel name (Google / Meta / Bing / TikTok / Other)
- Spend (£), revenue (£), ROAS, CPA
- Brand vs generic split (toggle filter)
- Period-compare WoW Δ%
- Strategist-tone one-liner per top channel
- Click-through to `/analyse/campaigns?channel=X`

**Agents speaking:** Sentinel, Investment Director

**Inline actions:**
- Click channel row → `/analyse/campaigns?channel=X` (drill)
- "Plan →" button per row → `/optimise/budgets?channel=X`
- Brand split filter chips (all / brand / generic / unknown)

**Emphasis:** the agent commentary chip per channel — strategist-voice, not data-pretty

**States:**
- *Loading:* "Sentinel is checking auctions…"
- *Empty (one channel only):* hide split, show single-channel breakdown
- *Lead-gen client:* hide ROAS/Revenue columns, show CPA/Conv columns

**Mode-aware:**
- Agency: full table + ratio bar + agent commentary
- Client: top 3 rows + advisor-framed commentary, no engine vs GA4 column split

**Universal components:**
- `<AgentChip>` per commentary chip
- `<WhyButton>` on each agent commentary

**Non-negotiables:**
- No bubble charts. Ratio bars or split bars only.
- Engine vs GA4 stays separate — never merge
- Channel normalised names: Google / Meta / Bing / TikTok / Affiliates / Other (canonical)

---

## 2.3 — Analyse → Campaigns

**Path:** `/accounts/[client]/analyse/campaigns`

**Purpose:** cross-channel campaign performance + drilldown from Channels.
**Core question:** which campaigns are winning, which are leaking?

**Carrier element:** **trend line** above table (90-day spend × ROAS) + **sparkline-per-row** in the table.

**Layout regions:**
- *Header:* date range, channel filter chip (cleared by × if drilled in), eyebrow + h1 `Campaigns` (with channel scope chip if filtered)
- *Hero:* spend trend line + 4 tiles (campaigns count / spend / revenue / blended ROAS)
- *Body:* `DataTable` rows: campaign · channel · campaign_type · status pip · cost · revenue · ROAS · CPA · CTR · CVR · 7-day sparkline · status pip · action verb
- *Footer:* drill to Ad groups (placeholder; future build)

**Sacred data points:**
- Campaign name + channel + campaign_type (raw + normalised)
- Status pip (enabled / paused / removed / unknown)
- Spend, revenue, ROAS vs target_roas, CPA vs target_cpa
- CTR, CVR, conversions, impressions
- 7-day ROAS sparkline per row
- Period-compare WoW Δ%
- Action verb (Scale / Reduce / Plan) based on ROAS-vs-target

**Agents speaking:** Investment Director, Bid Strategist

**Inline actions:**
- Click row → finding detail (if linked) or campaign detail page
- "Scale →" / "Reduce →" / "Plan →" per row → `/optimise/budgets?action=X&campaign=Y`
- Channel filter chip × clears scope

**Emphasis:** ROAS-vs-target column with red/amber/green colouring

**States:**
- *Loading:* "Investment Director is reviewing campaigns…"
- *Empty:* "No active campaigns in this range. Adjust the date range or check connections."
- *Channel-scoped:* show scope chip in h1 with × to clear

**Mode-aware:**
- Agency: full column set + sparkline + action verbs
- Client: name / spend / ROAS / Δ only, no sparkline, no action verbs

**Universal components:**
- `<AgentChip>` per row when finding linked
- `<WhyButton>` on rows with linked Finding

**Non-negotiables:**
- ROAS shown without `×`. Money rounds to £k for ≥10k.
- Inactive campaigns dimmed; never hidden (unless filter says so)

---

## 2.4 — Analyse → Creatives

**Path:** `/accounts/[client]/analyse/creatives`

**Purpose:** which ads are working / fatiguing.
**Core question:** what's tired, what's tearing it up?

**Carrier element:** **before-after sparkline pair** (CTR week 1 vs week 4) per row, plus thumbnail grid hero.

**Layout regions:**
- *Header:* segmentation chips (All / Top CTR / Bottom CTR / Fatigued / Healthy), format/objective filter chips, eyebrow + h1 `Creatives`
- *Hero:* thumbnail grid (top 8 by CTR or by spend) + format-mix donut
- *Body:* `DataTable` rows: thumb + headline · format · CVR · CTR · spend · frequency · fatigue %fill chip · fatigue status · action verb
- *Footer:* link to `/optimise/creative` for retire/scale flow

**Sacred data points:**
- Ad thumbnail + headline
- Ad format (Video / Static / DPA / UGC / Carousel / Reel / Story)
- Ad objective (Sales / Traffic / Awareness)
- CTR, CVR, spend, frequency
- Fatigue %fill chip (visual)
- Fatigue status (CRITICAL / HIGH / MODERATE / WATCH / HEALTHY)
- Quality ranking (Above average / Average / Below average)
- Action verb (Retire / Scale / Investigate / Healthy)

**Agents speaking:** Creative Director, Fatigue Referee

**Inline actions:**
- Click thumb → ad detail
- "Retire →" / "Scale →" per row → `/optimise/creative?action=X&ad=Y`
- Filter by fatigue status / format / objective

**Emphasis:** fatigue chip — instantly visible on every row

**States:**
- *Loading:* "Fatigue Referee is checking creative…"
- *Empty:* "No active creative in this range. Add ads or widen the date range."

**Mode-aware:**
- Agency: full column set + thumb grid + segmentation chips
- Client: thumb grid + top-3 fatigued only, no segmentation, no per-row data table

**Universal components:**
- `<AgentChip>` per row
- `<WhyButton>` on each row's fatigue claim

**Non-negotiables:**
- Thumbnails never broken — fall back to ad ID + format chip if image unavailable
- Fatigue status colour-coded per principles brief severity hierarchy

---

## 2.5 — Analyse → Audiences

**Path:** `/accounts/[client]/analyse/audiences`

**Purpose:** audience segment performance (mixed types — demographics / geo / hourly / devices / audience_type / geo_city / prospecting / meta_placement).
**Core question:** which segments to bid up / down on?

**Carrier element:** **sparkline-per-row** (CPA over 14 days) — NOT a single ratio bar across mixed types (incoherent).

**Layout regions:**
- *Header:* segment_type filter chips (Demographics / Geo / Hourly / Devices / Audience type / Geo city / Prospecting / Placement), eyebrow + h1 `Audiences`
- *Hero:* segment_type tally tiles (8) — count per type
- *Body:* `DataTable` rows GROUPED by segment_type: segment value · CPA · CVR · conversions · spend · 14-day sparkline · LAL tier chip (where applicable) · CPA delta column · bid-modifier inline buttons (-20% / -10% / +10% / +20%)
- *Footer:* Apply banner with batch count → `/api/optimise/audience-bid-modifier`

**Sacred data points:**
- segment_type (categorical — drives grouping)
- segment value (e.g. "18-24 male" / "London" / "iOS" / "1% LAL")
- CPA, CVR, conversions, spend per segment
- LAL tier chip (1% / 3% / 5%) where audience_type
- CPA delta vs account average
- Bid-modifier preset buttons (-20 / -10 / +10 / +20)
- Apply banner showing pending modifications count + total

**Agents speaking:** Audience Optimiser

**Inline actions:**
- Bid-modifier buttons per row queue a pending change
- "Apply X changes" banner action POSTs batch to `/api/optimise/audience-bid-modifier`
- Filter by segment_type chips

**Emphasis:** the bid-modifier buttons — inline action without leaving the page

**States:**
- *Loading:* "Audience Optimiser is checking segments…"
- *Empty:* "No segment data — connect Google Ads + Meta to flow targeting"
- *No conversions:* "No conversions yet — CPA delta column hidden"

**Mode-aware:**
- Agency: all segment_type groups, bid-modifier buttons
- Client: top-3 winning + top-3 losing segments only, no bid-modifier UI (read-only)

**Universal components:**
- `<AgentChip>` Audience Optimiser on each commentary
- `<EntityPanel>` open from any segment row → see related findings, recent bid changes

**Non-negotiables:**
- Group by `segment_type` first — never mix types in one ratio bar
- Lead-gen clients: hide ROAS-driven CPA delta if conversions are search-completes only
- Never show negative-bid-modifier on protected demographics (per platform policy)

---

## 2.6 — Analyse → Products

**Path:** `/accounts/[client]/analyse/products`

**Purpose:** SKU performance for ecom clients.
**Core question:** what's a hero, what's dead?

**Carrier element:** **ratio bar** (revenue share, top 10) above the table + tier badge per row.

**Layout regions:**
- *Header:* tier filter chips (All / Hero / Supporting / Underperforming / Dead), feed-health filter, eyebrow + h1 `Products`
- *Hero:* tier breakdown tiles (5) — Hero / Supporting / Underperforming / Dead / No data, click-to-filter
- *Body:* `DataTable` rows: image · GTIN · product_title · brand · revenue · units · ROAS · feed-health pip · tier badge · tier-aware action verb (Scale / Maintain / Investigate / Pause)
- *Footer:* link to `/optimise/creative?lever=products`

**Sacred data points:**
- Product image + title + GTIN + brand
- Revenue, units, ROAS per SKU
- Feed-health pip (red / amber / green)
- Tier classification (Hero / Supporting / Underperforming / Dead — quartile-aware)
- Tier-aware action verb
- Top-10 revenue ratio bar (above table)

**Agents speaking:** Product Director, Feed Optimiser (Keeper / Inspector legacy types)

**Inline actions:**
- Click image / SKU → product detail (future)
- "Scale →" / "Investigate →" / "Pause →" per row
- Filter by tier or feed-health

**Emphasis:** tier badge + action verb per row

**States:**
- *Loading:* "Product Director is classifying SKUs…"
- *Lead-gen client:* redirect to /analyse with note "Products only apply to ecom clients"
- *No GMC feed:* "No product feed connected — link Google Merchant Center to flow product data"

**Mode-aware:**
- Agency: full breakdown + tiers + actions
- Client: hero count + top-3 hero list + dead count, no tier filter

**Universal components:**
- `<AgentChip>` per row
- `<WhyButton>` on tier classification

**Non-negotiables:**
- Quartile-aware classification — small accounts shouldn't all collapse into bottom tier
- GTIN always shown — feed-health depends on it

---

## 2.7 — Analyse → Keywords

**Path:** `/accounts/[client]/analyse/keywords`

**Purpose:** search keyword performance.
**Core question:** which keywords need attention?

**Carrier element:** **sparkline-per-row** (7-day spend) + quality-score chip + match-type pip.

**Layout regions:**
- *Header:* match-type filter chips (Exact / Phrase / Broad), brand/generic filter, eyebrow + h1 `Keywords`
- *Hero:* match-type breakdown tiles (3) + quality-score histogram
- *Body:* `DataTable` rows: keyword · match-type pip · brand/generic chip · spend · clicks · CVR · CPA · QS chip · 7-day sparkline · "Add as negative" inline action for waste
- *Footer:* link to Search terms (placeholder)

**Sacred data points:**
- Keyword text
- Match type pip (Exact / Phrase / Broad)
- Brand vs generic classification
- Spend, clicks, CVR, CPA
- Quality score chip (1-10, colour-coded)
- 7-day spend sparkline
- Bid strategy column

**Agents speaking:** Bid Strategist (legacy: Wordsmith / Search-Term Analyst)

**Inline actions:**
- "Add as negative →" per row for high-spend low-CVR keywords
- Click keyword → search-term detail
- Filter by match-type or brand/generic

**Emphasis:** quality score chip — colour-coded per row

**States:**
- *Loading:* "Bid Strategist is reading keywords…"
- *Empty:* "No keyword data — connect Google Ads search campaigns"

**Mode-aware:**
- Agency: full table
- Client: top-10 by spend only, no negative-keyword action

**Universal components:**
- `<AgentChip>` Bid Strategist on commentary
- `<WhyButton>` on each "Add as negative" suggestion

**Non-negotiables:**
- Quality score colour-coded: 1-3 red, 4-6 amber, 7-10 green
- Lead-gen clients: prioritise high-intent keywords; ROAS column hidden

---

## 2.8 — Analyse → Attribution

**Path:** `/accounts/[client]/analyse/attribution`

**Purpose:** engine vs GA4 reconciliation, halo, cannibalisation.
**Core question:** what's the blended truth across measurement systems?

**Carrier element:** **ratio bar** (engine vs GA4 attribution split) + weekly ratio chart.

**Layout regions:**
- *Header:* eyebrow + h1 `Attribution`
- *Hero:* verdict banner — Cipher-led narrative ("Engine over-reports vs GA4 by 12% on Meta Search this period. Blended truth = engine × 0.62 + GA4 × 0.38")
- *Body:* engine÷GA4 ratio-over-time chart (12-week window) + per-channel breakdown table (engine / GA4 / blended truth / halo lift / cannibalisation flag)
- *Footer:* "How we built this" methodology explainer + linked recommendations

**Sacred data points:**
- Engine attribution (per channel)
- GA4 attribution (per channel)
- Blended truth (configurable weighting)
- Halo lift (where measurable)
- Cannibalisation flag (where detectable)
- 12-week ratio chart
- Methodology explainer

**Agents speaking:** Cipher (Atlas / Halo Monitor legacy types) — analytics-specialist voice

**Inline actions:**
- Click channel row → `<EntityPanel>` for that channel
- Methodology link → `/docs/methodology`

**Emphasis:** verdict banner — the strategist's interpretation, not the raw numbers

**States:**
- *Loading:* "Cipher is reconciling measurement…"
- *No GA4:* "GA4 not connected — engine-only view shown" + connection CTA
- *Lead-gen client:* swap blended-truth maths for blended-CPA framing

**Mode-aware:**
- Agency: ratio bar + per-channel breakdown + halo + cannibalisation
- Client: verdict banner only + "Want to dig deeper? Open the methodology" link

**Universal components:**
- `<WhyButton>` on the page-level Cipher analysis
- `<EntityPanel>` from any channel row

**Non-negotiables:**
- Engine and GA4 never merged in raw — only blended truth synthesises them
- Methodology must be one click away — claim trail is part of trust

---

## 2.9 — Analyse → Trends

**Path:** `/accounts/[client]/analyse/trends`

**Purpose:** WoW / MoM / YoY anomaly detection.
**Core question:** what changed this week and is it a problem?

**Carrier element:** **trend line** with anomaly markers + commentary chips per anomaly.

**Layout regions:**
- *Header:* WoW / MoM / YoY toggle, eyebrow + h1 `Trends`
- *Hero:* 3-most-recent anomaly headline cards with strategist commentary
- *Body:* trend chart (90-day) with anomaly markers + per-anomaly chip rows: severity pip · metric · delta · agent commentary
- *Footer:* link to `/optimise/recommendations` for ranked actions

**Sacred data points:**
- Anomaly chip per row with strategist commentary ("Spend +45% week, revenue keeping pace, healthy expansion")
- Severity pip (critical / high / medium / low / info)
- Metric affected (Spend / Revenue / ROAS / CPA / CVR / CTR)
- Delta (% and absolute)
- Window (WoW / MoM / YoY)
- Trend chart with markers

**Agents speaking:** Trend Spotter (Pulse), Sentinel

**Inline actions:**
- Click anomaly chip → finding detail
- "Investigate →" per anomaly → `/optimise/plan?finding=ID`
- WoW / MoM / YoY toggle filters anomalies

**Emphasis:** the anomaly chips — page is narrated, not mechanical

**States:**
- *Loading:* "Trend Spotter is reading the period…"
- *No anomalies:* "Nothing material this period — Trend Spotter is watching" (positive empty state)

**Mode-aware:**
- Agency: all anomalies + chart markers + commentary
- Client: top-3 anomalies as advisor-framed cards, no chart, no severity ladder

**Universal components:**
- `<AgentChip>` per anomaly
- `<WhyButton>` on each anomaly commentary

**Non-negotiables:**
- Page is narrated — every anomaly has interpretation, not just delta
- Commentary uses strategist voice (first person, plain English)

---

## 2.10 — Analyse → Website

**Path:** `/accounts/[client]/analyse/website`

**Purpose:** landing page intent + funnel health.
**Core question:** where are we losing customers?

**Carrier element:** **6-step funnel visual** + per-LP intent score callout.

**Layout regions:**
- *Header:* eyebrow + h1 `Website`
- *Hero:* 6-step funnel (Impressions → Clicks → Sessions → Add to cart → Checkout → Purchase) with worst-drop step highlighted
- *Body:* per-LP table: URL · sessions · CVR · bounce-inverse · engagement · intent score (0-100) · hot/warm/cold tier · action verb
- *Footer:* link to `/optimise/creative?lever=landing_page`

**Sacred data points:**
- 6-step funnel volumes + drop-off per step
- Worst-drop callout (e.g. "biggest leak: 38% lost between Add to cart and Checkout")
- Per-LP intent score (composite of CVR, bounce-inverse, engagement)
- Intent tier (hot / warm / cold)
- Action verb per row (Optimise / Investigate / Healthy)

**Agents speaking:** Inspector (legacy: landing_page_auditor)

**Inline actions:**
- "Optimise →" per LP → `/optimise/plan?lever=landing_page&lp=URL`
- Click funnel step → step detail / breakdown

**Emphasis:** worst-drop callout — "biggest leak: ..." in red-bordered panel

**States:**
- *Loading:* "Inspector is auditing pages…"
- *Lead-gen client:* hide Add to cart / Checkout steps, show 4-step funnel (Impressions → Clicks → Sessions → Conversions)
- *No GA4:* "GA4 not connected — page-level data unavailable" + CTA

**Mode-aware:**
- Agency: full funnel + per-LP table + intent scoring
- Client: funnel + worst-drop callout only, no per-LP table

**Universal components:**
- `<WhyButton>` on intent-score column

**Non-negotiables:**
- Funnel adapts to ecom vs lead-gen — never show Add to cart for lead-gen
- Worst-drop callout always present, even if drop-off is small (just less prominent)

---

# 3 · Optimise

## 3.1 — Optimise → Recommendations (root)

**Path:** `/accounts/[client]/optimise`

**Purpose:** ranked agent recommendations. The morning action surface.
**Core question:** what should I do this week, in priority order?

**Carrier element:** **slider current → target** per recommendation card + 8-day sparkline.

**Layout regions:**
- *Header:* view toggle (Cards / Table), filter chips by lever / severity / state, eyebrow + h1 `Recommendations`
- *Hero:* tally tiles (4) — open count / £ forecast impact / fix-now critical / levers in play
- *Body:* card feed (default) or table view — each card = one Finding
- *Footer:* "Add to plan" bulk action

**Sacred data points (per card):**
- Severity verb chip (Fix / Investigate / Scale / Review)
- Agent author chip (single + chained collaborators)
- Soft-toned headline (no platform jargon)
- One-line evidence excerpt
- £ forecast impact (right side, large mono)
- Cost-of-inaction line (where present)
- Trade-off line (where present)
- Confidence chip (low / medium / high)
- Lever badge (budget / bid / audience / creative / keyword / etc.)
- Inline action row: Approve · Send to plan · Snooze 24h · Dismiss
- WhyButton on footer
- Related findings link (where N>0)

**Agents speaking:** Investment Director, Bid Strategist, Creative Director, Audience Optimiser, Budget Optimiser, Product Director, Forecaster

**Inline actions:**
- Approve → POST `/api/findings/[id]/transition` + `/api/recommendations/feedback`
- Send to plan → `/api/findings/[id]/transition` (state=assigned)
- Snooze 24h → `/api/findings/[id]/transition` (state=snoozed)
- Dismiss → `/api/findings/[id]/transition` (state=archived)

**Emphasis:** £ forecast impact + the verb chip — the answer to "should I do this"

**States:**
- *Loading:* "Investment Director is ranking actions…"
- *Empty (queue clear):* "Queue clear — agents found nothing material this period. Refreshes nightly."
- *Filter empty:* "No recommendations match this filter. Try widening."

**Mode-aware:**
- Agency: full card with all chips + bulk actions
- Client: top-3-5 cards only, advisor-framed verbs ("worth a look" instead of "Fix"), agent attribution as "your AI strategist team"

**Universal components:**
- `<AgentChip>` for primary author + chained collaborators
- `<WhyButton entity={{ type: 'Finding', id }} />` on each card footer

**Non-negotiables:**
- Every recommendation must carry a named agent. No anonymous claims.
- £ impact in mono tabular-nums, slashed-zero, no decimals under £100
- Verb chip colour-coded per severity hierarchy

---

## 3.2 — Optimise → Plan (kanban)

**Path:** `/accounts/[client]/optimise/plan`

**Purpose:** Monday-style kanban for findings lifecycle.
**Core question:** what's queued, in flight, done?

**Carrier element:** **kanban velocity strip** above columns.

**Layout regions:**
- *Header:* "+ Add task" CTA, filter chips, eyebrow + h1 `Plan`
- *Hero:* velocity strip — "12 new this week · 8 approved · 3 applied · projected £14k impact"
- *Body:* 5 columns (New → Reviewed → Assigned → Approved → Applied), each card has: verb · soft-toned title · owner avatar · deadline countdown · £ impact · agent + lever chips · aged-7d red border · move-to-next button
- *Footer:* none

**Sacred data points (per card):**
- Verb (Fix / Investigate / Scale / Review)
- Soft-toned title
- Owner avatar (initials) + deadline countdown
- £ forecast impact (right corner, mono)
- Aged 7d+ red border
- Agent author chip + lever chip
- Move-to-next-state button
- WhyButton + OutcomeChip (Applied column only)

**Agents speaking:** all (depending on Finding source)

**Inline actions:**
- Move to next state → POST `/api/findings/[id]/transition`
- Add manual task → POST `/api/findings`
- Open card → finding detail

**Emphasis:** Applied column with `<OutcomeChip>` showing classified outcome — proof-point for clients

**States:**
- *Loading:* skeleton kanban with grey cards
- *Empty new:* "No new findings — agents look overnight"
- *Aged cards:* red border + "X days aged" chip

**Mode-aware:**
- Agency: 5 columns, all cards, bulk-state actions
- Client: 2 columns (Approved / Applied) only, no manual-add, read-only kanban

**Universal components:**
- `<AgentChip>` per card
- `<WhyButton>` per card
- `<OutcomeChip actionId={card.id} />` on every Applied column card

**Non-negotiables:**
- Aged-7d red border non-negotiable — visible debt
- OutcomeChip required on Applied — proves the platform learns
- Single-user state for pilot; realtime when multi-user

---

## 3.3 — Optimise → Budgets

**Path:** `/accounts/[client]/optimise/budgets`

**Purpose:** scenario sliders for budget reallocation.
**Core question:** if I shift £X from A to B, what happens?

**Carrier element:** **saturation curve** per channel + slider current-to-target.

**Layout regions:**
- *Header:* "Reset" + "Apply" buttons, eyebrow + h1 `Budgets`
- *Hero:* total-spend bar + projected ROAS delta (top right, large mono)
- *Body:* per-channel rows: channel · current spend · slider (-50% to +100%) · saturation curve preview · agent recommendation marker (green dotted) · projected revenue · current vs target ROAS
- *Footer:* total reallocation tally + "Apply X changes" → `/api/optimise/budget-scenario`

**Sacred data points:**
- Per-channel current spend
- Slider position (-50% to +100%)
- Saturation curve (y = a × (1 - exp(-spend/k)))
- Agent recommendation marker
- Projected revenue at slider position
- Current ROAS vs target_roas
- Total reallocation tally + projected blended ROAS delta

**Agents speaking:** Budget Optimiser, Investment Director

**Inline actions:**
- Drag slider → live recompute saturation curve + projected revenue
- "Apply X changes" → POST `/api/optimise/budget-scenario` (queues to Plan kanban)

**Emphasis:** projected ROAS delta in the header — the answer to "should I do this"

**States:**
- *Loading:* "Budget Optimiser is modelling scenarios…"
- *No saturation data:* show linear projection + amber chip "Saturation modelling not yet available — projection is linear"

**Mode-aware:**
- Agency: per-channel sliders + saturation + agent recommendations
- Client: total-spend slider + projected ROAS only, no per-channel detail

**Universal components:**
- `<AgentChip>` per agent recommendation marker
- `<WhyButton>` on the agent-recommendation marker

**Non-negotiables:**
- Sliders bounded -50% / +100% — never allow zero spend on a live channel via slider
- Saturation curve uses Newton iteration so derivative at current spend matches current ROAS

---

## 3.4 — Optimise → Creative

**Path:** `/accounts/[client]/optimise/creative`

**Purpose:** creative segmentation + retire/scale flow.
**Core question:** what creative needs intervention now?

**Carrier element:** **before-after sparkline pair** (CTR over time) per ad + thumbnail grid.

**Layout regions:**
- *Header:* segmentation chips (All / Top CTR / Bottom CTR / Fatigued / Healthy), format/objective filter chips, eyebrow + h1 `Creative`
- *Hero:* retire table (top of fold) + scale table (below fold) — two prioritised lists
- *Body:* full creative table with per-row buttons (Retire / Scale / Investigate)
- *Footer:* link to creative builder (placeholder)

**Sacred data points:**
- Ad thumbnail + headline + format
- CTR vs account average
- Frequency
- Fatigue %fill chip + status
- Spend
- Action verb per row (Retire / Scale / Investigate / Healthy)
- Before-after sparkline per row (CTR week 1 vs week 4)

**Agents speaking:** Creative Director, Fatigue Referee

**Inline actions:**
- "Retire →" per row → POST `/api/optimise/creative-action` (queues to Plan)
- "Scale →" per row → POST same
- Bulk retire/scale via header bulk-action

**Emphasis:** the retire table — biggest immediate win

**States:**
- *Loading:* "Creative Director is reviewing ads…"
- *Empty retire:* "No fatigued creative right now — nothing to retire"
- *Empty scale:* "No clear scale candidates — Fatigue Referee is watching"

**Mode-aware:**
- Agency: retire + scale + segmentation + bulk actions
- Client: top-3 retire + top-3 scale only, no full table

**Universal components:**
- `<AgentChip>` per row
- `<WhyButton>` on each retire/scale claim
- `<OutcomeChip>` on previously-applied rows

**Non-negotiables:**
- Thumbnail fallback: ad ID + format chip if image broken
- Before-after sparkline visible at row scale, never collapsed

---

## 3.5 — Optimise → Automation

**Path:** `/accounts/[client]/optimise/automation`

**Purpose:** skill template gallery + run history.
**Core question:** what scheduled work is running, and what could I add?

**Carrier element:** **kanban velocity strip** showing this-week's automated outputs.

**Layout regions:**
- *Header:* skill filter chips, eyebrow + h1 `Automation`
- *Hero:* "What's running right now" panel — operational visibility, real-time status
- *Body:* skill template gallery (6 cards) + run history feed (status pip · skill name · trigger · duration · output summary)
- *Footer:* "Add custom skill" CTA (advanced)

**Sacred data points:**
- Skill template name + description + agent attribution + trigger type + Add button
- Per-run: status pip, skill name, trigger type, duration, output summary or error, timestamp
- "What's running" live status

**Agents speaking:** all (depending on skill)

**Inline actions:**
- "Add" template → POST `/api/skills` with full skill body
- Filter run history by skill or status
- Click run row → run detail / output

**Emphasis:** "What's running right now" panel — operational visibility

**States:**
- *Loading:* "Listing scheduled work…"
- *Empty templates:* show all 6 templates as gallery (no filter applied)
- *No runs yet:* "No skill runs yet — pick a template above to start"

**Mode-aware:**
- Agency: full gallery + run history + custom skill
- Client: read-only — what's running for them, no add/edit

**Universal components:**
- `<AgentChip>` per template + per run
- `<WhyButton>` on each completed run's output

**Non-negotiables:**
- "What's running right now" updates in near-realtime (poll or SSE)
- Templates strategist-tone — never developer-jargon

---

## 3.6 — Optimise → Testing

**Path:** `/accounts/[client]/optimise/testing`

**Purpose:** test backlog + experiment outcomes.
**Core question:** what experiments are queued, in flight, complete?

**Carrier element:** **before-after sparkline pair** per completed test + DataTable for backlog.

**Layout regions:**
- *Header:* test status filter (All / Queued / In flight / Complete), eyebrow + h1 `Test backlog`
- *Hero:* learnings card — aggregate win rate, avg lift, top win highlight
- *Body:* test idea gallery (6 templates) + active tests table + completed tests table
- *Footer:* "Add custom test" CTA

**Sacred data points:**
- Per template: name, hypothesis, agent attribution, duration estimate, Add button
- Per active test: name, hypothesis, started date, days-running, projected end, "View progress" link
- Per completed test: name, outcome verdict, lift % vs control, agent's reading, "Apply learning" link
- Learnings aggregate: win rate, avg lift, top win

**Agents speaking:** Investment Director, Creative Director, Audience Optimiser, Bid Strategist, PMax Intelligence

**Inline actions:**
- Add template → POST `/api/optimise/queue-test-idea`
- "Apply learning" on completed → queues a recommendation

**Emphasis:** learnings card — proof the platform learns over time

**States:**
- *Loading:* "Listing tests…"
- *No tests yet:* show 6 templates as gallery + "Pick one to start" prompt
- *No completes yet:* learnings card hidden, replaced by "First test results land in 2 weeks"

**Mode-aware:**
- Agency: all sections + custom test
- Client: completed tests + learnings card only, no backlog UI

**Universal components:**
- `<AgentChip>` per template + per test
- `<WhyButton>` on each test verdict

**Non-negotiables:**
- Hypothesis required on every test — reject submissions without one
- Outcome verdict from real significance test, not just "X > Y by 5%"

---

# 4 · Equalise

## 4.1 — Equalise → Strategy (root)

**Path:** `/accounts/[client]/equalise`

**Purpose:** strategic hub — audit, scenarios, forecasts, opportunities, competition.
**Core question:** what's the bigger picture story I'm telling about this account?

**Carrier element:** **pillars motif** (SCALE 5-pillar dot/bar) — Bronze → Silver → Gold tier badge.

**Layout regions:**
- *Header:* eyebrow + h1 `Strategy`
- *Hero:* SCALE pillar visual + tier badge — "this account is **Silver** maturity (62/100)"
- *Body:* surface tile grid (5) — Audit / Scenarios / Forecasts / Opportunities / Competition + recent strategist-tone narrative card (Narrator-authored)
- *Footer:* "Open strategic deck" CTA → /deliver/strategy-decks

**Sacred data points:**
- SCALE pillar scores (Structure / Creative / Audience / Levers / Effectiveness — each 0-100)
- Bronze / Silver / Gold tier badge
- Recent narrative paragraph (Narrator)
- 5 strategic surface tiles with descriptions

**Agents speaking:** Narrator, Structure Auditor, Forecaster, Investment Director

**Inline actions:**
- Click tile → drill to sub-page
- "Open deck" → `/deliver/strategy-decks/new`

**Emphasis:** SCALE pillar visual — this is the lead-hook for client decks

**States:**
- *Loading:* "Structure Auditor is reviewing the account…"
- *Empty (new account):* "Audit hasn't run yet — first audit completes in 24h"

**Mode-aware:**
- Agency: 5 tiles + pillar visual + tier badge
- Client: pillar visual + tier badge + 1 narrator paragraph only, no sub-tiles (those are agency-internal)

**Universal components:**
- `<AgentChip>` Narrator on the narrative card
- `<WhyButton>` on the SCALE score

**Non-negotiables:**
- SCALE letters always Structure / Creative / Audience / Levers / Effectiveness — locked in `lib/scale-pillars.ts`
- Tier badge prominent — Bronze / Silver / Gold per total-score banding

---

## 4.2 — Equalise → Audit

**Path:** `/accounts/[client]/equalise/audit`

**Purpose:** SCALE 5-pillar maturity audit. **Lead-hook page — designed to be screenshot-able into a client deck.**
**Core question:** how mature is this account, what moves take it from current → aspirational?

**Carrier element:** **pillars motif** (hero, dark background, Hero gradient legitimate).

**Layout regions:**
- *Header:* eyebrow + h1 `Audit`
- *Hero:* SCALE 5-pillar score visual (current vs aspirational) — uses Hero gradient on this cluster
- *Body:* tier badge (Bronze / Silver / Gold), then per-pillar 30/60/90-day roadmap cards, then "Stack the moves above and SCALE goes from X → Y" aggregate banner
- *Footer:* "Export audit PDF" CTA → `/api/score/export`

**Sacred data points:**
- 5 pillars × current score (0-100) × aspirational target × delta to fill
- Per-pillar 30/60/90-day roadmap with specific moves + projected score lift per move
- "Queue →" deep-link per move
- Total current SCALE score
- Aggregate "stack the moves → projected total"
- Bronze / Silver / Gold tier
- Pillar-specific notes (what's good, what's gappy)

**Agents speaking:** Structure Auditor, Investment Director (synthesis)

**Inline actions:**
- "Queue →" per move → adds to Plan kanban via `/api/findings`
- "Export PDF" → `/api/score/export`

**Emphasis:** SCALE pillar visual + tier badge — the client-deck hero

**States:**
- *Loading:* "Structure Auditor is reviewing the account…"
- *No data per pillar:* render the pillar with grey state + "data flowing" note + "minimum 28 days to score" rule

**Mode-aware:**
- Agency: full audit + roadmap + Queue actions
- Client: pillar hero + tier + 90-day summary banner only, no Queue actions (those are agency-internal)

**Universal components:**
- `<AgentChip>` per agent recommendation
- `<WhyButton>` on each pillar score

**Non-negotiables:**
- SCALE letters from `lib/scale-pillars.ts` — never hand-typed
- Hero gradient on pillar cluster (legitimate hero moment)
- Roadmap moves must each project a specific score lift
- Aggregate banner does the maths visibly: 56 + 8 + 6 + 4 + 4 → 78

---

## 4.3 — Equalise → Forecasts

**Path:** `/accounts/[client]/equalise/forecasts`

**Purpose:** multi-scenario projections.
**Core question:** what does the next 8 weeks look like under different decisions?

**Carrier element:** **fan chart** (P10 / P50 / P90) — three projection lines on one chart.

**Layout regions:**
- *Header:* time horizon selector (4 / 8 / 12 weeks), eyebrow + h1 `Forecasts`
- *Hero:* fan chart with 3 lines: current pace · agent-recommended · aggressive growth — confidence bands
- *Body:* 3 scenario cards below — per scenario: spend / revenue / ROAS tally + delta vs anchor
- *Footer:* "Pin scenario to plan" CTA → `/optimise/scenarios`

**Sacred data points:**
- 3 projection lines (current pace / agent-recommended / aggressive growth)
- Confidence bands (P10 / P90 around P50)
- Per-scenario tallies: spend, revenue, ROAS, conversions
- Delta vs anchor scenario
- Time horizon (4 / 8 / 12 weeks)

**Agents speaking:** Forecaster, Investment Director

**Inline actions:**
- Switch time horizon
- "Pin to plan →" per scenario → POST `/api/optimise/pin-scenario`

**Emphasis:** confidence bands — visible uncertainty, not single-point guesses

**States:**
- *Loading:* "Forecaster is modelling scenarios…"
- *No baseline:* "Forecasting needs 28 days of spend data — current account at X days"

**Mode-aware:**
- Agency: 3 scenarios + fan chart + pin actions
- Client: hero fan chart + 1 advisor-framed paragraph only, no pin actions

**Universal components:**
- `<AgentChip>` Forecaster on the verdict
- `<WhyButton>` on the methodology

**Non-negotiables:**
- Three lines minimum — single linear projection is banned (per-page non-neg)
- Confidence bands visible, not buried

---

## 4.4 — Equalise → Opportunities

**Path:** `/accounts/[client]/equalise/opportunities`

**Purpose:** AI-ranked growth opportunities.
**Core question:** where's the next £ of growth coming from?

**Carrier element:** **ratio bar** of headroom % per opportunity + DataTable.

**Layout regions:**
- *Header:* opportunity-type filter chips, eyebrow + h1 `Opportunities`
- *Hero:* tally tiles (4) — total headroom £ / open count / quick-win count / strategic-bet count
- *Body:* `DataTable` rows: opportunity headline · opportunity type · £ headroom · agent author · action verb · Review → button
- *Footer:* link to `/optimise/recommendations` for ranked actions

**Sacred data points:**
- Opportunity headline (strategist-tone)
- Opportunity type (audience / creative / channel-mix / structure / measurement)
- £ headroom (sortable column)
- Headroom ratio % (vs current revenue)
- Agent author per opportunity
- Action verb (Scale / Test / Investigate)
- Review → button

**Agents speaking:** Investment Director, Audience Optimiser, Product Director, Forecaster

**Inline actions:**
- "Review →" per row → finding detail page
- Filter by opportunity type

**Emphasis:** £ headroom column — sortable

**States:**
- *Loading:* "Investment Director is ranking opportunities…"
- *No opportunities:* "No material opportunities right now — Investment Director is watching"

**Mode-aware:**
- Agency: full table + filter chips
- Client: top-3 opportunities only, advisor-framed, no agent attribution column

**Universal components:**
- `<AgentChip>` per row
- `<WhyButton>` per row

**Non-negotiables:**
- Headroom ratio uses `headroom_pct` from `v_growth_opportunities` directly (already a fraction) — never multiplied
- Opportunity-type taxonomy locked

---

## 4.5 — Equalise → Scenarios

**Path:** `/accounts/[client]/equalise/scenarios`

**Purpose:** save-and-compare budget scenarios.
**Core question:** which budget shape wins for next month?

**Carrier element:** **stacked channel-mix bar** per scenario + side-by-side comparison panel.

**Layout regions:**
- *Header:* "Compare selected" CTA, eyebrow + h1 `Scenarios`
- *Hero:* multi-select panel (up to 4) for side-by-side comparison
- *Body:* scenario cards grid — per scenario: name · budget total · forecast revenue · forecast ROAS · forecast conversions + delta vs anchor + stacked channel-mix bar + Pin to plan
- *Footer:* "Create new scenario" CTA → `/optimise/budgets`

**Sacred data points:**
- Per scenario: name, budget total, forecast revenue, forecast ROAS, forecast conversions
- Delta vs anchor scenario (one is anchor)
- Stacked channel-mix bar (visual)
- Pin to plan button → POST `/api/optimise/pin-scenario`

**Agents speaking:** Investment Director, Forecaster

**Inline actions:**
- Multi-select up to 4 → side-by-side panel above grid
- "Pin to plan →" per scenario → POST same as Forecasts

**Emphasis:** delta vs anchor — comparison is the value, not absolute numbers

**States:**
- *Loading:* "Forecaster is computing scenarios…"
- *Empty (no saved scenarios):* "No saved scenarios — create one in `/optimise/budgets`" + CTA

**Mode-aware:**
- Agency: full multi-select + comparison panel + pin
- Client: read-only — scenarios + their projections, no select/pin

**Universal components:**
- `<AgentChip>` per scenario
- `<WhyButton>` on agent's recommended scenario

**Non-negotiables:**
- Anchor scenario must be designated (current pace usually) — every delta is vs anchor
- Stacked bar uses canonical channel colours, never re-mapped

---

## 4.6 — Equalise → Competition

**Path:** `/accounts/[client]/equalise/competition`

**Purpose:** share-of-voice + competitive intel.
**Core question:** are we gaining or losing share?

**Carrier element:** **trend line** — your IS vs top 3 competitors, weekly.

**Layout regions:**
- *Header:* competitor filter chips, eyebrow + h1 `Competition`
- *Hero:* SoV trend chart (your line + top 3 competitors)
- *Body:* 3 momentum cards — you (gaining/losing/holding share) + biggest gainer + biggest faller
- *Footer:* "Reactive recs" panel — "respond to competitor X move" recommendations

**Sacred data points:**
- Your impression-share line (weekly)
- Top 3 competitors' IS-overlap lines
- Momentum cards: state (gaining / losing / holding) + delta % over period
- Biggest gainer + biggest faller competitor names + delta
- Reactive recommendations (linked findings)

**Agents speaking:** Competitive Intel (Shadow legacy)

**Inline actions:**
- Click competitor name → competitor detail / their recent ad creative
- Click reactive rec → finding detail

**Emphasis:** the trend chart — momentum > snapshot

**States:**
- *Loading:* "Competitive Intel is reading auctions…"
- *No competitor data:* "No auction-insight data — only Google Ads search supports this" + CTA
- *Lead-gen client:* hide CPA-driven framing, swap to "lead share" framing

**Mode-aware:**
- Agency: full chart + 3 momentum + reactive recs
- Client: chart + 1 momentum card only, no reactive recs

**Universal components:**
- `<AgentChip>` Competitive Intel on momentum cards
- `<WhyButton>` on each reactive rec

**Non-negotiables:**
- Competitor names locked from `competitor_config` — never seeded with placeholders
- IS data only from auction insights (Google Ads); never simulated

---

# 5 · Deliver

## 5.1 — Deliver (root)

**Path:** `/accounts/[client]/deliver`

**Purpose:** client outputs hub. What's going to the client this period.
**Core question:** what's due, what's been sent, what's being prepared?

**Carrier element:** **kanban velocity strip** showing in-flight deliverables.

**Layout regions:**
- *Header:* eyebrow + h1 `Delivery`
- *Hero:* "Deliverable status" verdict — in-flight count + most-recent + "next due"
- *Body:* tile grid (6) — Reports / Activity / Meetings / Strategy decks / Templates / Commentary
- *Footer:* "Send weekly pack" CTA

**Sacred data points:**
- In-flight deliverables count + names + due dates
- Most-recent delivered (with timestamp)
- "Next due" surface
- 6 deliverable surface tiles

**Agents speaking:** Narrator, Investment Director

**Inline actions:**
- Click tile → sub-page
- "Send pack" → opens scheduled report flow

**Emphasis:** what's due this week — top of page

**States:**
- *Loading:* "Loading delivery surface…"
- *Empty (no scheduled outputs):* "No deliverables scheduled — set cadence in /settings/digest"

**Mode-aware:**
- Agency: all 6 tiles + verdict + send pack
- Client: ⛔ this page is agency-only — clients see their delivered outputs in their portal, not this admin surface

**Universal components:**
- `<AgentChip>` Narrator on commentary card

**Non-negotiables:**
- "Next due" must be visible from the fold

---

## 5.2 — Deliver → Reports

**Path:** `/accounts/[client]/deliver/reports`

**Purpose:** report editor + browse history.
**Core question:** what's due, what's sent, what's archived?

**Carrier element:** **DataTable** with status pip + sparkline-per-row (engagement signal).

**Layout regions:**
- *Header:* filter chips (Due this week / Awaiting send / Sent / Archived), "+ New report" CTA, eyebrow + h1 `Reports`
- *Hero:* tally tiles (4) — due this week / awaiting / sent in last 14d / open rate avg
- *Body:* `DataTable` rows: title · kind (weekly / bi-weekly / monthly / QBR) · week_start · status pip · last_updated · agent draft? · open rate (if sent) · Edit / Send buttons
- *Footer:* "Schedule cadence" → `/settings/digest`

**Sacred data points:**
- Report title + kind + week_start + status (draft / awaiting_review / sent / archived)
- Last_updated timestamp
- Open rate (if sent, from engagement)
- Send action + Edit action
- "Due this week" filter prominent

**Agents speaking:** Narrator (drafts the report)

**Inline actions:**
- "Edit" → report editor
- "Send" → POST `/api/reports/[id]/send`
- Filter chips persist

**Emphasis:** "Due this week" filter — the most-used view

**States:**
- *Loading:* "Loading reports…"
- *Empty:* "No reports — schedule cadence in /settings/digest"
- *No engagement yet:* hide open-rate column

**Mode-aware:**
- Agency: full table + send flow
- Client: their portal version, read-only

**Universal components:**
- `<AgentChip>` Narrator on draft rows
- `<WhyButton>` on the agent's draft

**Non-negotiables:**
- Report status pip canonical: draft / awaiting_review / sent / archived

---

## 5.3 — Deliver → Activity

**Path:** `/accounts/[client]/deliver/activity`

**Purpose:** chronological log of agent + human activity.
**Core question:** what happened on this account in the last X days?

**Carrier element:** **timeline visual** — vertical activity stream with agent attribution per row.

**Layout regions:**
- *Header:* day filter (7 / 14 / 30 / 90), agent filter chips, eyebrow + h1 `Activity`
- *Hero:* tally tiles (3) — runs 7d / findings 7d / decisions 7d
- *Body:* timeline rows: kind icon · timestamp · agent or user · summary · severity pip
- *Footer:* "Filter by client" toggle (portfolio view)

**Sacred data points:**
- Per-row: kind (run / finding / decision / commentary / report-sent / etc.), timestamp, agent (if agent-authored) or user, summary, severity pip
- Filter by agent / kind / day window

**Agents speaking:** all (each row attributes to its source)

**Inline actions:**
- Click row → entity detail (run / finding / report etc.)
- Filter by agent name or kind

**Emphasis:** agent attribution per row — audit trail of who did what

**States:**
- *Loading:* "Loading activity…"
- *Empty (new account):* "Account just created — first activity tomorrow"

**Mode-aware:**
- Agency: full timeline + filters
- Client: agency outputs only (commentary / report-sent / decision-applied) — no internal-runs noise

**Universal components:**
- `<AgentChip>` per row
- `<WhyButton>` for agent-authored rows

**Non-negotiables:**
- Time-ordered, immutable — never edit-in-place
- Agent rows always carry agent attribution

---

## 5.4 — Deliver → Meetings

**Path:** `/accounts/[client]/deliver/meetings`

**Purpose:** meeting prep.
**Core question:** what should I cover in tomorrow's call?

**Carrier element:** **DataTable** of anticipated issues + talking points cards.

**Layout regions:**
- *Header:* "+ New meeting" CTA, meeting selector dropdown, eyebrow + h1 `Meetings`
- *Hero:* "Next meeting" verdict — meeting name + date + 1-line top issue
- *Body:* anticipated issues list with Review → button + talking points cards (3-5) + recent decisions linked
- *Footer:* "Generate prep deck" → `/deliver/strategy-decks/new`

**Sacred data points:**
- Meeting name + date + attendees
- Anticipated issues (per-row: issue summary, severity, agent author, Review →)
- Talking points (3-5 cards with strategist-tone framing)
- Recent decisions linked

**Agents speaking:** Investment Director (issue framing), Narrator (talking points)

**Inline actions:**
- "Review →" per anticipated issue → finding detail
- "Generate deck" → strategy deck builder

**Emphasis:** anticipated issues — the prep value-add

**States:**
- *Loading:* "Investment Director is preparing meeting brief…"
- *Empty (no scheduled meetings):* "No meetings scheduled — add via calendar integration"

**Mode-aware:**
- Agency: full prep panel + Generate deck
- Client: not visible — client doesn't see agency meeting prep

**Universal components:**
- `<AgentChip>` per anticipated issue
- `<WhyButton>` per issue

**Non-negotiables:**
- Anticipated issues must reference real findings (linked, not abstract)
- Talking points use strategist voice

---

## 5.5 — Deliver → Strategy decks

**Path:** `/accounts/[client]/deliver/strategy-decks`

**Purpose:** generate / browse client-facing decks.
**Core question:** what decks have I produced; can I generate one for tomorrow?

**Carrier element:** **DataTable** + thumbnail grid of recent decks.

**Layout regions:**
- *Header:* "+ Generate new deck" CTA, eyebrow + h1 `Strategy decks`
- *Hero:* deck templates gallery (5-6 templates per use-case)
- *Body:* recent decks table — title · created date · status (draft / approved / sent) · share link · download PDF
- *Footer:* "Edit template" link → `/deliver/templates`

**Sacred data points:**
- Per template: name, use-case description, sections list, agent author, generate button
- Per deck: title, created, status, share-link, PDF download

**Agents speaking:** Narrator (drafts deck content), Investment Director (deck framing)

**Inline actions:**
- "Generate" template → builds deck via `/api/decks/generate`
- "Share" → public link
- "Download PDF" → server-side render

**Emphasis:** templates gallery — frictionless start

**States:**
- *Loading:* "Loading decks…"
- *Empty:* "No decks yet — pick a template above to start"

**Mode-aware:**
- Agency: full templates + history + generate
- Client: their decks read-only via portal

**Universal components:**
- `<AgentChip>` Narrator + Investment Director on each deck

**Non-negotiables:**
- Deck status canonical: draft / approved / sent
- Templates strategist-tone, never developer-jargon

---

## 5.6 — Deliver → Templates

**Path:** `/accounts/[client]/deliver/templates`

**Purpose:** report + deck templates editor.
**Core question:** what reusable templates do I have, and can I tweak them?

**Carrier element:** card grid of templates, lightweight.

**Layout regions:**
- *Header:* "+ New template" CTA, type filter (Report / Deck / Email), eyebrow + h1 `Templates`
- *Hero:* template type tiles (3)
- *Body:* template cards grid — name · type · last_used · sections preview · Edit / Duplicate buttons
- *Footer:* link to `/settings/digest` for delivery cadence

**Sacred data points:**
- Per template: name, type, last_used timestamp, sections list, agent fields used

**Agents speaking:** none directly — templates use agent fields when filled

**Inline actions:**
- "Edit" → template editor
- "Duplicate" → new template from this one
- Filter by type

**Emphasis:** last_used timestamp — sort by recency by default

**States:**
- *Loading:* "Loading templates…"
- *Empty:* "No custom templates — system templates available in editor"

**Mode-aware:**
- Agency: full editor + create
- Client: not visible

**Universal components:**
- `<EntityPanel>` from any template → see decks + reports built from it

**Non-negotiables:**
- Template fields strongly-typed — agent fields validated against agent registry

---

## 5.7 — Deliver → Commentary

**Path:** `/accounts/[client]/deliver/commentary`

**Purpose:** strategist-tone narrative drafting per period.
**Core question:** what's the story I'm telling about this period?

**Carrier element:** **before-after sparkline pair** (period vs prior) + agent draft + human edit.

**Layout regions:**
- *Header:* period selector (Weekly / Monthly / QBR), eyebrow + h1 `Commentary`
- *Hero:* current-period narrative card — agent draft on left, human edit on right (or stacked on mobile)
- *Body:* edit history list — per-row: timestamp, who edited, diff summary, agent vs human
- *Footer:* "Save final" → POST `/api/commentary/[period]/finalise`

**Sacred data points:**
- Period (weekly / monthly / QBR)
- Agent's initial draft (text)
- Human-edited final (text)
- Edit diff history per save
- Save-final action

**Agents speaking:** Narrator (drafts the narrative)

**Inline actions:**
- "Save draft" → checkpoint
- "Save final" → POST finalise → triggers report attach
- "Re-draft" → re-runs Narrator on current data

**Emphasis:** the diff history — provenance for the client

**States:**
- *Loading:* "Narrator is drafting commentary…"
- *Empty:* "No data yet for this period — Narrator runs end-of-period"

**Mode-aware:**
- Agency: edit + save flow
- Client: read-only via portal

**Universal components:**
- `<AgentChip>` Narrator on draft
- `<WhyButton>` on the draft to walk to the source data

**Non-negotiables:**
- Edit history immutable — never overwrite
- Final save is irrevocable (can re-draft a new version, not edit a saved final)

---

## Cross-page non-negotiables (locked)

- Every recommendation / finding / anomaly carries a named agent (`<AgentChip>`)
- Numbers always `tabular-nums slashed-zero` + `ss06` — `lib/format.ts` formatters mandatory
- ROAS rendered without `×`. Money ≥ £10k uses k-suffix. Real minus glyph (−).
- en-GB locale (Optimise / Behaviour / Programme)
- No time-of-day framing ("Monday morning brief" banned per memory rule)
- No bubble charts — ratio bars / split bars / sparklines
- Date-range picker is page-template chrome — top-left every page that's time-scoped
- Page max-width: 1440px. 12-col grid. Gutter 16px.
- Universal components mounted as named exports — never redrawn
- Page never ships table-only — carrier element required
- SCALE letters from `lib/scale-pillars.ts` — never hand-typed

---

## Workflow gate

For each page above:

1. Claude Design produces a hi-fi page proposal (HTML or JSX) honouring this contract
2. Claude Code implements on a `feature/page-[name]` branch with preview deploy
3. James eyeballs preview, signs off OR pushes back on specific elements
4. Merge to main only after sign-off
5. Move to the next page

No batch sign-off. No page goes to main without James's nod.

---

*Source-of-truth pairing: `equaliser-ux-principles-v1.md` sets the floor; this doc sets the per-page contract; the C1 v2 design package shows the system.*
