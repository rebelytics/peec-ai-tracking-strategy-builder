# Phase A — Strategy (§9–§10)

Part of the **peec-ai-tracking-strategy-builder** skill (CC BY 4.0 — Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)). Section numbers are global across `SKILL.md` and `references/` — the section map in `SKILL.md` says where each § lives.

**Load trigger:** Read before drafting or revising any strategy recommendation, and again before the strategy sign-off. §10 applies to existing projects only.

**Contents:**

- 9.1 Prompt volume split (load-bearing recommendation)
  - 9.1.1 Search-volume axis (orthogonal to the funnel split)
- 9.2 Country and market scope
- 9.3 Brand roster
  - Own-brand configuration
  - Brand classification step (mandatory, before proposing any roster)
  - Competitor configuration
- 9.4 Tag taxonomy
  - Taxonomy hygiene check (existing projects only)
- 9.5 Topic structure
- 9.6 Model coverage (plan-tier aware)
  - Branch A: Plan permits engine choice (4+ active engines available)
  - Branch B: Plan caps active engines (3 or fewer)
- 9.6.1 Prompt-credit detection (known MCP gap)
- 9.7 Reporting KPI split (branded vs non-branded)
- 9.8 Strategy sign-off

---

## 9. Phase A — Strategy

**Hard gate:** Strategy cannot begin until the Intake summary block
(§8.4) has been written and contains non-empty entries — or explicit
"skipped because …" rationales — for each of: Ring 1 tools used, Ring 2
steps completed (first loop only), Ring 3 automated inventory + user
ask, and identified gaps. If any row is absent rather than explicitly
skipped, return to §8 Intake. See §3.8 for why this gate exists.

Goal: convert the intake into a concrete, prescriptive strategy
recommendation. The user accepts or calls out an override. No menus,
no "which would you like" questions.

Every recommendation block follows the same structure:

> **Recommended:** *[concrete numbers, categories, or choices]*
>
> **Reasoning:** *[1–2 sentences tied to intake data]*
>
> **Override this if:**
> - *[condition 1]* → *[what to change]*
> - *[condition 2]* → *[what to change]*
> - *[etc.]*

### 9.1 Prompt volume split (load-bearing recommendation)

> **Recommended:** 50% discovery, 30% consideration, 15% comparison,
> 5% branded reputation monitoring. Tag each prompt with `funnel:<tier>`.
>
> **Reasoning:** Discovery dominates where the brand needs to attract
> new customers (the overwhelming majority of Peec use cases).
> Comparison slots capture head-to-head competitor queries where AI
> answers frequently rank. Branded reputation monitoring measures how AI
> describes the brand when asked about it by name — useful, but should
> never dominate because branded prompts score near 100% visibility by
> construction (§4.10, §3.1).
>
> **Override this if:**
> - B2B or long sales cycle → flip to 25/45/20/10 (more consideration
>   weight).
> - Strong existing brand equity and branded prompts already at
>   visibility=1.0 → drop branded reputation monitoring to 0%, reclaim
>   slots for discovery.
> - Regulated vertical (cannabis, pharma, gambling, finance) → add a
>   `topic:safety` band at ~10%, taken off discovery; expect
>   legal-caution framing in AI responses (see §11 pattern library,
>   regulatory-aware sentiment).
> - TRIAL plan or ≤50 prompt slots → drop comparison entirely; focus
>   on discovery + branded reputation monitoring only.

#### 9.1.1 Search-volume axis (orthogonal to the funnel split)

The funnel split above is the **primary** axis. Peec also
exposes a **second orthogonal axis** via `list_prompts.volume` — the
search-volume ordinal that was previously only visible in the UI is
now pullable through MCP (see `peec-ai-mcp` §7.42). Treat volume as a
distinct prompt-portfolio axis alongside funnel stage; a prompt
portfolio that's balanced on funnel but dominated by "very low" volume
prompts is materially under-weighted for commercial coverage.

