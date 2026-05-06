# Equaliser AI — Per-Page Design Handoff

**Status:** v1.0 · Claude-Code-ready
**Anchors:** `equaliser-ux-principles-v1.md` · `Equaliser Phase C1 v2 Design Package.html`
**Scope:** 26 pages across 5 surfaces (Dashboard / Analyse / Optimise / Equalise / Deliver)
**Reading order:** Read this end-to-end before writing any page. Each page block is the **contract** — Claude Design owns the layout intent, you own the build.

---

## How to read a page block

Every page below follows the same shape. Treat each field as a hard requirement unless flagged "intent" — those are reasoning notes, not specs.

| Field | What it is | How to use |
|---|---|---|
| **ID / Path** | Route + canonical name (matches `nav-v2.md`) | Use exactly. Don't rename. |
| **One-liner** | The job-to-be-done in one sentence | If your build doesn't make this true, the build is wrong. |
| **Carrier** | The signature visual element this page leans on | One per page. Don't stack carriers — pick the one named. |
| **Template** | T1 (Action-led) / T2 (Diagnostic) / T3 (Operational) | Determines hero shape, density, default sort, action rail position. |
| **Layout** | The grid / region order | Match exactly. Negotiate via Claude Design, not by drift. |
| **Universals** | Components that MUST appear on this page | `<AgentChip>`, `<WhyButton>`, `<OutcomeChip>`, `<DataFreshness>`, `<EntityPanel>` — wire them to live data. |
| **Mode behaviour** | Agency vs Client deltas | Two modes, not two pages. Hide/reveal logic in one component. |
| **States** | Loading / Empty / Error / No-perms / No-data | All five must exist. Default state is the "happy path". |
| **Non-negotiables** | Things that will get a PR rejected | Read these first. |
| **Intent** | Why the page is shaped this way | When the spec is ambiguous, fall back to intent. |

---

## Universal contracts (apply to every page)

These aren't repeated per-page. Read them once, apply everywhere.

### U1 — Page chrome
- **Header:** eyebrow (uppercase, 11px tracking-wide) → page title (24/32) → optional one-line context (15px ink-sub).
- **Top-right cluster:** mode toggle (Agency/Client) · date range · `<DataFreshness>` (last sync, source).
- **AskBar:** persistent at the top of the main column on every Analyse / Optimise / Equalise / Deliver page. Routes to Strategist (the planning-and-recommendations agent). Never floats — anchored.
- **Action rail:** right column on T1 pages, top-of-page chip strip on T2/T3.

### U2 — Carrier elements
Each page picks ONE carrier from this set. Only one. The carrier is the visual element that makes the page recognisable from across the room.
- **Performance line** — large multi-series line/area, ROAS or revenue over time. Used on overview/landing pages.
- **Ratio bar** — horizontal split bar showing channel/campaign mix. Used where composition matters more than trend.
- **SCALE motif** — 5-pillar pillars chart (Structure / Creative / Audience / Levers / Effectiveness). Used on Audit, Audit Roadmap, anywhere referencing the framework.
- **Kanban velocity** — column-based work-in-progress visual. Used on Plan, Workstreams, Testing pipeline.
- **Finding card** — the canonical recommendation/insight unit. Used on Insights, Recommendations, Strategy.
- **Provenance trail** — small inline component showing source → method → confidence. Used on Attribution, anywhere a number could be challenged.

### U3 — Mode toggle
Single toggle in the chrome. Two modes:
- **Agency:** full data density, all metrics, engine vs GA4 split visible, every action available.
- **Client:** top-3 / top-5 lists, advisor commentary, no engine vs GA4 split unless explicitly requested, no destructive actions, no spend-mutation buttons.

Mode is **a render flag, not a route.** Do not build separate Agency and Client routes. The same component reads `mode` and adjusts.

### U4 — States (every page must handle all 5)
1. **Loading** — skeletons matching the real layout. Show the active agent: *"Sentinel is checking auctions…"* / *"Cipher is recomputing attribution…"*. Never a spinner alone.
2. **Empty (first-time)** — explain the upstream dependency. *"Connect Google Ads to see channel breakdown."* Always offer the connect/fix CTA.
3. **Empty (filter)** — distinct from first-time. *"No campaigns match these filters. Reset filters."*
4. **Error** — name what failed and what the user can do. *"Couldn't reach Meta API (timeout). Retry · Skip Meta · Contact support."* Never a generic "Something went wrong".
5. **No permissions** — *"You don't have access to this account. Ask James to add you."*

