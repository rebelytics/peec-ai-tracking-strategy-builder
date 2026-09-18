# Quality gates — Peec implementation (§15)

Part of the **peec-ai-tracking-strategy-builder** skill (CC BY 4.0 — Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)). This file holds the Peec-specific part of §15; the platform-agnostic rules are in the core skill `ai-visibility-tracking-strategy-builder`, which must be loaded alongside. Section numbers are global across the skill family — the section map in `SKILL.md` says where each § lives.

**Load trigger:** Read together with the core `references/quality-gates.md` at every phase transition. The core file lists every gate item; this file says, for the items that can only be checked through Peec tools or fields, which call or field verifies them. A failed gate item blocks the transition regardless of which file it is listed in.

**Contents:**

- §15.1 — Peec implementation (pre-Write)
- §15.2 — Peec implementation (pre-Analyse)
- §15.3 — Peec implementation (pre-Phase-B)
- §15.4 — Peec implementation (merged-set validation)

---

## 15. Quality gates — Peec implementation

Each entry below names the core gate item it implements, then the Peec
tool call, field, or report through which that item is checked. Items
of the core checklist that are not listed here are checked the same way
on every platform and need no Peec-specific step.

### §15.1 — Peec implementation

- **Core item "every field the platform can expose was derived before
  the user was asked":** Known MCP gaps (§8.1 table) were checked
  before asking the user for market, plan credits, or other derivable
  fields. Specifically: `list_projects` returns `{id, name, status}`
  only — no country / market metadata and no plan data;
  `get_credit_balance` does not exist (see
  `peec-ai-mcp/references/gotchas.md` §7.11 "Write-operation consent,
  verification, and safe-experimentation patterns");
  `list_models(is_active=true)` **is** available and gives the active
  engine count. The user ask for market and prompt credits is prefixed
  with "The Peec MCP doesn't expose this at the project level, so I
  need to confirm with you:" and all three plan questions — prompt
  credits, engine count, run cadence — are surfaced together.
- **Core item "cost-driving platform facts are stated, not implied":**
  any cadence or cost statement in the strategy or in a client-facing
  document carries the credit formula (1 prompt × 1 model × 1 day = 1
  credit) and names the plan gate on weekly cadence (§9.6.1). A
  document that presents cadence as "daily or weekly" without the gate
  fails this item.
- **Core item "brand roster specifies owned domains, name variants
  and a detection pattern":** the roster specifies `domains`,
  `aliases`, and optional `regex` for own brand and every tracked
  competitor (§9.3 own-brand configuration). `regex` is `null` unless
  aliases cannot capture the variant space.
- **Core item "every proposed prompt has … a market (country)
  assignment":** every proposed prompt has a `country_code`, plus its
  `topic_id` and `tag_ids` (§9.5, §9.4).
- **Core item "engine coverage recommendation was made against the
  engines actually available on the plan":** the Model coverage
  section detected plan tier via `list_models(is_active=true)` (3 or
  fewer active engines = likely plan-capped) and routed to the correct
  branch — Branch A (plan permits engine choice) or Branch B (plan caps
  active engines) — of §9.6; prompt-credit detection followed §9.6.1
  (known MCP gap, ask the user).
- **Core item "content strategy findings section is populated with
  actions outside the tracking tool":** these are the non-Peec actions
  surfaced during intake.

### §15.2 — Peec implementation

- **Core item "write verification (§12.5) completed cleanly":** §12.5
  verification on Peec is the `list_*` reconciliation — for each entity
  type, a fresh `list_prompts` / `list_brands` / `list_topics` /
  `list_tags` with `limit=10000`, checked against the §12.1 prediction
  `expected_final = initial − deleted + created`; a spot-check of 5–10
  random prompts for `tag_ids`, `topic_id`, and `country_code`; a brand
  list spot-check for `domains` / `aliases` / `regex`; and a fresh
  aggregate `get_brand_report` as the post-write baseline. A count
  mismatch signals a missed or duplicated write and is replayed, not
  noted.
- **Core item "detection-pattern spot-check (§13.1) is ready to
  run":** the ~5 detected / ~5 not-detected sample is drawn from Peec's
  chat reports (the chat-level surfaces of §13.1 / §13.8) and the
  response text read directly. A systematic miss is fixed with the
  `update_brand` change as a Wave 1 write while qualitative analysis
  proceeds; quantitative analyses re-run after Peec's recalc completes
  (~24h).
- **Core item "the platform's recommendation / action feed has been
  pulled for the loop's window":** `get_actions(scope=overview)` has
  been called for the loop's window, with high-opportunity slices
  (`opportunity_score ≥ 0.05`, or `gap_percentage ≥ 0.7` regardless of
  score) drilled into (§13.17.1).
- **Core item "each platform-generated recommendation has been run
  through the critical-filter step":** each `get_actions`
  recommendation has been run through the critical-filter step with
  signal / action / review-outcome recorded separately (§13.17.2) — the
  data columns are trusted, the `text` column's action framing is not.
- **Core item "every editorial / comparison target … has been
  legitimacy-checked":** every `EDITORIAL`/`COMPARISON` target surfaced
  by `get_actions` in a commercial vertical has been legitimacy-checked
  via the WebFetch → Claude-in-Chrome fallback chain (§13.17.3), with
  action-shape routed to PR (genuine editorial) or network-join
  (affiliate-driven) accordingly (§13.17.4).

### §15.3 — Peec implementation

- **Core item "every finding traces back to a platform data pull":**
  every finding included in Phase B traces back to a Peec tool call
  recorded in Phase A findings (§3.2 provenance) — tool name,
  parameters, and window.
- **Core item "labels match across the platform configuration, the
  Strategy sign-off, and Phase A findings":** the platform
  configuration here is the live Peec project — tag names from
  `list_tags`, topic names from `list_topics` (§3.6 label travel).
- **Core item "data-surface enumeration table (§14.6) is complete":**
  every Peec surface in the §14.6 list has been touched or explicitly
  marked not-applicable.
- **Core item "every topic-priority claim is backed by at least one
  external source":** `list_prompts.volume` alone is not sufficient
  backing (§14.10; and see §13.18 on its low discrimination in
  regulated verticals).
- **Core item "any vendor-tool review slide has at least one KEEP
  callout":** on Peec this is the KEEP/CUT review of `get_actions`
  output (§14.12).
- **Core item "closing slide is tool-independent":** it survives a
  decision not to renew Peec (§14.13).
- **Core item "no share metric is computed by adding per-brand
  percentages":** any "group" / "family" / "combined" figure was
  produced by a single Peec query with `brand_id IN (…)` (or by manual
  chat-ID set union) and labelled with that composition method (§14.3
  group-as-rows rule).
- **Core item "no time-series chart appears in a window during which
  the tracked prompt set changed":** the prompt-set changes that break
  a window are `create_prompt`, prompt-text `update_prompt`, and
  `delete_prompt` operations (§13.7 stable-cohort gate).
- **Core item "no internal artifact references on stakeholder
  slides":** the Peec identifiers that must not appear are project IDs
  (`or_…`), prompt IDs (`pr_…`), brand IDs (`kw_…`), and tag IDs
  (`tg_…`), alongside the platform-agnostic list (findings file paths,
  session IDs, workspace paths).

### §15.4 — Peec implementation

- **Core gate 1 "Counts":** the approved allocation is checked against
  the write prediction of §12.1 before the write and the `list_*`
  reconciliation of §12.5 after it; per-topic counts come from
  `list_prompts` grouped by `topic_id`, per-market counts by
  `country_code`.
- **Core gate 3 "Tag conformance":** the agreed vocabulary is the tag
  set returned by `list_tags` after the tag wave; `tag_ids` on every
  prompt must resolve to it (§9.4, §12.2 wave order).
- **Core gate 5 "Placement":** on Peec the brand-mention split is
  tag-based and a branded prompt can live under any topic (§9.7) — so
  the placement check is the second form of the core gate: no topic
  from `list_topics` is named after the own brand or a tracked
  competitor (a brand-name topic is a roster-classification defect,
  §9.3 / §9.5; the assortment-brand exception uses `brand:<name>`
  tags, not topics), and every prompt's brand-mention tag in
  `tag_ids` matches who is named in its text (core gate 4).
- **Core gate 10 "Competitor roster":** the roster is the brand list
  from `list_brands`; uniqueness of names and of every entry in
  `domains` is checked there, and the own brand is the project's
  configured own brand, not a competitor row (§9.3).
- **Core gate 11 "Format conforms to the platform's import
  contract":** Peec has no file import in this workflow — the
  "format" is the `create_prompts` / `create_brands` / `create_topics`
  / `create_tags` payload shape executed in §12 wave order (tags and
  topics before prompts, so that `tag_ids` and `topic_id` resolve).
  Read §12 in this companion before executing any write. The
  "unconfirmed import assumption" items of the core pre-flight do not
  apply; the §12.1 freshness check (`list_*` immediately before the
  write) takes their place.

---
