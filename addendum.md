# Equaliser AI — Per-Page Handoff · Addendum (Round 2)

**Status:** v1.1 · responds to Claude Code feedback
**Anchors:** `equaliser-per-page-handoff.md` (v1.0) · `equaliser-ux-principles-v1.md`
**Scope:** 3 missing production pages (A) + 3 system-wide locks (D / E / F)

Read after the v1.0 handoff. Same contract-block shape. Where this doc and v1.0 disagree, this doc wins.

---

# A — Three production pages missing from v1.0

These exist in the production routes but were not in the v1.0 bundle. Adding them now in the same shape.

## 3.0 — Optimise · Recommendations (root)
- **ID:** `/accounts/[client]/optimise` (root) · **Template:** T1 (Action-led) · **Carrier:** Finding card feed
- **One-liner:** "What should I do today, in priority order, with one click to action?"
- **Intent:** This is the morning action page. The user lands here from Dashboard's "Today's actions" link. It must be a single scrollable feed of Findings, sorted by severity then £-impact. No secondary panels, no filters above the fold — the feed is the page.
- **Layout:**
  1. **Header strip** — eyebrow `OPTIMISE · RECOMMENDATIONS` → title "Today's actions" → subtitle ("12 actions queued · £4,300 expected impact") → mode/date/freshness top-right.
  2. **Verdict banner** — Strategist's one-liner anchoring the day (see §F below for shape).
  3. **Filter pills** (collapsed by default) — Severity / Agent / Channel / Lever. One row.
  4. **Finding card feed** — vertically stacked cards, full-bleed in main column. Each card is the full Finding card primitive (see C1 v2 §03.1). Sort: severity desc, then expected £-impact desc.
  5. **Inline action row per card** — `Approve` (primary) · `Send to plan` (secondary, pushes to 3.0a Plan) · `Snooze 24h` (ghost) · `Dismiss` (ghost-rose).
