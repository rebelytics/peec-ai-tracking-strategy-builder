# Phase A — Intake (§3.8–3.10, §8)

Part of the **peec-ai-tracking-strategy-builder** skill (CC BY 4.0 — Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)). Section numbers are global across `SKILL.md` and `references/` — the section map in `SKILL.md` says where each § lives.

**Load trigger:** Read this file in full at the start of every Intake step, before asking the user for anything. §3.8–3.10 lead the file because intake shortcuts are the strongest failure mode this skill has seen.

**Contents:**

- 3.8 Intake shortcuts are the strongest failure mode this skill has seen
- 3.9 Recovery from intake-step skip is a user-ask, not a substitution
- 3.10 "Ring" numbering names a path, not a fixed step order
- 8.1 Ring 1 — Automated via connected tools and skills
  - Known MCP gaps — check before asking the user
  - Peec MCP (always available when this skill runs)
  - Loaded skills
  - Conversation context
  - Optional connected MCPs (skip if not connected)
- 8.2 Ring 2 — Baseline path (always works, no external dependencies)
  - Step A: Peec seed-and-harvest (primary day-0 signal)
  - Step B: Competitor FAQ scraping
  - Step C: AI self-report (coverage check)
  - Step D: YouTube autocomplete + top video titles
  - Step E: Reddit / community signals (only via connected tooling)
  - Step F: XML sitemap baseline (see §8.3.2)
  - Step G: User's domain knowledge (batched ask — see Ring 3)
- 8.3 Ring 3 — User-provided data (batched ask at end of intake)
  - 8.3.1 Automated inventory (before composing the ask)
  - 8.3.2 Automated URL-structure baseline (sitemap-first)
  - 8.3.3a Targeted data request (attachment-centric, plain text)
  - 8.3.3b Scoping widget (AskUserQuestion)
  - 8.3.3c Open-ended data-source invitation
  - Ring 3 handling
- 8.4 Intake summary output (mandatory gate for §9 Strategy)

---

### 3.8 Intake shortcuts are the strongest failure mode this skill has seen

If the agent feels tempted to proceed to Strategy with "enough" from
Ring 1, treat that feeling as a prompt to re-read §4.1 and §4.2. The
skill's core claim — prompts earn their slots from data, not intuition —
is defeated the moment Intake is cut short. There is no "good enough
Ring 1" that justifies skipping Ring 3.

Observed failure mode: after completing Ring 1 (Peec MCP +
brand-context skill + web searches), the agent jumped directly to
Strategy with 70
prompts and a full allocation table based on commercial intuition — zero
external data, zero baseline seed-and-harvest, zero Ring 3 user ask.
The rule existed three times over (§6, §8.3, §15.1 checklist). All three
layers failed under context pressure. The fix is structural: §8.4 and §9
now enforce a visible intake summary gate (see those sections). This
principle explains *why* the gate exists.

**Recurrence after structural fix.** The same failure mode recurred even
after §8.3 was restructured into named paths (§8.3.1/§8.3.2/§8.3.3).
When discovered mid-run, recovery is not optional. Three named shapes
govern what the failure looks like; §3.9 governs how to recover.

**The three named skip shapes:**

1. **Outright omission** — the intake step was simply not run, either
   by not asking at all or by asking with partial coverage of the
   required fields.
2. **Substitution on recovery** — the skipped step was noticed, but the
   agent filled the gap with web search / commercial intuition / another
   Ring's data rather than running the step it was meant to.
3. **Planned deferral to Loop 2** — the skipped step is **documented as
   a deliberate design choice**: "we'll layer external data in during
   Loop 2". This is the most dangerous shape because it passes every
   existing gate by accepting "documented" as equivalent to
   "dispositioned". The agent names the gap, justifies the deferral with
   workflow-aware language ("seed-and-harvest first; enrich later"),
   writes it into the strategy, and ticks §15.1's "external data
   acknowledged, not hidden" box — and none of that is the user actually
   sharing data.

All three are failures of the same rule: Intake is a prerequisite for
Strategy, not a parallel track. "We'll cover it later" is not a Strategy
input — it's deferred intake, and Strategy has to wait. If external data
genuinely isn't available for Loop 1, the user must **explicitly decline
in writing** and take ownership of the gap; the agent cannot self-grant
the deferral.

