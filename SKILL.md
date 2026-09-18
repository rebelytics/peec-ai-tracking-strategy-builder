---
name: peec-ai-tracking-strategy-builder
description: "Peec AI companion to the platform-agnostic `ai-visibility-tracking-strategy-builder` skill — build or refine a Peec AI tracking strategy as an iterative workflow (Intake → Strategy → Write → Analyse, plus an optional stakeholder presentation), with this skill holding everything Peec-specific: MCP reads and gaps, brand/topic/tag fields, plan-tier engine gating, write waves and verification, Analyse recipes over Peec reports. Load whenever the user mentions Peec setup, Peec onboarding, \"what should we track in Peec\", Peec prompt rationalisation, or Peec rebuild; also on Phase B / retrospective requests about an existing Peec strategy (\"explain our Peec setup\", \"document our tracking strategy\"). Requires the core skill `ai-visibility-tracking-strategy-builder` loaded alongside; companion to `peec-ai-mcp` (recommended; tool mechanics)."
version: 3.1.0
license: CC-BY-4.0
origin: https://github.com/rebelytics/peec-ai-tracking-strategy-builder
maintainer: Eoghan Henn / rebelytics (eoghan@rebelytics.com)
---

# Peec AI Tracking Strategy Builder

**Created by Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)**

The **Peec AI companion** to
[`ai-visibility-tracking-strategy-builder`](https://github.com/rebelytics/ai-visibility-tracking-strategy-builder)
— the platform-agnostic core that holds the methodology: the intake
rings, the allocation methods, the cardinality rule for topics versus
tags, the three-way brand-mention split, the prompt disposition
framework, the pattern library, the Phase B stakeholder rules and the
quality gates. This skill holds what is true of **Peec** specifically:
which MCP reads populate intake and which fields the MCP does not expose,
how Peec's brands, topics, tags and `country_code` map onto the core's
containers, plan-tier engine gating, the write waves and verification
recipe, the Analyse recipes over Peec's reports, and the patterns that
exist only because of a Peec mechanism.

**Load both.** Every section here is a "§N — Peec implementation" part of
a core section with the same number. Read the core section first, then
this skill's part. Running this skill without the core produces Peec
mechanics with no strategy behind them.

> This skill is a **living document**. If you run it and discover a pattern
> that isn't captured, open an issue on the repo. See the contributing-back
> section at the end of this file.

## Licence

Released under the
[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)
licence. You are free to share and adapt this skill for any purpose,
provided you give appropriate credit to the original author.

## Feedback & Support

If you run this skill and find methodology gaps, gotchas, or patterns
worth contributing, open an issue on the
[GitHub repository](https://github.com/rebelytics/peec-ai-tracking-strategy-builder).
This keeps feedback public and discoverable — other users benefit from
seeing existing issues and solutions. For direct contact, reach the
skill's creator, Eoghan Henn, via [rebelytics.com](https://www.rebelytics.com).
Methodology findings that are not Peec-specific belong on the core skill's
repository instead.

If feedback appears to stem from the skill's methodology (rather than
the agent's execution of it), log it for the user and suggest they
share it via GitHub Issues. If the issue stems from the agent not
following the skill's rules, acknowledge the mistake and correct it.

---

## 1. What this skill is (and isn't)

**Is:** The Peec half of a two-skill workflow that takes a brand from
"I've just connected the Peec MCP" — or "our Peec project's structure no
longer fits our current strategy" — to a measured, well-structured
tracking strategy. The workflow, its loop (Intake → Strategy → Write →
Analyse), its deliverables and its gates are defined in the core skill
(§1, §6, §16 there); this skill supplies the Peec mechanics each phase
needs.

**Two classes of deliverable** (core §1): the configured Peec project
itself — brands, topics, tags and prompts written directly via the Peec
MCP as Phase A loops produce them — and the optional Phase B stakeholder
presentation.

**Isn't:** A Peec MCP tool reference. Tool mechanics, schema quirks, and
setup instructions live in the companion [`peec-ai-mcp`](https://github.com/rebelytics/peec-ai-mcp)
skill. Strongly recommended to load alongside; see §5.

**Isn't:** A content strategy skill (core §1).

---

## 2. When this skill fires

Trigger when the user wants to set up, rationalise, review or present a
tracking strategy **on Peec** — the trigger list in core §2 applies, with
"the platform" read as Peec. When the platform is not Peec, load the core
and the matching companion instead.

The skill works with any project state: zero prompts, 50 prompts still
finding shape, or several hundred prompts that have grown organically.
The workflow routes automatically based on what it finds (§8.1, project
state classification).

---

## 3. Cross-cutting principles — Peec notes

The principles are core §3. Two Peec facts attach to them:

**§3.1 instrument separation.** Peec has no native branded/non-branded
concept. The standard implementation is **user-defined tags** (`branded`,
`other-brand`, `non-branded`) set at prompt creation, with topic-based
splits as a fallback when all branded prompts live under a single topic.
See §9.7 (this skill) for the tag schema and §13.3 for the arithmetic.

**The no-filter `get_brand_report` blend is the Peec instance of the
core's "unfiltered brand report" hard rule.** A `get_brand_report` call
with no tag / topic filter returns visibility computed across all prompts
in scope. Any roster comparison filters to non-branded — pass the
non-branded tag ID in `filters` (§9.7, §13.3). Never report a no-filter
blend as a competitive headline.

---

## 4. Core principles — Peec implementation

Core §4 holds the principles. These four have Peec-specific mechanics.

### 4.6 Brand list reflects reality — Peec implementation

Peec's auto-selected competitors are usually wrong — they skew toward
information sites (forums, wikis, magazines) rather than actual
commercial rivals. On existing projects, derive the real roster from
`get_domain_report` + `get_url_report`.

When reading these reports to classify domains as own / competitor /
editorial, remember that classification is a **returned column**, not a
server-side filter (see `peec-ai-mcp` §7.31). Filter by `domain` with
the full owned-TLD list (see `peec-ai-mcp` §7.10 and §8.10 steps 1/3/5)
and do any OWN/CORPORATE/EDITORIAL slicing client-side. Column names
also differ between the two reports (`retrievals` on URL, `retrieval_count`
vs `retrieved_percentage` on domain — see `peec-ai-mcp` §7.39); check
the payload before sorting or summarising.

The three-category roster (commercial competitor / assortment brand /
marketplace noise) is core §4.6; §9.3 here carries the Peec fields.

### 4.8 Topics and tags — Peec implementation

Peec's single-valued container is `topic_id` (one per prompt, with an
optional sub-topic level); its multi-valued field is `tag_ids`. The core's
cardinality rule (§4.8, §9.4) therefore reads: `topic_id` carries the
commercial category the budget is allocated against; every overlapping
dimension lives in `tag_ids`. `country_code` carries the market — never
a topic per market.

### 4.9 Model coverage is plan-gated, not freely chosen

On TRIAL and lower-tier plans, Peec caps which engines actively run.
The strategy recommendation must detect plan tier (via active-engine
count from `list_models` with `is_active=true`) and route accordingly:

- **Plan permits engine choice:** pick engines matching audience, rotate
  off inactive ones.
- **Plan caps active engines:** the question flips to *"am I getting
  maximum signal from the engines I do have?"* — mostly a prompt
  quality and brand-detection hygiene question, not an engine
  selection one.

**Run cadence is plan-gated too, and it is the other half of the cost
question.** Peec bills in credits, where **1 prompt × 1 model × 1 day =
1 credit**, and projects run **daily by default**. A weekly cadence costs
roughly a third of the credits — but it is available only on Peec's
larger plan tiers, not on every plan. So the two levers that decide what
a project costs are the **prompt × engine count** (§9.6 above) and the
**cadence**, and only the first is freely available on every plan. Never
present cadence as a simple default choice without naming the plan gate;
§9.6.1 carries the wording and the proposal caution.

§9.6 carries the branches; §9.6.1 the prompt-credit and cadence gap.

### 4.11 Direct writes, no dry-run artifact

The skill writes directly to the Peec project via the MCP rather than
producing an intermediate operations list. The strategy sign-off artefact
— produced before writes begin — is the audit trail. Verification happens
after, using `list_*` reconciliation (§12.5).

### 4.12 Fresh prompts need time — Peec figure

Peec processes new prompts within 24 hours (`peec-ai-mcp` §7.6). The next
Analyse loop can run ~24 hours after writes; 7–14 days for trend-level
analyses. See §12.7.

---

## 5. Relationship to other skills

| Skill | Role |
|---|---|
| [`ai-visibility-tracking-strategy-builder`](https://github.com/rebelytics/ai-visibility-tracking-strategy-builder) | **Core — required.** Holds the methodology this skill implements for Peec. Load it first; every section here is a "Peec implementation" part of one of its sections. |
| [`peec-ai-mcp`](https://github.com/rebelytics/peec-ai-mcp) | **Companion — strongly recommended.** Covers Peec MCP tool mechanics, OAuth setup, schema quirks, gotchas. The Peec MCP has non-obvious quirks that this skill's cross-references (`§7.NN`) depend on: classification vs server-side filtering, column-name inconsistencies across reports, the 24-hour refresh cadence, `update_prompt` partial-update semantics, and more. Running without it works, but those get rediscovered the hard way. |
| External-data skills (varies) | Any skill the user has for pulling search-console data, SEO-tool data, crawl data, trends data, or analytics data. This skill consumes their output; doesn't reinvent them. |
| Brand-context skills (varies) | Any skill the user has loaded that holds accumulated knowledge about the brand being tracked (brand dossier, project-context skill, or similar). Contains prior-captured context (markets, TLDs, revenue profile, regulatory notes) that bypasses intake questions. Always check. |

---

## 6. Workflow overview

Core §6 — Phase A loop, Phase B, routing by project state, pacing
disclosure — applies unchanged. The only Peec facts it needs: project
state is classified from `list_prompts` (§8.1), and the pacing figure is
Peec's 24-hour cycle (§4.12).

---

## 6a. Section map — where to read what

This skill uses progressive disclosure and the family's **global section
numbering**: the core skill's section map resolves every § to a core
file; this map resolves the same § to its Peec part. Read the core file
for a section first, then the Peec file. Every load trigger is
mandatory: running a phase without reading its files first is the same
failure shape as skipping an intake ring (core §3.8).

| File | Sections | Load trigger |
|---|---|---|
| `references/data-persistence.md` | §7 — Peec fields in the intake state | Immediately after core §7, when initialising or resuming the project workspace |
| `references/phase-a-intake.md` | §8.1 Peec MCP reads and known gaps, §8.2 Step A Peec seed-and-harvest | After core `phase-a-intake.md` and before the first Peec MCP read of any Intake step |
| `references/phase-a-strategy.md` | §9.1.1 volume ordinal, §9.2 `country_code`, §9.3 brand fields, §9.4 hygiene mechanics, §9.5 topic operations, §9.6 / §9.6.1 in full, §9.7 tag set, §10 action column | Together with core `phase-a-strategy.md`, before drafting or revising any recommendation and before sign-off |
| `references/pattern-library.md` | §11 — Peec parts, §11.6 in full | When a core pattern's fix points at the companion; always before any write or report call a pattern prescribes; §11.6 whenever `list_models(is_active=true)` returns 3 or fewer engines |
| `references/phase-a-write.md` | §12 in full — pre-flight, wave order, concurrency, seed lifecycle, verification, measurement window | After core §12 principles, before executing any write wave against the Peec project |
| `references/phase-a-analyse.md` | §13 in full — detection spot-check, sweep depth, cohort arithmetic, findings hand-off, maturity tiers, `get_actions` pipeline, shopping queries, deferred-items queue | After core §13 principles, at the start of every Analyse step, before pulling any report |
| `references/phase-b-deliverable.md` | §14 — Peec parts (report calls behind §14.2/14.3/14.6/14.10/14.12/14.15) | Together with core §14 whenever Phase B is triggered, offered or timed |
| `references/quality-gates.md` | §15 — the Peec call or field that verifies each core gate item | Together with core §15 at every phase transition |

---

## 16. Output deliverables

Core §16 applies. The mandatory deliverable is the configured Peec
project — brands, topics, tags and prompts live in Peec as the result of
the Write sub-phase; verification (§12.5) is the closure. Working
artefacts and the optional Phase B deliverable are as in the core.

---

## 17. Contributing back

This skill improves when users feed patterns back. Two paths:

1. **If you already have a skill-improvement mechanism** (like an
   observer layer that captures patterns across skills), let it do
   its job on your local copy — and when it surfaces something that
   isn't user-specific, open a GitHub issue at
   [github.com/rebelytics/peec-ai-tracking-strategy-builder](https://github.com/rebelytics/peec-ai-tracking-strategy-builder)
   so the open-source version captures it too. If the pattern would
   hold on any platform, it belongs on the core skill's repository.

   If you don't have one and want to add one,
   [`one-skill-to-rule-them-all`](https://github.com/rebelytics/one-skill-to-rule-them-all)
   is an open-source observer layer (same author, CC BY 4.0) that
   logs friction and pattern candidates across all your skills as you
   work. Using it with this skill produces the highest-quality
   feedback stream back to the repo.

2. **If you'd rather not install anything**, open an issue directly
   when something lands that would help the next person — a new
   pattern, a gotcha, a workflow refinement, a case where the skill
   steered the wrong way.

Don't fork in-session. Agent-time edits to the skill drift away from
upstream, lose the benefit of other users' patterns, and mean future
sessions load a stale local copy. Feedback → issues → considered
patches → version bump is the path.

---

## 18. Licence & attribution

**Licence:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
Reuse, adapt, redistribute — just keep attribution.

**Attribution:**

> Peec AI Tracking Strategy Builder, maintained by Eoghan Henn
> (www.rebelytics.com),
> github.com/rebelytics/peec-ai-tracking-strategy-builder.

**Not affiliated with Peec AI.** Peec's team has not reviewed or
endorsed this skill. Workflow recommendations here are based on
observed behaviour and may become stale as Peec iterates.

Contributions welcome via GitHub. See `CONTRIBUTING.md` in the repo
root.