> **Recommended volume mix (within each funnel tier):**
> - **Head (high / very high):** 20–30% — tests whether the brand
>   surfaces on the queries that drive the category.
> - **Mid (medium):** 40–50% — the workhorse prompts that carry most of
>   the signal.
> - **Long tail (low / very low):** 20–30% — captures niche / specific
>   intent and keeps long-tail coverage legible.
>
> **Reasoning:** A portfolio that's all head prompts is great for
> visibility headlines but hides long-tail gaps; all long-tail misses
> the queries that actually drive category traffic. The orthogonal
> distribution means each funnel tier itself has head/mid/tail
> coverage, not just the roster as a whole.
>
> **Override this if:**
> - Pre-existing TRIAL-tier project with <30 prompts → drop long tail
>   entirely; focus on head + mid so the small budget doesn't fragment.
> - Very niche vertical where head-volume queries don't exist (e.g. a
>   specific B2B SaaS category) → the "head" tier may be empty by
>   nature; concentrate on mid + long tail and note the constraint.
> - Project whose current portfolio is already ≥80% "very low" volume →
>   Loop 2 should prioritise adding head/mid prompts over adding more
>   long-tail. The `volume` signal makes this measurable.

**Handling the volume ordinal in code.** `list_prompts.volume` returns
string ordinals (`"very low"` / `"low"` / `"medium"` / `"high"` /
`"very high"`) — not the 1–5 integers the schema claims. Do not
numerically sort or compare without first mapping to integers
client-side. See `peec-ai-mcp` §7.42 for the field behaviour.

**Using volume in Analyse.** In §13.3 (branded vs non-branded
findings), segment visibility by volume tier — "we have 30% visibility
on high-volume discovery prompts and 70% on very-low discovery
prompts" is a very different story from a flat "50% discovery
visibility" headline. Volume segmentation turns a muddled average into
a commercially-meaningful diagnosis.

### 9.2 Country and market scope

> **Recommended:** Start with the top 2 markets by revenue or traffic.
> Add `country_code` on every prompt from day 0.
>
> **Reasoning:** Multi-market is cheap to add now, painful to backfill
> — every prompt without `country_code` is a prompt that won't be
> filterable by market later. See `peec-ai-mcp` §7.15 — `country_code`
> is required on create; there is no `language` field (language is
> inferred from text).
>
> **Override this if:**
> - Single-market brand → use one country_code everywhere, but set
>   it explicitly.
> - Multi-TLD with shared content across markets → track flagship
>   market first, add others once flagship visibility data exists.
> - The brand operates in a country outside Peec's 92-country enum
>   → flag and discuss fallback with the user before proceeding
>   (see `peec-ai-mcp` §7.15).

### 9.3 Brand roster

> **Recommended:** Own brand with full owned-domain list + 5 tracked
> competitors, manually curated.
>
> **Reasoning:** Peec's auto-selected competitors skew to reference
> and UGC sites rather than commercial rivals (§4.6). A manually
> curated shortlist of 5 genuine competitors produces cleaner
> share-of-voice data than a sprawling 15+ list.

#### Own-brand configuration

> **Recommended:** For the own brand, set:
> - `domains` = all TLDs the brand operates (not just primary).
> - `aliases` = all common brand-name variants seen in AI responses
>   (e.g. a fuller legal name, a bare domain, or an initials shorthand
>   like "ACME" for "Acme Coffee Company").
> - `regex` = set only if aliases can't capture the variant space
>   (e.g. multiple word-boundary cases). Pass `null` otherwise.
>
> **Reasoning:** Multi-TLD own-brand classification (§4.6, and
> `peec-ai-mcp` §7.10) — any owned TLD missing from `domains` will be
> classified as `CORPORATE` in domain reports, not `OWN`. That silently
> misreads cross-TLD mentions as competitive. Missing aliases are the
> single most common cause of understated own-brand mentions in Peec.
>
> **Override this if:**
> - Brand genuinely operates only one TLD → single domain is correct.
> - Brand has overlapping TLDs with different companies (rare, but
>   possible in regulated trademarks) → omit conflicted TLDs and note
>   in the persisted intake state.