### U5 — Trust / provenance (mandatory on data)
- Every primary number has a `<WhyButton>` that opens a popover with: source, method, freshness, and the SQL/spec link.
- Every recommendation has an `<AgentChip>` showing which agent produced it (with persona + role).
- Charts show a `<DataFreshness>` chip (e.g. "Updated 14:38 · 7 min ago").

### U6 — Workflow gate
Some pages are gated behind workflow state (e.g. Activity needs at least one action taken; Reports needs a report scheduled). Where gated, show a **first-run state** that explains the prerequisite and offers the action that unblocks it.

### U7 — Non-negotiables (apply globally)
- **British English** in all UI copy. *Optimise* not *Optimize*. *Programme*, *colour*, *organise*. Numbers: £, comma thousands, en-dashes for ranges.
- **Numbers:** monospace digits (`font-variant-numeric: tabular-nums`). ROAS as `3.2x` not `3.2×` (no Unicode multiplier — keep it ASCII for copy/paste).
- **No emoji in product UI.** Ever.
- **No bubble charts.** Ratio or split bars.
- **No pie charts** unless ≤4 slices and only on a Client view summary.
- **Charts:** locked palette (Spark / Lavender / Stat / muted); no gradient fills; one accent per chart.

---

# Surface 1 — Dashboard

## 1.1 — Dashboard
- **ID:** `/`  ·  **Template:** T1 (Action-led)  ·  **Carrier:** Performance line
- **One-liner:** "What changed since I last logged in, and what should I do today?"
- **Intent:** This is the morning page. The user lands here once per day. It should answer *"is anything on fire?"* in 3 seconds and *"what's worth my time today?"* in 30.
- **Layout:**
  1. Performance line (ROAS, last 14 days, multi-account if Agency).
  2. KPI strip (Spend / Revenue / ROAS / CPA / Conv) — tabular nums, deltas vs previous period.
  3. **Today's actions** — top 3 Findings as Finding cards, with `<WhyButton>` and Approve/Skip.
  4. Activity strip — 5 most recent agent actions (Sentinel, Cipher, Strategist, etc.) with timestamps.
- **Mode:** Agency shows multi-account roll-up + per-account drill. Client shows their account only + advisor commentary above the KPI strip.
- **Universals:** AskBar, DataFreshness, AgentChip on every action, WhyButton on every KPI.
- **States:** All 5. First-time empty: *"Connect your first ad account."*
- **Non-negotiables:** No "Welcome back, James" greeting. No motivational copy. Show data.

---

# Surface 2 — Analyse

Analyse is the **diagnostic** surface. T2 template throughout. Density-first.

## 2.1 — Overview
- **ID:** `/analyse`  ·  **Template:** T2 (Diagnostic)  ·  **Carrier:** Performance line
- **One-liner:** "How is paid media doing right now, across everything?"
- **Layout:** Performance line full-width → KPI strip → channel ratio bar → "What's interesting" findings list (5 items, finding cards).
- **Mode:** Client view collapses to: line + KPI + 3 finding cards with advisor commentary.
- **Universals:** AskBar at top. WhyButton on every metric. AgentChip on findings.
- **Non-neg:** Engine vs GA4 toggle on KPI strip (Agency only). No bubble charts.

## 2.2 — Channels
- **ID:** `/analyse/channels`  ·  **Template:** T2  ·  **Carrier:** Ratio bar
- **One-liner:** "Which channels are scaling, holding, or leaking — and what's the recommendation?"
- **Layout:** Ratio bar (channel mix by spend) → channel table (rows = channels, columns = spend / rev / ROAS / CPA / conv / share / Δ CPA / Δ ROAS / status / commentary).
- **Mode:** Agency = full table + commentary chip. Client = top 3 rows + advisor commentary, no engine vs GA4 split.
- **Status column:** `Scale` (green) / `Maintain` (neutral) / `Investigate` (amber) / `Reduce` (red).
- **Non-neg:** No bubble charts. Channel names canonical: Google Search / Google Shopping / Meta Advantage+ / TikTok / Pinterest / Microsoft / YouTube. Engine vs GA4 stays separate.

## 2.3 — Campaigns
- **ID:** `/analyse/campaigns`  ·  **Template:** T2  ·  **Carrier:** Ratio bar
- **One-liner:** "Which campaigns are working, which aren't, and which need a decision?"
- **Layout:** Filter strip (channel / brand split / objective) → campaign table (rows = campaigns, sortable columns). Default sort: spend desc.
- **Mode:** Client = winners + losers only (top 5 / bottom 3), no Agency-only objectives.
- **Non-neg:** Pagination at 50 rows, never load 1000+ on initial render. Saved views per user.

