# Pattern library — Peec implementation (§11)

Part of the **peec-ai-tracking-strategy-builder** skill (CC BY 4.0 — Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)). This file holds the Peec-specific part of §11; the platform-agnostic rules are in the core skill `ai-visibility-tracking-strategy-builder`, which must be loaded alongside. Section numbers are global across the skill family — the section map in `SKILL.md` says where each § lives.

**Load trigger:** Read the core `references/pattern-library.md` first (skim its contents list during every Strategy and every Analyse step). Open this file whenever a core pattern's fix says "see §11.N in the platform companion", and always before executing any Peec write or report call that a pattern prescribes. §11.6 lives here in full — read it whenever `list_models(is_active=true)` returns 3 or fewer engines.

**Contents:**

- 11.1 — Peec implementation (`domains`, classification `OWN` vs `CORPORATE`, batched `update_brand`)
- 11.2 — Peec implementation (no `is_sister` flag; `[Sister]` prefix write sequence)
- 11.3 — Peec implementation (`delete_tag`)
- 11.6 Plan-tier model coverage gating (full pattern — Peec-only mechanism)
- 11.8 — Peec implementation (`list_search_queries`)
- 11.9 — Peec implementation (dead-weight listicle report recipe)
- 11.13 — Peec implementation (`sources`, `list_search_queries`, engine architecture pointers)
- 11.14 — Peec implementation (`mentioned_brand_ids`, `classification=CORPORATE`, `gap >= 2`)
- 11.16 — Peec implementation (sub-brand roster entry)
- 11.19 — Peec implementation (per-prompt breakdown call)
- 11.22 / 11.23 — Peec implementation (refusal and empty-response detection via `get_chat`)

---

## 11. Pattern library — Peec parts

The symptom / diagnosis / action of every pattern is in the core file.
Each part below adds only the Peec mechanism a core step refers to.

### 11.1 — Peec implementation

The "domain list" is the own brand's `domains` array. Domain
classification in `get_domain_report` / `get_url_report` treats
non-listed own TLDs as `CORPORATE`, not `OWN` (see `peec-ai-mcp` §7.10).
For existing projects the fix is a required `update_brand` in the Write
sub-phase (§12) — this triggers background recalculation (see
`peec-ai-mcp` §7.19), so batch all `name` / `aliases` / `regex` /
`domains` changes into a single call.

### 11.2 — Peec implementation

Peec's data model has no `is_sister` flag. Peec's brand-matching
respects the `name` field for detection, so the core rule applies in
full: prefixing `name` with `[Sister]` without populating `aliases` in
the same call breaks detection silently.

Correct write sequence for option 1:

1. Read the current brand record (`list_brands`, capture current
   `name`, `aliases`, `regex`).
2. `update_brand(brand_id, name="[Sister] <original_name>",
   aliases=[<original_name>, ...existing_aliases])`.
3. Verify: re-run `get_brand_report` for that brand ID over the same
   time window you had before. Mention counts should be within
   ±5% (noise window). If they drop materially, the alias didn't take —
   revert the name change and investigate.

Never run step 2 without step 3, and never run the `name` update
without the `aliases` update in the same call (Peec's update semantics
don't guarantee atomicity across separate calls).

### 11.3 — Peec implementation

Retire the duplicated tag with `delete_tag` after retagging the
affected prompts.

### 11.6 Plan-tier model coverage gating

**Symptom:** `list_models(is_active=true)` returns 3 or fewer engines
despite the plan's marketing claims of broader coverage.

**Diagnosis:** TRIAL and lower-tier Peec plans cap active engines.
Plan tier isn't exposed directly in the MCP — active-engine count is
the best available proxy.

**Action:** Strategy §9.6 branches model-coverage recommendation on
this. On a gated plan, don't waste cycles on "which engines should we
track" — the answer is "the ones you have". Redirect strategy effort to
prompt quality, aliases/regex, and measurement hygiene. Surface the
gating in the Strategy sign-off so stakeholders can factor it into
plan-upgrade decisions.

### 11.8 — Peec implementation

The query-fanout report is `list_search_queries`; the `site:` fanout
queries appear in its query text.

### 11.9 — Peec implementation

**Report recipe:** Use `get_url_report` with `filters:
[{field: "domain", operator: "in", values: <own_brand.domains>}]` — **not**
a classification filter; classification isn't server-side filterable
(see `peec-ai-mcp` §7.31). Sort by `retrievals` (integer, URL report
column — see `peec-ai-mcp` §7.39). Then calculate
`citation_rate = citations / retrievals` client-side and flag URLs
where retrievals are high and citation_rate ≈ 0. `peec-ai-mcp` §8.10
steps 1, 3, 5 walk through this exact recipe.

### 11.13 — Peec implementation

The fanout data is `list_search_queries`; the chat's source list is the
`sources` array on the chat payload (`get_chat`). "Empty source list"
in the core text means `sources: []`. For the per-engine architectural
reasons behind parametric vs retrieval-first behaviour, see the
`peec-ai-mcp` skill §7.5. Before labelling chats as parametric, apply
the sampling caveat: `sources: []` has multiple causes (see
`peec-ai-mcp` §7.8 and §11.23 below).

### 11.14 — Peec implementation

"Gap of 2 or more" is the `gap >= 2` filter on the URL / domain gap
reports. The roster auto-suggestion that skews toward information sites
is Peec's own suggestion feature. Cross-reference `mentioned_brand_ids`
in the top-N gap URLs / domains against `list_brands`; any frequently-
appearing brand ID, or `classification=CORPORATE` domain not associated
with a tracked brand, is the roster candidate.

### 11.16 — Peec implementation

Add the sub-brand as a first-class entry in `list_brands` — as an
own-brand alias or a separate brand entity depending on domain overlap.
The brand-domain rules that decide which are in the `peec-ai-mcp` skill.

### 11.19 — Peec implementation

The per-prompt visibility breakdown is
`get_brand_report(dimensions=[prompt_id])`, run on every INVEST /
borderline topic in Analyse.

### 11.22 / 11.23 — Peec implementation

Peec's aggregate fields treat a content-policy refusal, an empty or
placeholder engine response and a genuine non-mention identically —
no error, no warning. The three states are separable only by reading
the chat payload via `get_chat`: an empty or placeholder body (e.g.
`"No response."`) is the §11.23 engine-returned-empty state; a
populated body reading as "can't help" / "not able to provide" /
"recommend consulting a professional" is the §11.22 refusal state; a
populated body with `sources: []` is the §11.13 parametric state. The
`chatgpt-scraper` model produces refusals most often, with Claude and
Gemini occasionally. The full seven-cause list for empty results, and
the sampling procedure, are in `peec-ai-mcp` §7.8.

---