### 3.9 Recovery from intake-step skip is a user-ask, not a substitution

When a §8.3 intake step is discovered to have been skipped, the recovery
action is to **ask the user for the data the skipped step was meant to
elicit** — not to substitute web search, not to swap in a baseline-path
seed-and-harvest, not to claim the external data was covered elsewhere.
Substitutions move the skip from "obvious" to "hidden"; the user has no
way to see that Ring 3 was never completed. The structural enforcement
at §8.4 depends on the user-ask step producing real user input, not
synthetic substitutes.

**What recovery looks like:**

1. Pause whatever phase discovered the skip.
2. Name the skip in-session: "I see Ring 3 external data intake was
   skipped — I need to run that now before the Strategy can be trusted."
3. Run the user-ask in the mode the skipped step specifies (batched
   AskUserQuestion for Ring 3 §8.3.3; attachment request for §8.3.1 data
   dump paths).
4. Only after the user has actually provided the data (or explicitly
   declined and taken ownership of the gap in writing) may the skill
   proceed.

Recovery via web search, baseline activities, or "I'll just note the
assumption" is a repeat of the original failure with a new coat of paint.

### 3.10 "Ring" numbering names a path, not a fixed step order

Ring 1 / Ring 2 / Ring 3 in §8 name **data-source paths**, not a linear
step sequence. Ring 1 = automated via connected tools, Ring 2 = baseline
seed-and-harvest, Ring 3 = batched user ask. They run in parallel or in
whatever order the project state demands — there is no "Ring 2 comes
after Ring 1". Skills referencing the rings should phrase them as paths
("the Ring 3 user-ask path", "the Ring 1 automation path"), never as
ordinals that imply completion of N before starting N+1.

The numbering is kept for continuity with existing references, not
because it encodes a sequence. When planning intake, pick the paths the
project state activates and run them concurrently where the tools allow.

---
## 8. Phase A — Intake

Goal: populate the intake state and the project-state snapshot for this
loop.

**First loop** runs the full three-ring intake below: Ring 1 first,
Ring 2 as the floor, Ring 3 as the only user-facing ask.

**Subsequent loops** typically run a Peec-only refresh — re-reading
`list_brands`, `list_topics`, `list_tags`, `list_prompts`, and the
current-state summary — unless a finding from the previous Analyse (§13)
surfaces a gap that external data could close. In that case, re-enter
Ring 2 or Ring 3 for that specific gap only. The skill does not require
re-running the full three-ring intake on every loop.

Pacing disclosure (see §6): at the first Intake, the agent tells the user
that the default iteration pace is daily (Peec processes new prompts
within 24 hours), that longer 7–14 day windows are available for
trend-level analyses, that the number of loops is driven by findings
rather than a preset timeline, and that once the strategy stabilises the
agent can produce a stakeholder-facing presentation (Phase B, §14).

### 8.1 Ring 1 — Automated via connected tools and skills

Before touching the user, check each of these. Skip any that isn't
available; don't ask the user whether they're available — inspect the
environment.

#### Known MCP gaps — check before asking the user

Before asking the user for any information, the agent must attempt to
derive it from the Peec MCP. Several fields that seem like they should
be available are not. The table below enumerates the known gaps so the
agent checks, confirms the gap, and frames the user ask as "the MCP
doesn't expose this" rather than "tell me":

| Field | MCP tool to check | What it returns | Gap |
|---|---|---|---|
| **Project country / market** | `list_projects` | `{id, name, status}` only | No country, language, or market metadata at project level. Ask the user: "Peec's MCP doesn't expose project-level market data, so I need to confirm: which markets should this strategy prioritise?" |
| **Plan tier / prompt credits** | `list_projects` | `{id, name, status}` only | No plan data. `get_credit_balance` does not exist (see `peec-ai-mcp` §7.37). Ask the user: "Peec's MCP doesn't expose plan credits. How many prompts does your plan allow?" Single precise question, not a multi-tier multiple choice. |
| **Active engine count (proxy for plan gating)** | `list_models(is_active=true)` | Active engine list | ✓ Available. 3 or fewer = likely plan-capped (§9.6). |