## 2.4 — Creatives
- **ID:** `/analyse/creatives`  ·  **Template:** T2  ·  **Carrier:** Finding card (creative-fatigue findings)
- **One-liner:** "Which creatives are tired, which are scaling, what's next to test?"
- **Layout:** Creative grid (thumbnail · CTR · CVR · spend · status) → fatigue findings panel (Wordsmith agent).
- **Mode:** Client = top performers + 3 fatigue findings, no asset library access.
- **Non-neg:** Thumbnails real, never grey placeholders in production. Fatigue threshold defended in the WhyButton.

## 2.5 — Audiences
- **ID:** `/analyse/audiences`  ·  **Template:** T2  ·  **Carrier:** Ratio bar (segment mix)
- **One-liner:** "Who are we reaching, and which audiences are working?"
- **Layout:** Audience table (segment · reach · CPA · CVR · share · status) + overlap chip when audiences overlap >20%.
- **Mode:** Client = top 3 segments only, no audience-builder tools.

## 2.6 — Products
- **ID:** `/analyse/products`  ·  **Template:** T2  ·  **Carrier:** Ratio bar (revenue mix by product)
- **One-liner:** "Which products carry revenue, which underperform, what's the feed health?"
- **Layout:** Product table (SKU · revenue · ROAS · feed status · margin if connected) + feed health chip strip.
- **Mode:** Client = top 10 products + bottom 3 + feed health summary.
- **Non-neg:** Feed-health surfaced before performance. Feed errors are blocking — show them.

## 2.7 — Keywords
- **ID:** `/analyse/keywords`  ·  **Template:** T2  ·  **Carrier:** Ratio bar (brand vs generic)
- **One-liner:** "Which keywords drive value, which leak, what's the search-term coverage?"
- **Layout:** Brand/Generic split bar → keyword table → search-terms panel (Lookout agent recommendations: add as keyword / add as negative).
- **Mode:** Client = brand vs generic summary + top 10 keywords + 5 negative recommendations.

## 2.8 — Attribution
- **ID:** `/analyse/attribution`  ·  **Template:** T2  ·  **Carrier:** Provenance trail
- **One-liner:** "Where did the conversions actually come from, by which model?"
- **Layout:** Model selector (Last-click / Position-based / Data-driven / MMM if available) → channel attribution stack chart → comparison table.
- **Mode:** Client = data-driven model only, with explainer copy from Cipher.
- **Universals:** Cipher's `<AgentChip>` everywhere. WhyButton on each model.
- **Non-neg:** Never show conflicting model values without explanation. The provenance trail is mandatory: source · method · confidence.

## 2.9 — Trends
- **ID:** `/analyse/trends`  ·  **Template:** T2  ·  **Carrier:** Performance line (multi-series)
- **One-liner:** "What's moving over time, and what's the trajectory?"
- **Layout:** Time-range pills (7/30/90/YTD/Custom) → multi-series line (toggleable: spend, rev, ROAS, CPA, conv) → anomaly callouts.
- **Mode:** Client = simplified 30/90 only, anomaly callouts written by Strategist.

## 2.10 — Website
- **ID:** `/analyse/website`  ·  **Template:** T2  ·  **Carrier:** Funnel (ratio bars stacked)
- **One-liner:** "How does paid traffic convert on-site, where do users drop off?"
- **Layout:** Funnel chart (visits → product views → add-to-cart → purchase) → page performance table → device split.
- **Mode:** Client = funnel + 3 priority pages.
- **Non-neg:** Funnel labels match GA4 events 1:1, never invented.

---

# Surface 3 — Optimise

Optimise is the **action** surface. T1 template throughout. Decisions, not analysis.

## 3.1 — Budgets
- **ID:** `/optimise/budgets`  ·  **Template:** T1  ·  **Carrier:** Finding card
- **One-liner:** "Where should I move money this week — and is the system already doing it?"
- **Layout:**
  1. Budget situation strip (Total spend / Pacing / Budget remaining / Days left).
  2. Recommended moves (3–5 Finding cards: from → to, £ amount, expected ROAS impact, agent reasoning).
  3. Auto-pilot status — what Pace agent is doing autonomously, with toggle.