#### Brand classification step (mandatory, before proposing any roster)

Before proposing a brand as a competitor, classify it against the
three-category shape from §4.6:

1. **Commercial competitor** — distinct business, fighting for the same
   customer's wallet. Add to the roster with `is_own=false`; classify
   in the persisted intake state as `direct_competitor`,
   `aspirational`, or `sister_brand`.
2. **Assortment brand** — a brand the own retailer stocks and
   merchandises (typically appears as a product line at
   `/collections/{brand}`, a Shopify collection, or a brand category
   page on the own site). Detect via a sitemap scan (§8.3.2) for
   `/collections/`, `/brands/`, `/manufacturer/`, or equivalent path
   patterns. Retailing the brand doesn't make it a competitor —
   conflating the two inflates competitor counts and corrupts gap
   analysis. Surface these separately in the Strategy output so
   stakeholders see the distinction.
3. **Marketplace / generic noise** — Amazon, Google Shopping, generic
   directory pages. Not a competitor; not stocked; skip entirely from
   the roster.

**Assortment-brand handling rules:**

- **Don't add as a roster competitor by default.** Assortment brands
  inflate SoV denominators and can turn the own retailer's own
  assortment into a headline "competitor threat".
- **Tag, not track** (preferred). Tag prompts that mention the
  assortment brand with a `brand:<name>` tag so topic/tag-filtered
  reports can surface per-assortment-brand signal without polluting
  the competitor SoV.
- **Topic sub-structure** (alternative, for assortment-heavy sites).
  If the retailer merchandises by brand as a primary navigation axis
  (e.g. a sneaker store with per-brand landing pages dominating the
  URL structure), it may make sense to use those brand names as
  topics or sub-topics (§9.5). Prefer the tag approach unless the
  brand roster is very small and assortment coverage dominates the
  commercial structure.
- **User decides.** Surface the classification choice explicitly in
  §8.3.3b (scoping widget) rather than assuming.

#### Competitor configuration

> **Recommended:** For each of the 5 commercial competitors (not
> assortment brands — see the classification step above), set `name`,
> `domains`, `aliases`. Skip `regex` unless needed. Classify each
> competitor in the persisted intake state as `direct_competitor`,
> `aspirational`, or `sister_brand`.
>
> **Reasoning:** Sister-brand misclassification is a portfolio-brand
> failure mode (§11 pattern library). A sibling brand in AI responses
> takes share from the own brand on paper, but the group still wins —
> reports that don't distinguish sister brands from rivals will
> systematically overstate competitive pressure.
>
> **Override this if:**
> - Brand is part of a corporate group with sibling brands in the
>   same vertical → prefix sister-brand `name` with `[Sister]` in
>   Peec so reports self-document (Peec has no native `is_sister`
>   flag). **Important:** when applying this prefix, the `aliases`
>   array must be populated in the same update call with the
>   original brand name, or brand detection breaks silently — see
>   §11.2 for the full pattern and safe wave ordering.
> - More than 5 genuine commercial competitors exist and the plan
>   allows → add up to 10, but treat the extras as secondary in
>   SoV calculations.
> - Fewer than 3 real commercial rivals exist (niche / category
>   leader) → populate with 3 aspirational competitors (market leaders
>   the brand wants to benchmark against).

### 9.4 Tag taxonomy

> **Recommended:** Three axes, 15–25 tags total:
> - **Intent:** `intent:commercial`, `intent:comparison`,
>   `intent:transactional`, `intent:informational`, `intent:branded`
> - **Funnel:** `funnel:awareness`, `funnel:consideration`,
>   `funnel:decision`, `funnel:branded`
> - **Category:** one tag per business category from the topic
>   structure (§9.5), prefixed `cat:`
>
> Every prompt carries at least one tag from each dimension.
>
> **Reasoning:** 2–3 dimensional taxonomies stay consistent under
> growth. More dimensions produce orphaned tags and inconsistent
> application. Never parallel two dimensions that measure the same
> thing (e.g. don't maintain both `transactional` and
> `funnel:decision` — pick one).
>
> **Override this if:**
> - Brand already has a taxonomy in use (brand guidelines, GSC query
>   groupings) → mirror it rather than invent a parallel one.
> - Regulated vertical → add a `regulatory` dimension with tags like
>   `regulatory:restricted` so sentiment reports can filter these out
>   of headline numbers (see §11 pattern library).
> - Portfolio brand with sister-brand overlap → add a `relationship`
>   dimension with `relationship:sister` vs `relationship:competitor`
>   so SoV reports can distinguish.
> - Tag count would exceed 25 with all planned dimensions → drop the
>   weakest dimension (usually intent or comparison) and fold it into
>   a wider tag.

