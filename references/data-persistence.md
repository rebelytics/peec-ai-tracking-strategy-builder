# Data persistence — Peec implementation (§7)

Part of the **peec-ai-tracking-strategy-builder** skill (CC BY 4.0 — Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)). This file holds the Peec-specific part of §7; the platform-agnostic rules are in the core skill `ai-visibility-tracking-strategy-builder`, which must be loaded alongside. Section numbers are global across the skill family.

**Load trigger:** Read immediately after the core skill's §7 file, when initialising or resuming a Peec project workspace and before writing any loop artefact.

---

## §7 — Peec implementation

The core §7 schema is complete except for the `platform:` block. For a
Peec project that block holds the identifiers a loop needs to resume
against the same project without re-discovery. Persist them once, at
the end of Intake, and read them back at the start of every loop.

### 7.2 — Peec implementation: the `platform:` block

```yaml
platform:
  name: peec
  project_id: <id>                 # from list_projects; every read/write
                                   # call is scoped to it
  brand_mention_tags:              # Peec tag IDs carrying the §9.7 split;
    branded: <tag_id>              # the §13.3 tag-filter recipe reads these
    other_brand: <tag_id>          # omit if the project runs the two-cohort
    non_branded: <tag_id>          # split (§4.10)
  plan:
    prompt_credits: <n>            # asked from the user (§9.6.1) — the MCP
                                   # does not expose plan data and
                                   # get_credit_balance does not exist
                                   # (peec-ai-mcp/references/gotchas.md §7.11
                                   #  "Write-operation consent, verification,
                                   #  and safe-experimentation patterns")
    engines_capped: true|false     # §9.6 — 3 or fewer active engines
                                   # most likely means a gated plan
    run_cadence: daily|weekly      # asked from the user (§9.6.1) — no MCP
                                   # field exposes it. Daily is the
                                   # default; weekly is gated to the
                                   # larger plan tiers, so "weekly" also
                                   # implies an ungated plan
  active_engines:                  # from list_models(project_id,
    - <model_id>                   #   is_active=true) at last Intake;
    - <model_id>                   # re-read at every Intake, not trusted
                                   # across loops
  own_brand_id: <brand_id>         # the is_own brand; update_brand target
                                   # for alias/domain/regex fixes
```

Field notes:

- **`project_id`** — mandatory. Without it the next loop has to call
  `list_projects` and ask the user which project is meant.
- **`brand_mention_tags`** — mandatory once the §9.7 tags exist. These
  IDs are what make the preferred §13.3 cohort recipe
  (`get_brand_report` with `filters=[{tag_id: …}]`) callable without a
  `list_tags` round-trip. Tag IDs are stable; tag *names* are not a
  safe key because they can be renamed.
- **`plan.prompt_credits`** — asked once, persisted so subsequent loops
  don't re-ask (§9.6.1). Update only when the user reports a plan
  change.
- **`plan.run_cadence`** — asked in the same block as
  `prompt_credits`, and persisted for the same reason. It belongs in the
  state because credit spend is prompts × engines × run-days: without
  the cadence, a prompt allowance cannot be converted into an allocation,
  and any cost figure in a deliverable is a guess (§9.6.1).
- **`active_engines`** — the engine set is a plan constraint (§9.6
  Branch B), so it is persisted as a *record of what was seen*, not a
  cached truth. Re-read `list_models` at every Intake; a diff against
  the persisted list is itself a finding (plan change, new engine
  enabled).
- **`country_code`** — the core schema's `markets[].country` maps
  directly onto Peec's per-prompt `country_code` field. Persist the
  market list once in the core block; do not duplicate it here.

### 7.1 — Peec implementation: folder name

The core layout's `tracking-strategy/` folder was historically named
`peec-strategy/` in this skill. Existing workspaces keep whichever name
they have; do not rename a folder that prior loops' hand-off docs point
at.

### 7.4 — Peec implementation: handoff-doc additions

In non-persistent environments the session-end handoff doc must carry
the whole `platform:` block above verbatim — `project_id` and the three
tag IDs in particular, because a session that starts without them
cannot run the §13.3 tag-filter recipe and will silently fall back to
arithmetic subtraction, which cannot produce the `other-brand` line.

---