When asking the user about a known MCP gap, always prefix with the
reason: "The Peec MCP doesn't expose this at the project level, so I
need to confirm with you:" — this reframes the ask from "I didn't check"
to "the platform doesn't expose this" and teaches the user where the gap
lives. Persist every answer to the intake state (§7) so subsequent loops
don't re-ask. Surface plan-related questions (prompt credits + engine
count) together so the user answers all plan constraints in one go.

#### Peec MCP (always available when this skill runs)

Capture:

- `list_projects` — confirm the project ID to operate on.
- `list_brands(project_id)` — own brand flag, domains, aliases, regex,
  current competitor roster. Note per-brand missing config
  (aliases: null, regex: null, domain coverage gaps).
- `list_topics(project_id)` — current topic structure and count.
- `list_tags(project_id)` — current tag taxonomy, count, dimension
  coherence (look for overlap like `transactional` vs `funnel:decision`).
- `list_prompts(project_id, limit=10000)` — full prompt inventory with
  `text, tag_ids, topic_id`. Pagination matters — see `peec-ai-mcp`
  §7.18 (default is 100 per page, silent truncation if missed).
- `list_models(project_id, is_active=true)` — active engines. Count
  gates the model-coverage branch (§9.6): 3 or fewer ⇒ likely plan-capped.
- If the project has existing chats, pull a small recent window
  (`list_chats` + `list_search_queries`) to seed fanout mining in Ring 2.

Resulting project-state classification:

- **New project:** zero prompts, auto-suggested brands still present.
  The Write sub-phase (§12) will be pure creates.
- **Existing project:** prompts already in place. The Write sub-phase
  (§12) will use the disposition framework (§10).

#### Loaded skills

Scan the active skill list. Specifically check for:

- **Brand-context skills** — any skill that holds accumulated knowledge
  about the brand being tracked (brand dossier, project-context skill,
  or similar — however it's named in the agent's environment). Captures
  markets, TLDs, revenue profile, regulatory notes, sister-brand
  relationships.
- **Portfolio / group-context skills** — where the brand is part of a
  larger group, the agent may have a shared-context skill covering
  sister brands, shared infrastructure, and group-level strategic themes.
- **Business-strategy / commercial-context skills** — any skill capturing
  the commercial priorities, capacity, or revenue goals that inform
  allocation decisions.

If a brand-context skill exists, read it first and populate intake state
from it before asking the user anything.

#### Conversation context

Scan the current session transcript for already-provided context —
brand name, markets, strategic priorities, known competitors.
Populate intake state from this before re-asking.

#### Optional connected MCPs (skip if not connected)

- **GSC MCP** — pull top queries over 12 months per market via the API
  (not subject to the ~1000-row UI export cap; pull several thousand if
  useful). Filter for question-shaped queries
  (`how|what|why|is|does|should`) and commercial intent. Feeds the
  volume-signal and allocation steps in the Strategy sub-phase (§9).
- **SEO-tool MCPs** — CPC × SV for commercial weighting, SERP competitors
  for the brand's keyword universe, AI Overview presence flags.
- **Analytics MCPs (GA4, Adobe, Matomo, Piwik Pro, or equivalent)** —
  landing-page sessions, key events, revenue. The **strongest commercial
  signal** for e-commerce — prioritise over keyword-demand signals where
  both are available (see §8.2).
- **Commerce-platform MCPs** — revenue by category, product catalogue.
- **Crawl-tool MCPs (Screaming Frog, Sitebulb, or equivalent)** — content
  inventory, page type distribution, structural content gaps.
- **Community/forum access (MCP or agentic browser extension)** —
  community question-mining. If any such tool is connected, use it;
  Reddit in particular is not directly accessible via the baseline web
  tools (see Ring 2 notes).

### 8.2 Ring 2 — Baseline path (always works, no external dependencies)

When Ring 1 returns a sparse picture, the baseline path produces enough
signal to build a competent strategy. Every agent running this skill
can execute all six steps — no MCPs beyond Peec, no user-specific tool
access.

#### Step A: Peec seed-and-harvest (primary day-0 signal)

This is the load-bearing step for new projects and the richest
free-standing demand signal available.

1. **Day 0:** create 10–15 deliberately broad discovery prompts across
   the brand's rough topic clusters. Prompt shapes should be generic:
   "what is X", "best X for Y", "X vs Y", "recommendations for X",
   "how to choose X". Tag them `seed:harvest` and set `country_code`
   from the primary market. These prompts join the 24-hour cycle
   immediately (see `peec-ai-mcp` §7.6).