- **Mode:** Agency = approve/skip per move, can edit amount. Client = "Pace is shifting £X this week" summary, no controls.
- **Universals:** Pace's AgentChip on every move. WhyButton showing the maths.
- **Non-neg:** No "set & forget" without a guardrail. Show the cap. Show the abort condition.

## 3.2 — Creative
- **ID:** `/optimise/creative`  ·  **Template:** T1  ·  **Carrier:** Finding card
- **One-liner:** "Which creatives need refresh, what should we test next?"
- **Layout:** Fatigue findings list → creative test queue → ad-copy generator (Wordsmith agent surface).
- **Mode:** Agency = full queue, can promote to test. Client = "next 3 things we're testing" + advisor commentary.

## 3.3 — Automation
- **ID:** `/optimise/automation`  ·  **Template:** T1  ·  **Carrier:** Kanban velocity
- **One-liner:** "What is the system doing on its own, what's queued, what's awaiting approval?"
- **Layout:** Kanban (Doing / Queued / Awaiting approval / Done last 7d). Cards = automated actions. Each card has agent · action · target · expected outcome.
- **Mode:** Agency = full kanban with controls. Client = "in flight" summary only.
- **Universals:** Every card has an AgentChip. Every card has a WhyButton.
- **Non-neg:** No silent automation. If an agent moves a card to "Done", it must show what it did and why.

## 3.4 — Testing
- **ID:** `/optimise/testing`  ·  **Template:** T1  ·  **Carrier:** Kanban velocity
- **One-liner:** "What experiments are running, what's winning, what's next?"
- **Layout:** Kanban (Backlog / Running / Reading / Decided). Each experiment card shows hypothesis · variants · current readout · status (Winning / Inconclusive / Losing).
- **Mode:** Agency = full pipeline + can launch new test. Client = running tests + last 3 decisions.
- **Non-neg:** Never declare a winner without the confidence interval. "Inconclusive" is a valid outcome — surface it.

---

# Surface 4 — Equalise

Equalise is the **strategic** surface. T1 template, longer-horizon framing.

## 4.1 — Strategy
- **ID:** `/equalise/strategy`  ·  **Template:** T1  ·  **Carrier:** Finding card
- **One-liner:** "What's the plan for the next 90 days, what's the case for it, and what's the trade-off?"
- **Layout:** Strategist's brief (eyebrow → headline → 3 bullets) → 3 Finding cards as the "moves" → trade-off panel ("if we do X, we accept Y") → next-actions list.
- **Mode:** Agency = editable, can approve into Plan. Client = read-only narrative + advisor commentary.
- **Universals:** Strategist's AgentChip prominently. AskBar wired to extend the conversation.
- **Non-neg:** Trade-offs are mandatory. Never present a strategy without naming what it costs.

## 4.2 — Forecasts
- **ID:** `/equalise/forecasts`  ·  **Template:** T2  ·  **Carrier:** Performance line (with cone of uncertainty)
- **One-liner:** "If we keep going, what does the next quarter look like — at low / mid / high effort?"
- **Layout:** Forecast line with cone (P10 / P50 / P90) → effort dial (Low / Mid / High) → expected outcomes table by effort level.
- **Mode:** Agency = adjust assumptions. Client = mid-effort forecast + commentary.
- **Non-neg:** Always show the cone. Never a single-line "we'll do £X" forecast.

## 4.3 — Opportunities
- **ID:** `/equalise/opportunities`  ·  **Template:** T2  ·  **Carrier:** Finding card (size-coded)
- **One-liner:** "Where's the upside we're not capturing yet?"
- **Layout:** Opportunity grid (cards sized by expected impact) → effort × impact 2x2 → "queue this" CTAs that push to Optimise.
- **Mode:** Client = top 3 + plain-English explainer.

## 4.4 — Scenarios
- **ID:** `/equalise/scenarios`  ·  **Template:** T2  ·  **Carrier:** Performance line (multi-line)
- **One-liner:** "What happens if we cut Meta by 30%? What happens if we double Search?"
- **Layout:** Scenario builder (sliders per channel) → projected outcome lines (baseline vs scenario) → confidence + caveats.
- **Mode:** Agency only. Client view: locked, with "ask your team for a scenario".
- **Non-neg:** Always show the baseline. Never a scenario alone.

