# Phase A — Strategy — Peec implementation (§9–§10)

Part of the **peec-ai-tracking-strategy-builder** skill (CC BY 4.0 — Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)). This file holds the Peec-specific part of §9–§10; the platform-agnostic rules are in the core skill `ai-visibility-tracking-strategy-builder`, which must be loaded alongside. Section numbers are global across the skill family — the section map in `SKILL.md` says where each § lives.

**Load trigger:** Read together with the core `references/phase-a-strategy.md` before drafting or revising any strategy recommendation for a Peec project, and again before the strategy sign-off. §10 applies to existing projects only.

**Contents:**

- 9 Phase A — Strategy (gate and block format: core)
- 9.1 Prompt volume split — core
  - 9.1.1 Search-volume axis — Peec implementation (`list_prompts.volume`)
- 9.2 Country and market scope — Peec implementation (`country_code`)
- 9.3 Brand roster — Peec implementation
  - Own-brand fields (`domains` / `aliases` / `regex`)
  - Competitor fields and the `[Sister]` prefix rule
- 9.4 Tag taxonomy — Peec implementation (hygiene check via `list_tags`)
- 9.5 Topic structure — Peec implementation (topic operations, text immutability)
- 9.6 Model coverage (plan-tier aware)
  - Branch A: Plan permits engine choice (4+ active engines available)
  - Branch B: Plan caps active engines (3 or fewer)
- 9.6.1 Prompt-credit detection (known MCP gap)
- 9.7 Reporting KPI split — Peec implementation (the tag set, report filters)
- 9.8 Strategy sign-off — core
- 9.9 Prompt authoring — core
- 10 Prompt disposition framework — Peec action column

---

## 9. Phase A — Strategy

The hard gate (Intake summary block, §8.4), the goal, and the
Recommended / Reasoning / Override block format are defined in the core
§9. Every recommendation for a Peec project uses that format; the
sections below only add what Peec's fields, plan tiers and MCP tools
change.

### 9.1 Prompt volume split

Core §9.1 — the funnel split, the damped revenue-share weighting
(§9.1.2), the breadth heuristic (§9.1.3) and the total-size rule. Nothing
Peec-specific, except that the "very small budget" override applies to
TRIAL-tier projects and any plan with ≤50 prompt slots (see §9.6.1 for
how to find out).

#### 9.1.1 Search-volume axis — Peec implementation