2. **Day 1 (~24 hours later):** for each seed chat, call
   `list_search_queries(chat_id=...)`. This returns the sub-queries the
   AI engine fanned out to when answering. **This is the highest-fidelity
   demand signal available without paid tools** — it's what the AI
   itself considers the adjacent questions in the category.
3. Also call `list_shopping_queries(chat_id=...)` for any shopping-mode
   chats — commercial-intent queries, often product-specific.
4. Mine the fanout for platform patterns: `site:reddit.com`,
   `site:amazon.*`, `site:youtube.com`, `site:quora.com`. These are the
   platforms the AI believes hold answers in the vertical. High Reddit
   fanout with no brand presence on Reddit is a distribution signal
   (see §11 pattern library).
5. **Seed prompt lifecycle:** at Strategy time (§9), decide per
   seed prompt — keep it if it fits the final strategy and the topic
   survives, delete it if not. Seed prompts are not automatically
   preserved.

For existing projects, the seed-and-harvest step is optional — fanout
data can be mined from existing chats instead. Use seed-and-harvest
when the existing prompt set doesn't cover a category the strategy
needs to explore (e.g., the brand is entering a new vertical).

#### Step B: Competitor FAQ scraping

For each of the brand's top 3–5 known commercial competitors:

1. `WebSearch("[competitor name] FAQ")` — finds the competitor's FAQ
   URL.
2. `WebFetch(<faq url>)` — extracts the question headings.
3. Supplement with `WebSearch("[competitor name] blog")` for top
   listicles; `WebFetch` the most prominent for H2/H3 question
   structures.

Competitor FAQs are the most direct supply-side signal of "we get
asked this a lot". Limits: corporate domains are nearly always
accessible via WebFetch; community platforms (Reddit, Quora) are often
blocked — see Step E below.

#### Step C: AI self-report (coverage check)

The agent uses its own reasoning inline during the session. Prompt:

> *"For a [brand type] operating in [markets], list 30 question-shaped
> queries someone might ask an AI model before buying in this
> category. Include a mix of awareness, consideration, comparison,
> and purchase-intent queries. Flag any queries that touch regulated
> or grey-area topics."*

Generated inline — no external model call, no artifact, just reasoning.
Used as a coverage check: after baseline-path outputs are aggregated,
compare against this list to spot blind spots.

This signal is weak relative to fanout data and competitor FAQs, but
it's free and instant. Treat it as a gap-check, not a seed source.

#### Step D: YouTube autocomplete + top video titles

For the brand's top 2–3 categories:

1. `WebSearch("[category] YouTube reviews")` — surfaces top-ranked
   videos whose titles are question-shaped.
2. Optionally `WebFetch(<youtube url>)` on the top 3 results — some
   video pages return transcripts / descriptions that reveal the
   question space.

Video is a major AI citation source (ChatGPT and AI Overview cite
YouTube heavily in product categories). Video titles are a secondary
demand signal.

#### Step E: Reddit / community signals (only via connected tooling)

**Reddit is not directly accessible via the baseline WebFetch or
WebSearch tools.** As of this skill's current release, WebFetch
refuses `reddit.com` and `old.reddit.com` at the tool level;
WebSearch doesn't support `site:` operators and doesn't surface
Reddit organically for typical queries. Re-test if a later agent /
tooling generation unblocks Reddit access.

So Reddit stays out of the baseline path. Three fallbacks:

1. **Connected Reddit tooling, if present.** If a Reddit MCP is
   connected, use it. If the agent has an agentic browser extension
   or equivalent that can browse Reddit directly, use that. Mine the
   2–3 relevant subreddits for top-year threads.
2. **Peec fanout proxy.** If the seed-and-harvest (Step A) shows
   `site:reddit.com` in the fanout queries, that's the Reddit-demand
   signal — delivered through Peec without needing direct Reddit
   access.
3. **User-provided.** If the user has scraped or exported Reddit data
   (rare), consume it via Ring 3.

Quora and Stack Exchange are usually accessible via WebFetch — try
them for technical/niche verticals, but don't rely on them for general
e-commerce.

#### Step F: XML sitemap baseline (see §8.3.2)