## 4.5 — Competition
- **ID:** `/equalise/competition`  ·  **Template:** T2  ·  **Carrier:** Ratio bar (share of voice)
- **One-liner:** "What's the competitive picture — share, presence, gaps?"
- **Layout:** Share-of-voice ratio bar → impression-share table → gap findings (Lookout agent).
- **Mode:** Client = SOV + 3 gap findings.
- **Non-neg:** Source attribution mandatory (auction insights / Pathmatics / SEMrush). No invented data.

---

# Surface 5 — Deliver

Deliver is the **outputs** surface. T3 template (operational lists, repetitive structure).

## 5.0 — Deliver root
- **ID:** `/deliver`  ·  **Template:** T3  ·  **Carrier:** Finding card (recent outputs)
- **One-liner:** "What's been produced for this client recently, and what's coming next?"
- **Layout:** Recent outputs list (reports / decks / activity) → upcoming schedule → quick-create buttons.

## 5.1 — Reports
- **ID:** `/deliver/reports`  ·  **Template:** T3  ·  **Carrier:** Operational list
- **One-liner:** "What reports are scheduled, when do they go out, what's in them?"
- **Layout:** Report list (frequency / next send / recipients / template) → preview panel → schedule editor.
- **Mode:** Agency = full editor. Client = view-only of their own reports.
- **Non-neg:** Reports are previewed in their final rendered form before sending. Never a JSON-y "configuration".

## 5.2 — Activity
- **ID:** `/deliver/activity`  ·  **Template:** T3  ·  **Carrier:** Timeline
- **One-liner:** "What has the system done over time, with full audit trail?"
- **Layout:** Reverse-chronological timeline (action / agent / target / outcome / artefact link). Filterable by agent, surface, date.
- **Universals:** AgentChip on every entry. Each entry expandable to show the WhyButton-equivalent reasoning.
- **Non-neg:** Immutable. Activity log is never edited or deleted; corrections are appended.

## 5.3 — Meetings
- **ID:** `/deliver/meetings`  ·  **Template:** T3  ·  **Carrier:** Operational list
- **One-liner:** "What's on for the next client meeting, what was said last time, what was promised?"
- **Layout:** Upcoming meetings list → for each: agenda + last-meeting recap + open commitments → "build deck for this" CTA.

## 5.4 — Strategy decks
- **ID:** `/deliver/decks`  ·  **Template:** T3  ·  **Carrier:** Operational list
- **One-liner:** "What decks have we built, what's in them, what gets reused?"
- **Layout:** Deck library (thumbnail / title / last-edited / client) → "new from template" → preview.
- **Non-neg:** Decks generated from real data; templates are slide-shapes, not content.

## 5.5 — Templates
- **ID:** `/deliver/templates`  ·  **Template:** T3  ·  **Carrier:** Operational list
- **One-liner:** "Which output templates are in play, what do they produce, who owns them?"
- **Layout:** Template list (kind / owner / last-used / linked outputs) → editor.
- **Mode:** Agency only.

## 5.6 — Commentary
- **ID:** `/deliver/commentary`  ·  **Template:** T3  ·  **Carrier:** Operational list
- **One-liner:** "What advisor commentary is queued, drafted, sent?"
- **Layout:** Commentary queue → draft editor (Wordsmith agent assist) → sent log.
- **Mode:** Agency only.

---

## Build order recommendation

Don't build alphabetically. Build in this order so the system stress-tests itself:

1. **Dashboard 1.1** — proves the overall chrome + carrier model.
2. **Analyse 2.2 Channels** — densest table, proves the data primitives.
3. **Optimise 3.1 Budgets** — proves T1 (action-led) template + Finding card + agent approval flow.
4. **Equalise 4.1 Strategy** — proves the long-form/strategist surface.
5. **Deliver 5.2 Activity** — proves the audit trail / timeline.

Once those five hold, the rest follow the same primitives.

## Don't ship until

- All five states render on every page (Loading / Empty / Error / No-perms / No-data).
- Mode toggle works on every page (Agency ↔ Client) without route changes.
- Every primary number has a WhyButton.
- Every recommendation has an AgentChip.
- British English throughout. Search the codebase for *Optimize*, *Color*, *Organize* and fix.
- Tabular nums on every metric.
- No bubble charts. No gradient fills. No emoji.
- AskBar persistent on every Analyse / Optimise / Equalise / Deliver page.

## When in doubt

Read the **Intent** field on the page block. If still ambiguous, fall back to `equaliser-ux-principles-v1.md`. If still ambiguous, ask Claude Design before guessing.

---

*End of contract. Visual reference package: `Equaliser Per-Page Design Package.html`.*
