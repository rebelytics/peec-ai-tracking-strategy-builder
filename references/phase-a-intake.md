# Phase A — Intake, Peec implementation (§8.1, §8.2 Step A)

Part of the **peec-ai-tracking-strategy-builder** skill (CC BY 4.0 — Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)). This file holds the Peec-specific part of §8; the platform-agnostic rules are in the core skill `ai-visibility-tracking-strategy-builder`, which must be loaded alongside. Section numbers are global across the skill family — the section map in `SKILL.md` says where each § lives.

**Load trigger:** Read this file after the core `references/phase-a-intake.md` and before the first Peec MCP read of any Intake step — it says which calls populate Ring 1, which fields the MCP does not expose, and how the Ring 2 fanout harvest is run on Peec.

**Contents:**

- §3.8–3.10 — pointer to the core
- §8 preamble — pointer to the core; Peec-only refresh on subsequent loops; pacing
- §8.1 — Peec implementation
  - Known MCP gaps — check before asking the user
  - Peec MCP reads (always available when this skill runs)
  - Project-state classification
- §8.2 Step A — Peec seed-and-harvest
- §8.2 Steps B–G, §8.3, §8.4, §8.5 — pointer to the core

---

### 3.8–3.10 Intake failure modes, recovery, ring numbering

Core `phase-a-intake.md` §3.8–3.10 — no Peec-specific part. Read them
first; they lead the core file for a reason.

## 8. Phase A — Intake

The goal, the three-ring model, the loop-awareness rule and the pacing
disclosure are in core §8. Two Peec specifics:

**Subsequent loops** typically run a Peec-only refresh — re-reading
`list_brands`, `list_topics`, `list_tags`, `list_prompts`, and the
current-state summary — unless a finding from the previous Analyse (§13)
surfaces a gap that external data could close. In that case, re-enter
Ring 2 or Ring 3 for that specific gap only (core §8).

**Pacing disclosure (core §8, §6):** the default iteration pace is daily
because Peec processes new prompts within 24 hours (see §12.7); quote
that cadence when giving the disclosure.

**Don't conflate the two "dailies".** The 24-hour figure above is how
fast *this workflow* can loop. Peec's **run cadence** is a separate,
billed thing: **1 prompt × 1 model × 1 day = 1 credit**, projects run
daily by default, and a weekly cadence (roughly a third of the credit
use) is available **only on Peec's larger plan tiers**. Whenever the
pacing disclosure touches cost — and it should, because cadence and the
prompt × engine count are the two levers a client conversation turns on
— state the credit formula and name the plan gate rather than implying
cadence is a free choice. §9.6.1 carries the formula, the trade-offs and
the wording for client-facing documents.

### 8.1 — Peec implementation

Core §8.1 says what Ring 1 must capture from the platform and how to
frame asks about fields the platform doesn't expose. This part gives
the Peec calls and the known gaps.

#### Known MCP gaps — check before asking the user

Before asking the user for any information, the agent must attempt to
derive it from the Peec MCP. Several fields that seem like they should
be available are not. The table below enumerates the known gaps so the
agent checks, confirms the gap, and frames the user ask as "the MCP
doesn't expose this" rather than "tell me":

| Field | MCP tool to check | What it returns | Gap |
|---|---|---|---|
| **Project country / market** | `list_projects` | `{id, name, status}` only | No country, language, or market metadata at project level. Ask the user: "Peec's MCP doesn't expose project-level market data, so I need to confirm: which markets should this strategy prioritise?" |
| **Plan tier / prompt credits** | `list_projects` | `{id, name, status}` only | No plan data. `get_credit_balance` does not exist (see `peec-ai-mcp/references/gotchas.md` §7.11 "Write-operation consent, verification, and safe-experimentation patterns"). Ask the user: "Peec's MCP doesn't expose plan credits. How many prompts does your plan allow?" Single precise question, not a multi-tier multiple choice. |
| **Active engine count (proxy for plan gating)** | `list_models(is_active=true)` | Active engine list | ✓ Available. 3 or fewer = likely plan-capped (§9.6). |
| **Run cadence (daily vs weekly)** | — | Not exposed | No cadence field anywhere in the MCP surface. Cadence is a billing setting: daily by default, weekly (about a third of the credit use) only on the larger plan tiers (§9.6.1). Ask it in the same block as the other plan questions: "Peec's MCP doesn't expose your run cadence — is the project running daily, or weekly?" A "weekly" answer also tells you the plan tier is one of the larger ones. |

When asking the user about a known MCP gap, always prefix with the
reason: "The Peec MCP doesn't expose this at the project level, so I
need to confirm with you:" — this reframes the ask from "I didn't check"
to "the platform doesn't expose this" and teaches the user where the gap
lives. Persist every answer to the intake state (§7) so subsequent loops
don't re-ask. Surface plan-related questions (prompt credits + engine
count) together so the user answers all plan constraints in one go.

#### Peec MCP reads (always available when this skill runs)

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

#### Project-state classification

- **New project:** zero prompts, auto-suggested brands still present.
  The Write sub-phase (§12) will be pure creates.
- **Existing project:** prompts already in place. The Write sub-phase
  (§12) will use the disposition framework (§10).

Loaded skills, conversation context and optional connected data MCPs
(GSC, SEO tools, analytics, crawl, community access) are platform-
agnostic — core §8.1.

### 8.2 Step A — Peec seed-and-harvest

Core §8.2 Step A defines the query-fanout harvest (10–15 broad
discovery prompts, read the fanout ~24 hours later, mine platform
patterns, disposition the prompts at Strategy time). On Peec it runs as
follows:

1. **Day 0:** create the discovery prompts via `create_prompts`. Tag
   them `seed:harvest` and set `country_code` from the primary market.
   These prompts join the 24-hour cycle immediately (see `peec-ai-mcp`
   §7.6).
2. **Day 1 (~24 hours later):** for each seed chat, call
   `list_search_queries(chat_id=...)`. This returns the sub-queries the
   AI engine fanned out to when answering — the fanout the core step
   mines.
3. Also call `list_shopping_queries(chat_id=...)` for any shopping-mode
   chats — commercial-intent queries, often product-specific.
4. Platform-pattern mining (`site:reddit.com`, `site:amazon.*`,
   `site:youtube.com`, `site:quora.com`) and the Reddit fanout proxy
   are as in core §8.2 Step A / Step E.
5. **Seed prompt lifecycle:** at Strategy time (§9), decide per seed
   prompt — keep it if it fits the final strategy and the topic
   survives, delete it if not. Seed prompts are not automatically
   preserved; the Write sub-phase (§12) retags survivors from
   `seed:harvest` to production tags.

For existing projects, the seed-and-harvest step is optional — fanout
data can be mined from existing chats (`list_chats` +
`list_search_queries`) instead. Use seed-and-harvest when the existing
prompt set doesn't cover a category the strategy needs to explore
(e.g., the brand is entering a new vertical).

### 8.2 Steps B–G, 8.3 Ring 3, 8.4 Intake summary gate, 8.5 Data-source handling notes

No Peec-specific part — core `phase-a-intake.md`. The §8.4 intake
summary block is mandatory before Strategy on every platform, Peec
included.

---