URL-structure baselining from the XML sitemap is a Ring-2 automated
step but is documented once, in §8.3.2, because it precedes the Ring 3
user ask directly. Run it before composing Ring 3.

#### Step G: User's domain knowledge (batched ask — see Ring 3)

The one user-facing ask happens at the end of intake, not throughout.
See Ring 3 for the question set.

### 8.3 Ring 3 — User-provided data (batched ask at end of intake)

**On the first loop, Ring 3 is fired regardless of Ring 1 richness.**
"The user has no data" is a valid answer; "the agent decided Ring 1 was
enough" is not. See §3.8 for why this is a hard rule.

Ring 3 runs in four sub-steps. The sequence enforces §4.3 ("automated
sources first, user asks last") structurally rather than by prose alone.

**Two structurally different asks — don't collapse into one widget.**
The Ring 3 user-facing ask has two functionally distinct parts that
must not be merged into a single AskUserQuestion call:

- **The data request** (§8.3.3a) — "please attach these files /
  confirm each data category is unavailable". Attachment-centric,
  plain-text, cannot be compressed into a multiple-choice widget.
  The user needs to see the full list of what they could provide and
  respond item-by-item.
- **The scoping widget** (§8.3.3b) — "how many prompts? which
  markets? which brand priorities?". Binary or short-choice questions
  well-suited to AskUserQuestion, with explicit override slots per
  §4.5.

Collapsing these into one widget is the known failure mode: the
widget surface can represent scoping choices but not attachment
requests, so the data request is silently dropped. The split is
load-bearing; keep it explicit.

#### 8.3.1 Automated inventory (before composing the ask)

Before composing the Ring 3 question, the agent must enumerate what data
is already accessible for this brand. Check each of:

- **Brand-context skills** — read each loaded brand-context skill for
  referenced files, existing tool access, data-layer docs, crawl
  configs, taxonomy documents.
- **Tool-reference skills** — check each tool-specific skill for
  credentials, project IDs, and access patterns that bypass a user
  export (e.g. a search-console MCP reference, an AI-visibility-platform
  API reference, an analytics MCP reference).
- **Workspace folders** — scan the brand-specific folder for CSVs,
  JSONs, MDs, XLSXs that predate this session.
- **Connected MCPs** — list every connected MCP tool and note which
  could supply Ring 3 data (analytics, search console, rank tracking,
  CRM, community/forum access).

Output: an "already accessible" block in the Intake summary (§8.4),
with file paths and tool names. This block feeds the targeted ask below.

#### 8.3.2 Automated URL-structure baseline (sitemap-first)

Before composing the user-facing ask, run the sitemap-based URL
structure baseline. This replaces the earlier framing of "ask the user
for crawl data" — crawl tools (Screaming Frog, Sitebulb, or equivalent)
are the *bonus*, not the source.

1. `WebFetch("<domain>/sitemap.xml")` or fetch via bash. If it returns
   a sitemap index, fetch the top 2–3 nested sitemaps (typically
   `sitemap_pages.xml`, `sitemap_posts.xml`, `sitemap_products.xml` or
   similar).
2. Extract URLs and classify by path pattern (product URLs, category
   URLs, editorial URLs, help-centre URLs). This produces a coarse
   page-type distribution without crawling content.
3. Feed the distribution into §9.5 (topic structure) and the source
   authority inputs for §13.9 (URL gap analysis) / §13.15 (own-brand
   URL citation map).

**This is available on every commercial site without user effort and
should be the starting point for URL-structure discovery on every
project.** Where Screaming Frog, Sitebulb, or equivalent crawl data is
available (via Ring 1 brand-context skills or Ring 3 user provision),
use it to *enrich* the sitemap baseline — adding
page-type-segmentation, broken-link detection, and indexability flags
that the sitemap alone can't give you. Crawl data is never a
substitute for the sitemap baseline because it's not always available;
it's additive when it is.

**What the sitemap doesn't replace:** internal linking, crawlability
data, rendered content, structured-data coverage. When a full crawl is
available, prefer it for those specific signals.

**Scope caveat:** sitemaps may be truncated, outdated, or exclude
noindex URLs. Treat the URL list as "publishable pages the site wants
indexed", not as the full content footprint.

#### 8.3.3a Targeted data request (attachment-centric, plain text)

After sitemap baselining (§8.3.2), the agent composes the data request
as **plain text** listing every category the user could provide an
attachment or export for. This is the "please send me these files"
ask — not a widget. Use AskUserQuestion ONLY for the scoping widget in
§8.3.3b; the data request must be a separate plain-text message so the
user can respond item-by-item with attachments.

Frame each option with what's already known — e.g. "I already have
your crawl export and taxonomy doc from the brand folder; do you also
have a fresher search-console export or should I use the existing
one?" instead of "Do you have crawl data?"

**Mandatory template for the data request.** The plain-text ask must
enumerate every row from the `ring3_data_disposition` table (§7.2) that
is currently `outstanding`. For each row, state:

- What the data is (plain language).
- How to provide it (CSV attachment, URL, pasted text).
- Why it matters (one sentence connecting to Strategy).
- That "I don't have this / not applicable" is a valid response.

The user's reply then populates the `ring3_data_disposition` table —
each row becomes `received` (data provided), `declined_by_user` (user
explicitly declined in writing), or `not_applicable`. Rows the user
doesn't address remain `outstanding`, and Strategy cannot proceed.

The standard categories to check for gaps (skip any already covered by
§8.3.1):

1. **SEO tool data** — Ahrefs, Semrush, DataForSEO, or equivalent.
   Adds AI Overview presence flags, CPC × SV, SERP competitor context.
2. **Search Console data** — GSC export or direct access. Top queries
   over 12 months. Note: the GSC UI export is capped at ~1000 rows.
   To go beyond that — pull several thousand queries if useful — use
   the GSC API (directly, or via a connected GSC MCP).
3. **Web analytics revenue data** — GA4, Adobe, Matomo, Piwik Pro.
   Landing-page revenue by category. **Strongest commercial signal
   for e-commerce.** Handling notes:
    - **Exclude navigational / utility from the denominator.** Homepage
      (`/`), checkout, account, login, password recovery, wishlist, site
      search results, and loyalty program URLs capture revenue from
      users who already know the brand and aren't discoverable via AI
      visibility. Compute category allocation against the "discoverable
      content" subset only.
    - **Classify product detail pages into categories.** Product detail
      pages typically outnumber category pages 10:1 and aggregate to a
      significant share of revenue. Use URL keywords, breadcrumb
      structure, or crawl data to classify each product URL into a
      category — otherwise the allocation table underweights categories
      that earn their revenue through individual product pages.
    - **Watch for transient / price-driven revenue clusters.** Sale,
      clearance, outlet, and discount sections often concentrate
      significant revenue into a single URL (`/sale`, `/clearance`,
      `/outlet`). These represent a *buying behaviour* signal that no
      keyword tool will surface because they aren't keyword-driven —
      users respond to price framing. Treat them as their own intent
      cluster (see §11.15).
    - **Understand attribution.** GA4's default is last-non-direct
      click. Categories that sit late in the purchase journey (checkout,
      account, loyalty) get over-credited; categories early in the
      journey (blog, informational content) get under-credited. The
      AI-visibility journey is typically early-to-mid-funnel, so tilt
      the allocation toward categories that convert from first-visit or
      mid-journey landings.
    - **Don't shrink a category to zero on revenue alone.** A category
      at €0 revenue may still be strategically important (emerging
      category, category the client wants to enter). Keep 2 diagnostic
      prompts so the AI visibility gap stays measurable.
    - **Cross-check against GSC.** If GSC clicks and GA4 revenue agree
      on rank order, confidence is high. When they disagree, GA4 is
      usually the better guide for allocation on e-commerce — but note
      the disagreement in the intake record so the reasoning is
      traceable.
    - **Non-e-commerce equivalent:** for services, B2B, or SaaS, the
      mirror signal is "leads/signups per landing page" or "pipeline
      value per landing page" from CRM/analytics. Same principle —
      measure outcome, not demand.
4. **Website crawl data** — Screaming Frog, Sitebulb, or equivalent
   crawl export. Page type distribution, content gaps,
   product/category inventory.
5. **Customer-voice data** — sales call transcripts (Gong, Chorus,
   Fireflies), support tickets (Zendesk, Intercom), site search query
   logs, post-purchase survey text, live chat logs. Even 100–500 rows
   matters.
6. **Brand-specific context** — buyer personas, common prospect
   questions, existing taxonomy, brand guidelines, regulatory notes,
   competitor intelligence.

#### 8.3.3b Scoping widget (AskUserQuestion)

The scoping widget uses AskUserQuestion for short-choice questions
about scope, budget, priority, and positioning. This is a **separate
call** from the plain-text data request in §8.3.3a — don't merge them.

Typical scoping questions (omit any already answered by existing
intake / brand-context skill):

- Country scope — single market, primary + secondary, multi-market?
- Prompt budget — how many prompts should the plan cover in Loop 1?
- Engine priority — should the strategy privilege specific engines (e.g.
  ChatGPT + AI Overview for a DE e-com site) or spread evenly?
- Brand positioning — customer-facing vs. enterprise-facing, premium vs.
  value, etc.
- Assortment-brand handling (§4.6 / §9.2) — should stocked brands be
  tracked as roster brands, used as tags, or omitted entirely?

Each AskUserQuestion question should carry an explicit "Override this
if…" slot per §4.5, so the user can divert from the recommended default
without being trapped in a multiple-choice menu.

#### 8.3.3c Open-ended data-source invitation

At the end of the scoping widget, include an open-ended option: "What
other data sources do you think would help? Examples: existing prompt
sets in other AI-visibility platforms, industry report subscriptions,
internal taxonomy docs, CRM segmentation exports, sales call
libraries, etc." This invites user expertise rather than assuming the
skill's enumerated list is complete. The user has context the skill
doesn't — the skill should invite that context, not cap it.

#### Ring 3 handling

Two distinct messages in sequence: §8.3.3a (plain-text data request)
followed by §8.3.3b (scoping widget). Both must fire before Strategy.
Per §3.8, the data request is not optional even if Ring 1 is rich.

If the user provides data, save it to the persistence store (§7) so
subsequent runs don't re-ask. Specifically:

- CSV/XLSX exports go under the persistence store's user-provided
  subdirectory (or equivalent per §3.7 — mechanism is agent/user
  choice).
- Free-text responses go into the intake state in the relevant field.
- Dated snapshots preserve what was provided when.
- The `ring3_data_disposition` table (§7.2) is updated row-by-row
  based on the user's reply — `received`, `declined_by_user`, or
  `not_applicable`. Rows without explicit disposition remain
  `outstanding` and block the Strategy gate.

### 8.4 Intake summary output (mandatory gate for §9 Strategy)

At the end of the Intake sub-phase, the agent writes:

- Updated intake state (persisted per §7).
- A **mandatory** intake summary block in the session output. Strategy
  (§9) cannot begin until this block has been written and contains
  non-empty entries — or explicit "skipped because …" rationales — for
  every row below. If any Ring 2 or Ring 3 row is absent rather than
  explicitly skipped, return to Intake.

Required rows:

  - Loop number (first loop vs subsequent).
  - Project state (new / existing).
  - Ring 1 tools that populated data.
  - Ring 2 steps completed (first loop only). Each of Steps A–F must
    appear with a result or an explicit skip rationale.
  - Ring 3 automated inventory (§8.3.1) — what data was already
    accessible.
  - Ring 3 sitemap baseline (§8.3.2) — URL structure extracted from
    `<domain>/sitemap.xml` with page-type distribution.
  - **External data received — itemised.** Reproduce the
    `ring3_data_disposition` table (§7.2) verbatim, one row per data
    category, each marked `received` / `declined_by_user` /
    `outstanding` / `not_applicable`. This row is the authoritative
    check that closes the "planned deferral" skip shape (§3.8 shape 3):
    a blanket "Ring 3 user-provided data: confirmed unavailable" is no
    longer an acceptable summary — every category must be listed with
    its specific disposition. If any row is `outstanding`, return to
    §8.3.3a.
  - Gaps: which data sources returned nothing (and how the strategy
    will compensate).
  - Pre-Write quality gates (§15.1) — the agent must write the
    checklist results into the Intake summary block verbatim, including
    ✓ / ✗ marks and the rationale for any ✗. This turns the checklist
    from an internal practice into a visible artefact the user can
    inspect.

This block feeds the Strategy sub-phase (§9) and eventually any
stakeholder-facing intake-and-data-sources summary in Phase B.

---