- **Sacred elements (don't lose):**
  - Severity **verb chip** (Fix / Investigate / Review / Scale — see §D).
  - **Author AgentChip** — bottom-left of card, with persona + role.
  - **Soft-toned headline** — sentence-case, plain English, no jargon. *"Search ROAS softened on brand — recommend bid floor at 3.5×"* not *"BID_FLOOR_THRESHOLD_BREACH"*.
  - **£ forecast impact** — top-right of card, monospace, signed (`+£420/wk` or `−£180 risk`).
  - **WhyButton** — opens reasoning popover (source · method · confidence · the maths).
- **Mode behaviour:**
  - **Agency:** full feed, all actions enabled, can edit £ amounts before approving.
  - **Client:** read-only feed of top 5, "Your team is working on this" advisor commentary above the feed, no Approve/Snooze/Dismiss.
- **States:** All 5. Empty (filter): *"No actions match. Reset filters."* Empty (first-run): *"Sentinel is checking your accounts. Recommendations will appear here within 15 minutes of first sync."*
- **Non-negotiables:**
  - One feed. No tabs. No "Recommended / Other / Past".
  - Every card has a verb chip, author AgentChip, £ impact, and WhyButton. No exceptions.
  - Snooze is 24h fixed — not "snooze for X". Decision fatigue kills this page.
  - Dismiss requires a reason chip (Not relevant / Already done / Wrong account). Logged to Activity.

## 3.0a — Optimise · Plan (kanban)
- **ID:** `/accounts/[client]/optimise/plan` · **Template:** T1 · **Carrier:** Kanban velocity
- **One-liner:** "What's queued, in flight, applied — across the team, in one board."
- **Intent:** Monday.com-style work board. The honest place to look at "what is everyone (humans + agents) doing this week". Drag cards between columns. Auto-promotes when an action is approved on the Recommendations feed.
- **Layout:**
  1. **Header strip** — eyebrow `OPTIMISE · PLAN` → title "This week's plan" → subtitle ("18 cards · £8,200 in motion") → mode/date/freshness.
  2. **Velocity strip** — full-width thin chart above the board: cards completed per day, last 14 days. Tiny, contextual, not the hero.
  3. **Five columns** — `Backlog` · `Queued` · `Doing` · `Awaiting approval` · `Applied`. Equal-width on desktop, horizontally scrollable on mobile.
  4. **Card per action** — see Sacred below.
  5. **+ New card** at the foot of Backlog column (Agency only).
- **Sacred elements per card:**
  - **Verb chip** (severity-mapped, top-left).
  - **Owner avatar** (top-right) — human teammate or AgentChip; never blank.
  - **Headline** — same shape as Finding card (sentence-case, plain).
  - **Deadline countdown** — *"due in 2 days"* / *"overdue · 4 days"*. Red when overdue.
  - **£ impact chip** — bottom-left, monospace, signed.
  - **Agent + lever chips** — bottom-right row: which agent owns the work + which lever it pulls (Bid / Budget / Audience / Creative / Structure / Feed).
  - **OutcomeChip** — appears only on cards in the **Applied** column. Shows actual £ outcome vs forecast £ impact ("+£380 actual vs +£420 forecast · 90%").
- **Mode behaviour:**
  - **Agency:** full kanban, can drag, edit, assign, approve.
  - **Client:** view-only, columns collapse to "In flight (5)" + "Applied this week (3)" + "Outcomes" — no kanban visual, just lists.
- **States:** All 5. Empty (first-run): *"No plan yet. Approve a recommendation to add it here."* with link to 3.0.
- **Non-negotiables:**
  - Five columns. Not four, not six.
  - Every card has owner, verb, deadline, £ impact, agent, lever — six fields, fixed.
  - OutcomeChip is auto-populated; user cannot edit (provenance lives in WhyButton).
  - Drag from Awaiting approval → Applied requires Approve action; never silent.

## 4.2 — Equalise · Audit (revised, supersedes v1.0 §4.2 Forecasts in carrier)
> **Note:** v1.0 numbered Forecasts as 4.2. Forecasts moves to **4.3** (renumbering 4.3→4.4, 4.4→4.5, 4.5→4.6 in v1.0). Audit takes 4.2.

- **ID:** `/accounts/[client]/equalise/audit` · **Template:** T2 (Diagnostic) · **Carrier:** **Pillars motif** (the SCALE 5-pillar viz)
- **One-liner:** "Where does this account stand on the SCALE framework, what tier is it, and what's the 90-day path to the next tier?"
- **Intent:** This is the **lead-hook page** — the screenshot Equaliser sells off. It earns a hero gradient (the only page that does). Reads like a Bain audit deck slide: pillar verdict, tier, projected lift if the recommended moves are stacked.
- **Layout:**
  1. **Hero band** — full-width, soft Spark Blue → near-white gradient (legitimate carrier moment, locked to this page only). Inside: eyebrow `EQUALISE · AUDIT`, title (auto: *"Acme · SCALE Audit · Q2 2026"*), tier badge (Bronze / Silver / Gold) at large size, total score (e.g. `64/100`).
  2. **Pillars motif** — five vertical pillars, each labelled S / C / A / L / E. Two bars per pillar: **current** (filled, Spark) + **aspirational** (outlined, dashed). Hover reveals the score and the 1-line verdict.
  3. **Pillar verdict cards** — five cards, one per pillar, in a row. Each: pillar letter + name, current score, status verb chip (see §D), 1-sentence verdict authored by Strategist, primary lever to pull.
  4. **90-day roadmap** — horizontal swimlane (3 lanes: 0–30d / 31–60d / 61–90d). Each move is a chip with: pillar tag · move headline · score-lift estimate (e.g. *"+6 points to S"*) · £-impact estimate.
  5. **"Stack the moves" banner** — bottom of page. *"Stack all 11 moves → projected total: 78/100 · Silver tier · +£42k revenue/quarter."* CTA: `Send to Plan` (pushes all moves to 3.0a Plan).
- **Sacred elements:**
  - **5 pillars × current vs aspirational** — never just current. The aspirational outline is what makes the page motivational.
  - **Tier badge: Bronze (≤50) / Silver (51–75) / Gold (76+)** — fixed thresholds. Display as a small lozenge top-right of hero.
  - **30/60/90-day roadmap** — three lanes, never "Q1/Q2/Q3" or "this month/next month".
  - **Score-lift per move** — every roadmap chip names the lift in points.
  - **Stack-the-moves projected total** — single banner at foot, must show the new tier achievable.
- **Mode behaviour:**
  - **Agency:** full hero, full roadmap, can edit moves.
  - **Client:** full hero, simplified roadmap (top 5 moves), advisor commentary above the pillar cards. No edit. The hero gradient stays — this is the page that earns the moment in both modes.
- **Universals:** Strategist AgentChip on hero. WhyButton on the total score and on each pillar score. AskBar at top.
- **States:** All 5. Loading: *"Strategist is auditing five pillars across 90 days of data…"* (this page is heavy — give it a real progress message). First-run: *"Audit needs 30 days of data. We'll generate yours on day 30."*
- **Non-negotiables:**
  - Pillars motif is the carrier — no other page uses it.
  - Hero gradient lives only here. Do not reuse on Dashboard, Strategy, etc.
  - SCALE is **S**tructure / **C**reative / **A**udience / **L**evers / **E**ffectiveness. Never invent new pillars or rename.
  - Tier thresholds are 50 / 75 — fixed.

---

# D — Severity ramp (system lock)

Confirmed and locked. One mapping, applied across every Finding card, Anomaly chip, Recommendation card, and Activity entry on every page.

| Severity | Verb chip | Colour token | Chip background | When |
|---|---|---|---|---|
| `critical` | **Fix** | `rose` | `rose tint` | Money on fire / outage / mis-spend > £1k/day |
| `high` | **Fix** | `rose` | `neutral` | Material risk, action needed this week |
| `medium` | **Investigate** | `amber` | `amber tint` | Needs attention, look this week |
| `low` | **Review** | `muted` | `neutral` | Watching, no action yet |
| `opportunity` | **Scale** | `emerald` | `emerald tint` | Upside not captured |
| `info` | **Review** | `spark blue` | `spark tint` | Observation, no action |

**Locked. Don't drift.**

- **Verb chip text** is the only label that surfaces in the UI. The severity name (`critical` / `high` / etc.) is metadata, not display.
- **Two severities map to "Fix"** — `critical` and `high`. They differ by chip background only (rose tint = the page-stopper; neutral = "fix soon").
- **Two severities map to "Review"** — `low` and `info`. Same verb, different colour. `info` is positive (spark blue tint, an observation worth noting), `low` is dim (neutral, just watching).
- **Sort order:** `critical > high > opportunity > medium > low > info`. Opportunities outrank medium because upside is treated as actionable.
- **Across pages:** Recommendations feed (3.0), Plan kanban (3.0a), Audit pillar cards (4.2), Activity (5.2), Anomaly callouts on Trends (2.9) — all use this single mapping.
- **Colour tokens** — already in the locked system (`rose / amber / muted / emerald / spark`). Don't introduce new severity colours.

---

# E — First-run / empty-account state (system lock)

The first impression for every pilot. One pattern, repeated on every page that has an empty state due to "no data yet".

## Shape

```
┌────────────────────────────────────────────────────────────┐
│                                                            │
│                       [icon, 48px]                         │
│                                                            │
│              Connect Google Ads to see this.               │
│                                                            │
│      Channel breakdown appears once your first sync        │
│      completes — usually within 15 minutes.                │
│                                                            │
│              [ Connect Google Ads ]   Skip for now         │
│                                                            │
│       — or — see how this looks with sample data —         │
│              [ Load sample data ]                          │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

- **Icon, 48px** — single-line outline icon (`lucide-react` style: `Link`, `Plug`, `BarChart3`, `Sparkles` — context-appropriate). **No illustration.** No mascots, no isometric scenes, no characters. The product is for analysts; illustration looks unserious.
- **Headline (24/32, sentence case)** — names the missing dependency in plain English. *"Connect Google Ads to see this."* / *"No campaigns yet — add one to get started."* / *"Audit unlocks on day 30."*
- **Body (15px, ink-sub, max 2 lines)** — explains what they'll see once unblocked, and how long it takes.
- **Primary CTA** — the action that unblocks. Filled Spark button.
- **Secondary** — `Skip for now` (ghost) — dismisses the page-level prompt; the next visit re-prompts.
- **"Sample data" escape hatch** — a third option, lower in the panel: `Load sample data`. Loads a read-only demo dataset for this page so the pilot can see the shape immediately. Critical for first-run perception. Show on every empty state where sample data exists.
- **Vertical centring** — empty-state card sits in the page's main column, vertically centred in the viewport (not stuck to the top under the chrome). Max-width 480px.
- **No background image, no gradient.** Solid bg. Page chrome stays.

## Variants by reason

The shape is the same. Only the icon, headline, body, and CTA change.

- **No connection (Day 1):** icon `Plug`. *"Connect Google Ads to see this."* CTA `Connect Google Ads`.
- **Connection pending (sync in progress):** icon `Sparkles`. *"Sentinel is syncing your account."* CTA `View progress` (links to settings).
- **No data yet (insufficient days):** icon `Calendar`. *"Audit unlocks on day 30. You're on day 6."* CTA absent — `What's coming` (ghost).
- **Filter zero-result (distinct from no-data):** icon `Filter`. *"No results match. Try fewer filters."* CTA `Reset filters`.
- **No permissions:** icon `Lock`. *"This account isn't shared with you. Ask James for access."* CTA `Request access` (mailto/in-product message).

**One reference design lives in the C1 v2 package** (Dashboard / Channels first-run renders). Every other page inherits this exact pattern — same icon size, same copy length, same CTA stack.

---

# F — Verdict banner (system lock)

The agent-authored one-liner that anchors a page. Used on Dashboard (1.1), Analyse Overview (2.1), Audit (4.2), Equalise Strategy (4.1), Recommendations (3.0).

## Shape

```
[AgentChip] · [verb]: [interpretation, one number]. [optional: next action].
```

**Example:**
> **Sentinel · Watch:** Search ROAS held 4.2 vs 3.8 last week — scale candidate. Recommend +£500/day on Generic.

## Rules

- **Exactly one AgentChip** — leftmost. Persona name + small icon. The agent that authored the verdict.
- **Verb word** — single capitalised verb from a fixed set: `Watch` · `Hold` · `Act` · `Caution` · `Scale` · `Audit`. Pick by severity (Watch = info, Hold = low/medium-no-action, Act = high/critical, Caution = high-uncertainty, Scale = opportunity, Audit = audit-page only).
- **Colon separator** between verb and interpretation.
- **Sentence 1 (interpretation)** — exactly **one number** in the sentence. Never two. The number is the load-bearing fact. ROAS, £, %, count — pick the most consequential.
- **Sentence 2 (optional, action)** — present if there's a clear next move. Starts with a verb (Recommend / Suggest / Hold / Avoid).
- **Length caps:**
  - **Headline (verb + sentence 1):** ≤ 140 characters.
  - **Total (incl. sentence 2):** ≤ 200 characters.
  - **Hard truncate** with ellipsis at the cap; full text in WhyButton.
- **Tone** — conversational, declarative, plain English. Never *"It appears that…"* / *"You may want to consider…"*. Direct.
- **British English** — *programme*, *colour*, *optimise*. Numbers: `£`, comma thousands, `4.2x` (no Unicode multiplier).
- **Locked phrasing for tier statements (Audit only):**
  > *Strategist · Audit:* Account scores **64/100 — Bronze tier**. Stack the recommended 11 moves to reach Silver in 90 days.

## Visual

- One line, full-width of main column. ~16px text, ink colour. AgentChip is its own row item (chip-shaped), verb is bold.
- Subtle 1px Spark Blue left-border on the banner (4px-wide). No background fill. No icon other than the AgentChip.
- Sits **directly under the page title**, above any KPI strip or carrier viz.

## Where it appears

- Dashboard (1.1) — Sentinel-authored.
- Analyse Overview (2.1) — Sentinel-authored.
- Recommendations feed (3.0) — Strategist-authored.
- Audit (4.2) — Strategist-authored.
- Strategy (4.1) — Strategist-authored.

Other pages may use it ad-hoc but these five always have one.

---

## Renumbering (housekeeping)

To accommodate the additions, v1.0 surface 4 (Equalise) renumbers:

| v1.0 | v1.1 |
|---|---|
| 4.1 Strategy | 4.1 Strategy *(unchanged)* |
| 4.2 Forecasts | **4.2 Audit** *(new)* |
|  | **4.3 Forecasts** *(was 4.2)* |
| 4.3 Opportunities | 4.4 Opportunities |
| 4.4 Scenarios | 4.5 Scenarios |
| 4.5 Competition | 4.6 Competition |

Surface 3 (Optimise) gets two new entries at the top:

| v1.0 | v1.1 |
|---|---|
|  | **3.0 Recommendations (root)** *(new)* |
|  | **3.0a Plan (kanban)** *(new)* |
| 3.1 Budgets | 3.1 Budgets *(unchanged)* |
| 3.2 Creative | 3.2 Creative *(unchanged)* |
| 3.3 Automation | 3.3 Automation *(unchanged)* |
| 3.4 Testing | 3.4 Testing *(unchanged)* |

Total page count: **30** (Dashboard 1 + Analyse 10 + Optimise 6 + Equalise 6 + Deliver 7).

---

*End of addendum. Read alongside `equaliser-per-page-handoff.md` v1.0.*