#### Taxonomy hygiene check (existing projects only)

For existing projects, before proposing the taxonomy, run a duplication
check:

1. For each pair of tags in `list_tags`, compute the prompt-set
   overlap (intersection of their prompt IDs, divided by the smaller
   set).
2. Flag any pair with >60% overlap as a duplication candidate.
3. For each flagged pair, propose which tag to retire and which to
   keep.

Common overlaps: `transactional` ↔ `funnel:decision`,
`informational` ↔ `funnel:awareness`, `branded` ↔ `funnel:branded`.

### 9.5 Topic structure

> **Recommended:** 5–8 topics for single-market projects. Each topic
> maps to a business category. Topic names should be clean (no
> prefixes duplicating tag dimensions — `cat:seeds` is a tag, "Seeds"
> is a topic). **Topics name categories the own brand sells into —
> never commercial competitor names** (§4.8).
>
> **Reasoning:** More than 10 topics usually signals either multiple
> markets mashed into one project (topic-per-market is wrong — that's
> `country_code`'s job) or topics acting as tags. Every tag that names
> a prompt cluster larger than ~3 prompts should be considered a
> candidate topic, not just a tag (§4.8). A competitor brand name as a
> topic is a strong signal the brand roster (§9.3) isn't classified
> correctly — competitors belong on the roster with `is_own=false`,
> not as topics.
>
> **Override this if:**
> - Multi-category marketplace with genuinely distinct verticals → up
>   to 12 topics is acceptable if each has 8+ prompts.
> - Single-vertical specialist → as few as 3–4 topics is fine.
> - Existing project has overlapping topics (e.g. "Growing" and
>   "Growing Equipment") → propose a merge with `update_prompt.topic_id`
>   to move prompts, then `delete_topic` the redundant one.
> - Assortment-heavy retailer where brand names dominate site structure
>   → assortment brand names may legitimately serve as sub-topics or
>   (preferred) as `brand:<name>` tags; see §9.3's
>   "Brand classification step" for the decision criteria.

**Prompt disposition and assortment brands.** When reviewing an
existing project's prompt set against the new topic structure, watch
for prompts whose topic is a brand name. Two sub-cases:

- **Topic is a commercial competitor name** → move the prompt to the
  relevant category topic and add a `competitor:<name>` tag. This
  preserves the data while routing the signal to the right axis.
- **Topic is an assortment brand name** → apply §9.3's classification
  rules. If the retailer merchandises by brand at primary-navigation
  depth, the topic may be legitimate; otherwise move the prompt to a
  category topic and tag with `brand:<name>`.

Either move is a `update_prompt.topic_id` call (not a text edit — see
`peec-ai-mcp` §7.13 for why text is immutable).

### 9.6 Model coverage (plan-tier aware)

**First, detect plan tier.** Count active engines via
`list_models(project_id, is_active=true)`. If 3 or fewer, the plan
most likely caps model coverage.

#### Branch A: Plan permits engine choice (4+ active engines available)

> **Recommended:** Match models to audience:
> - **European B2C retail:** ChatGPT (scraper), Google AI Overview
>   (scraper), Perplexity, Grok.
> - **Developer tools / B2B SaaS:** add Claude; drop Grok unless the
>   audience is on X.
> - **German-speaking / DACH:** prioritise Google AI Overview —
>   highest German-language query volume.
>
> **Reasoning:** Every tracked model inflates chat count and plan cost
> proportionally. Don't default to "all models" — match audience.
>
> **Override this if:**
> - Developer / technical audience → include Claude and Perplexity
>   prominently; deprioritise AI Overview.
> - Global English audience → add Perplexity, drop regional variants.
> - Enterprise procurement context → include Copilot.

#### Branch B: Plan caps active engines (3 or fewer)

> **Recommended:** Accept the active engine set as a constraint.
> Focus strategy effort on prompt quality, brand-detection config
> (aliases, regex, domains), and measurement hygiene. Revisit engine
> selection if upgrading plan.
>
> **Reasoning:** On a gated plan, "which engines should I track" is
> the wrong question — that choice isn't yours to make. The right
> question becomes "am I getting maximum signal from the engines I
> have?", which is almost entirely about prompt quality and brand
> configuration (§9.3 own-brand aliases/regex) rather than engine
> rotation.
>
> **Override this if:**
> - Plan upgrade is under discussion → surface model-coverage gaps
>   as inputs to that decision.
> - Active engine set doesn't include any of the brand's priority
>   audience's preferred AI tools → flag and recommend upgrade.

### 9.6.1 Prompt-credit detection (known MCP gap)

Plan detection has two axes: **engines** (detectable via MCP — §9.6
above) and **prompt credits** (not detectable). `list_projects` does not
return plan information. The `peec-ai-mcp` companion skill confirms that
`get_credit_balance` does not exist (see `peec-ai-mcp` §7.37).

The agent must always ask the user for the project's prompt allocation
as part of the known-MCP-gaps block (§8.1). Frame the ask as: "Peec's
MCP doesn't expose plan credits, so I need this from you directly: how
many prompts does your plan allow?" Single precise question — not a
multi-tier multiple choice (e.g., "20–30 vs 50–80 vs 100+").

Persist the answer in the intake state (§7) under a `plan.prompt_credits`
field (or equivalent) so subsequent loops don't re-ask. Surface the
prompt-credit constraint alongside the engine-count detection so the
user answers all plan-related questions in a single pass.

### 9.7 Reporting KPI split (branded vs non-branded)

> **Recommended:** Report branded-prompt metrics and non-branded-prompt
> metrics as **separate KPIs**. Never average them into one headline
> visibility number.
>
> **Reasoning:** Branded prompts score near 100% visibility by
> construction (the prompt mentions the brand). Rolling them into
> overall SoV inflates headline metrics and obscures the real
> "unprompted discovery" signal (§4.10, §3.1).
>
> **Override this if:**
> - Brand has no branded prompts tracked → skip this split (it's a
>   non-issue). Still recommended to add 2–3 branded prompts for
>   brand-awareness tracking, but keep them tagged `funnel:branded`
>   and filter them out of discovery reports.

**Strict-with-disclosure rule.** Separate reporting of branded vs
non-branded is required (§3.1). If a combined metric is ever produced —
e.g. because a stakeholder insists on a single headline — it must be
accompanied by an explicit "this is an anti-pattern" disclosure and a
breakdown showing both cohorts underneath. Never silently produce a
blended figure. The disclosure isn't a formality; it's the mechanism
that prevents the blended number from becoming the canonical one.

**Implementation — Peec has no native branded/non-branded concept.** The
user-defined tag pattern is the standard. Set two tags at prompt
creation:

- `branded` — prompts that contain the brand's own name
- `non-branded` — every other prompt

Both tags must be applied at creation time (a prompt with no
branded/non-branded tag is a configuration bug — it can't be filtered
into either cohort). Tags are the cleanest split because they survive
topic restructuring (§9.5), they cross topic boundaries (a branded
prompt can live under any topic), and they flow through `get_brand_report
filters=[{tag_id:...}]` natively. Document the tag IDs in the intake
state (§7) so downstream loops can compute the split without re-deriving.

**Codify this as a value-add.** Whenever a Peec project is built or
rationalised, the branded/non-branded tag pair is part of the strategy
deliverable, not an implementation detail. The tag naming, the
convention, and the rationale appear in the sign-off artefact (§9.8) —
it's a piece of methodology the strategy adds on top of what Peec ships
natively.

### 9.8 Strategy sign-off

Before the Write sub-phase (§12) runs, the user must sign off on the
Strategy output. The **sign-off is required; the format is not.**

The sign-off artefact can take whatever form fits the engagement:

- A full markdown strategy document (one example — see the schema below
  for a concrete structure).
- A concise structured message in the chat confirming each §9.1–9.7
  recommendation.
- An interactive list the user ticks through.
- A handoff doc in environments without persistence.

What the sign-off must capture, regardless of format:

- Loop number and date.
- Each §9.1–9.7 recommendation as accepted or overridden, with the
  override reasoning where applicable.
- Prompt disposition decisions (existing projects) — see §10.
- The implementation plan preview (what the Write sub-phase will do).
- The measurement plan (earliest sensible next Analyse per §12.7).
- Any content-strategy findings surfaced during intake that live
  outside Peec.

Example full-strategy-document structure (one of several valid formats):

```
# [Brand] Peec AI Tracking Strategy — Loop N
Date | Project | Build or Refine | Prepared by

1. Executive summary (1 paragraph)
2. Intake & data sources (what was consulted, what was provided,
   what gaps remain)
3. Strategic recommendations (one block per §9.1–9.7 with Recommended /
   Reasoning / Override blocks)
4. Prompt disposition — existing projects only (§10 six-bucket table)
5. Implementation plan (Write preview: what will be written to Peec,
   in what order)
6. Measurement plan (what to watch, earliest re-analysis date, baseline)
7. Content strategy findings (non-Peec actions surfaced during intake)
8. Appendix: intake state snapshot, data sources table, override decisions log
```

If the full-document format is chosen, it's **presentation quality** —
formatted for a stakeholder to read without further explanation. Use
tables, headings, and numbered blocks. Avoid agent-internal jargon.

If a shorter format is chosen, the sign-off still needs to be a concrete
artefact (not implicit) — something the user can point at and say "yes,
this is what I signed off on". The Analyse sub-phase (§13) will refer
back to it.

---

## 10. Prompt disposition framework (existing projects only)

When the project has existing prompts, every one of them is classified
into one of six buckets:

| Bucket | Criteria | Action |
|---|---|---|
| **Keep as-is** | Strong commercial intent, non-zero visibility, aligned with a strategy category | No write required |
| **Keep with retagging** | Good prompt but needs updated tags or topic | `update_prompt(tag_ids, topic_id)` — full replacement (see `peec-ai-mcp` §7.14) |
| **Keep as gap-to-close** | Legitimate commercial prompt, currently 0% visibility, strategy flags as a performance gap to hunt | Retag with `gap-to-close`; feed into content strategy recommendations |
| **Keep as diagnostic** | Zero visibility expected (structural gap, regulatory barrier, low-priority category), but worth measuring for trend | Retag with `diagnostic`; filter out of headline reports |
| **Reframe** | Right intent, wrong phrasing (wrong language, wrong product framing). `update_prompt` can't change `text` (see `peec-ai-mcp` §7.13), so this is `delete_prompt` + `create_prompt` paired. Loses historical data. | Paired delete + create. Only reframe when the improved framing is worth the data loss. |
| **Remove** | Educational (produces Wikipedia, not retailer mentions), duplicative, or zero-signal without diagnostic value | `delete_prompt` |

The **gap-to-close** bucket captures the common case of a legitimate
commercial prompt that currently scores zero — "where can I buy X in
Germany" for a brand that ought to appear there but doesn't. Keeping
it (vs deleting as noise) preserves the metric that tracks the gap
closing. This is different from `diagnostic`, which flags prompts we
expect to stay at zero.

The disposition table is part of the Strategy sign-off (§9.8) — concrete
and line-by-line when existing prompts are involved. Subsequent Analyse
loops (§13) will check the gap-to-close bucket for movement and may
reclassify prompts between buckets as evidence accumulates.

---