Peec exposes the second orthogonal axis of core §9.1.1 via
`list_prompts.volume` — the search-volume ordinal that was previously
only visible in the UI is now pullable through MCP (see `peec-ai-mcp`
§7.42). The recommended head/mid/tail mix and its overrides are in the
core; the TRIAL-tier override there ("existing project with <30 prompts
on a capped budget") is the usual case for a pre-existing TRIAL project.

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

### 9.2 Country and market scope — Peec implementation

The market recommendation (top 2 markets, market attribute on every
prompt from day 0) and the cross-market divergence check are in the core
§9.2. In Peec the market attribute is `country_code`:

- Add `country_code` on every prompt from day 0. See `peec-ai-mcp` §7.15
  — `country_code` is required on create; there is no `language` field
  (language is inferred from text).
- Single-market brand → one `country_code` everywhere, but set it
  explicitly.
- The brand operates in a country outside Peec's 92-country enum → flag
  and discuss fallback with the user before proceeding (see
  `peec-ai-mcp` §7.15). This is the "country the tool cannot represent"
  override of the core.
- Topic-per-market is wrong — that's `country_code`'s job (core §9.5).

### 9.3 Brand roster — Peec implementation

The roster size, the own-brand identity concept, the mandatory brand
classification step and the competitor selection criteria are in the
core §9.3. Peec's auto-selected competitors are the case in point for
the core's "tool-suggested lists skew to reference and UGC sites" (§4.6).

#### Own-brand fields

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

The overrides (single-TLD brand; conflicted TLDs) are in the core.

#### Competitor fields and the `[Sister]` prefix rule

Commercial competitors go on the roster with `is_own=false`. For each of
the 5 commercial competitors (not assortment brands — core §9.3
classification step), set `name`, `domains`, `aliases`. Skip `regex`
unless needed.

Brand is part of a corporate group with sibling brands in the same
vertical → prefix sister-brand `name` with `[Sister]` in Peec so reports
self-document (Peec has no native `is_sister` flag). **Important:** when
applying this prefix, the `aliases` array must be populated in the same
update call with the original brand name, or brand detection breaks
silently — see §11.2 for the full pattern and safe wave ordering.

Assortment brands are tagged, not tracked (`brand:<name>` — core §9.3);
they do not get a roster row. A competitor brand name appearing as a
topic is a signal the roster isn't classified correctly — competitors
belong on the roster with `is_own=false`, not as topics (core §9.5).

### 9.4 Tag taxonomy — Peec implementation

The cardinality rule, the three-axis 15–25 tag recommendation, the
namespace rule and the tag-sprawl discipline are in the core §9.4. Peec
is the **"both at once"** tool shape: a prompt carries exactly one
`topic_id` (the single-valued container) and any number of `tag_ids`
(the multi-valued field). A prompt's fields are `text`, `topic_id`,
`tag_ids` and `country_code` — there is no single-keyword back-reference
field, so the demand term a prompt was derived from is recorded in the
intake state (§7), not on the prompt. Where a sub-topic level is used
(§4.8) it subdivides its parent topic only, never a second axis.

**Taxonomy hygiene check (existing projects only).** Run the core's
duplication check by pulling the project's tag list with `list_tags`,
then computing the prompt-set overlap for each pair from `list_prompts`
(intersection of prompt IDs divided by the smaller set); flag >60%
overlap and propose which tag to retire.

### 9.5 Topic structure — Peec implementation

Peec's container is the **topic**. The 5–8 recommendation, the
"categories, not brand names" rule, the derivation steps and the
identical-across-markets rule are in the core §9.5.

Peec operations behind the core's dispositions:

- **Merging overlapping topics** → move prompts with
  `update_prompt.topic_id`, then `delete_topic` the redundant one.
- **Prompt whose topic is a brand name** (competitor or assortment
  brand — core §9.5 "Prompt disposition and brand-name containers") →
  the move is an `update_prompt.topic_id` call (not a text edit — see
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

### 9.7 Reporting KPI split — Peec implementation

The separate-KPI rule, the strict-with-disclosure rule, the three-case
brand-mention split, the two-tags-or-three decision, placement and ratio
are all in the core §9.7.

**Implementation — Peec has no native branded/non-branded concept.** The
user-defined tag pattern is the standard. Set the brand-mention tag at
prompt creation, choosing on **who is named in the prompt text**:

- `branded` — the own brand is named
- `other-brand` — a different brand is named and the own brand is not:
  a commercial competitor, or an assortment brand the retailer stocks
  (§4.6 / §9.3)
- `non-branded` — no brand is named

Exactly one of the three is applied at creation time. Tags flow through
`get_brand_report filters=[{tag_id:...}]` natively, which is why the
split lives in tags rather than topics. Document the three tag IDs in
the intake state (§7) so downstream loops (§13.3) can compute the split
without re-deriving.

### 9.8 Strategy sign-off

Core §9.8 in full. For a Peec project the example document title reads
"[Brand] Peec AI Tracking Strategy — Loop N", the implementation plan
lists what will be written to Peec and in what order (§12), and the
engine set from §9.6 above is recorded as an accepted or overridden
recommendation like the others.

### 9.9 Prompt authoring

Core §9.9 in full. Peec-specific constraint to carry into any authoring
brief: prompt `text` is immutable after creation (`peec-ai-mcp` §7.13), so
the educational-opener ban and the brand-mention tag must be applied at
authoring time — a prompt that needs its wording fixed later is a
paired delete + create (§10).

---

## 10. Prompt disposition framework — Peec action column

The six buckets and their criteria are in the core §10. The Peec
operations behind the Action column:

| Bucket | Peec action |
|---|---|
| **Keep as-is** | No write required |
| **Keep with retagging** | `update_prompt(tag_ids, topic_id)` — full replacement, not a merge (see `peec-ai-mcp` §7.14) |
| **Keep as gap-to-close** | Retag with `gap-to-close` (the bare tag string used on existing Peec projects; new projects may use the core's `signal:gap-to-close`); feed into content strategy recommendations |
| **Keep as diagnostic** | Retag with `diagnostic` (likewise `signal:diagnostic` on new projects); filter out of headline reports |
| **Reframe** | `update_prompt` can't change `text` (see `peec-ai-mcp` §7.13), so this is `delete_prompt` + `create_prompt` paired. Loses historical data. |
| **Remove** | `delete_prompt` |

Whichever tag strings a project uses, use one convention per project
and record it in the intake state (§7) so Analyse (§13) filters on the
right tag IDs.

---
