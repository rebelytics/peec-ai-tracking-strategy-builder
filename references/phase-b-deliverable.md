# Phase B — Stakeholder deliverable (§14 — Peec implementation)

Part of the **peec-ai-tracking-strategy-builder** skill (CC BY 4.0 — Eoghan Henn / [rebelytics.com](https://rebelytics.com)). This file holds the Peec-specific part of §14; the platform-agnostic rules are in the core skill `ai-visibility-tracking-strategy-builder`, which must be loaded alongside. Section numbers are global across the skill family.

**Load trigger:** Read together with core §14 whenever Phase B is triggered, offered, or being timed — specifically before the §14.6 data-surface enumeration, before computing any combined-brand figure (§14.2, §14.3), before a topic-priority slide (§14.10), before a `get_actions` review slide (§14.12), and before writing provenance lines in a §14.15 handoff.

**Contents:**

- 14.2 — Peec implementation (combined-brand figure, ratio metrics, prompt-set changes, no-filter blend)
- 14.3 — Peec implementation (engine roster, group-figure query)
- 14.6 — Peec implementation (surface-to-tool mapping for the enumeration table)
- 14.10 — Peec implementation (`list_prompts.volume` is not backing)
- 14.12 — Peec implementation (`get_actions` as the vendor-recommendation surface)
- 14.15 — Peec implementation (provenance notation in the handoff)

All rules, anti-patterns, gates and templates of §14.1–§14.16 live in the core file `references/phase-b-deliverable.md` of `ai-visibility-tracking-strategy-builder`. Nothing below adds a rule; each part only names the Peec tool or field behind a core rule.

---

## §14.2 — Peec implementation

- **Combined-brand figure (rate-summing hard rule).** The only way to
  compute an actual group figure is to re-query the underlying chat
  set with both brand IDs filtered into one call —
  `get_brand_report` with `brand_id IN (…)` — or to union the chat
  ID sets directly. If the stakeholder needs a group figure, run a
  single Peec query with `brand_id IN (…)` (or a manual chat-ID
  union) and label the result as a single combined-brand query, not
  as a sum.
- **Which Peec metrics are ratio metrics** (and therefore cannot be
  summed across brands): see §7.37 in `peec-ai-mcp`. Sentiment and
  position are per-mention aggregates and are exempt.
- **What counts as a prompt-set change** for the trend /
  time-series rule: any `create_prompt`, prompt-text
  `update_prompt`, or `delete_prompt` inside the window changes the
  denominator mid-flight.
- **The no-filter blend.** The "unfiltered brand-report blend" of
  the core rule is the no-filter `get_brand_report` call — it mixes
  branded and non-branded prompts. Always filter to non-branded
  only for roster comparisons (§3.1 worked example).

## §14.3 — Peec implementation

- **Engine roster before contrast copy.** List the active engines on
  the Peec project via `list_projects` (or the roster entry in
  intake state) before drafting any "AI search, not X" slide.
- **Group / family / portfolio rows.** If the stakeholder needs a
  single group figure for a one-row-per-brand chart, query Peec
  with `brand_id IN (…)` (or do a manual chat-ID union) and label
  the result as a single combined-brand query, not as a sum.

## §14.6 — Peec implementation

The core §14.6 enumeration table uses generic surface names. On Peec
each row maps to the following tool; enumerate with the Peec names so
the findings file records exactly which call was made:

| Core surface (§14.6)                                    | Peec surface                                   |
|---------------------------------------------------------|------------------------------------------------|
| Brand report (visibility / SoV / position / sentiment)  | `get_brand_report`                             |
| Domain citation report                                  | `get_domain_report`                            |
| URL citation report                                     | `get_url_report`                               |
| The platform's own recommendations (mandatory §13.17)   | `get_actions` (two-step workflow, §13.17)      |
| Page-content reading of top gap URLs                    | `get_url_content` on top gap URLs              |
| Shopping / product-query surface (ecomm)                | `list_shopping_queries` (§13.12)               |
| Query fanout                                            | Fanout via `list_search_queries` (§13.10)      |
| Per-engine chat reading (§13.14)                        | `list_chats` filtered per engine → `get_chat`  |
| Own-brand URL citation map (§13.15)                     | `get_url_report` filtered to the own domain(s) |

When the user's early-draft feedback is "not enough substance," the
first question is "which Peec surfaces haven't we touched?" — pull
the untouched rows above before drafting.

## §14.10 — Peec implementation

Peec's `list_prompts.volume` ordinals on their own are not sufficient
backing for a topic-priority claim — they collapse to "low" / "very
low" across most commercial topics in regulated verticals and don't
discriminate between priority and noise (§13.18).

## §14.12 — Peec implementation

On Peec, the vendor-recommendation surface reviewed by a KEEP / DROP
slide is `get_actions`. Worked example of a KEEP-column
reinterpretation: Peec said "publish on comparison sites"; the
strategy said "join the affiliate network" (§13.17.4). The
reinterpretation is the consulting layer — surface it visibly.

## §14.15 — Peec implementation

In the handoff document's **data sources (provenance)** item and the
`provenance:` list of the YAML sketch, name the Peec tool call(s)
that produced each number, referenced for audit but not for the
subagent to re-query. Format: `get_brand_report(…) → visibility =
37%`. Never let this notation leak onto a stakeholder slide (core
§14.3, "no internal artifact references").

---
