# Phase A — Write (§12)

Part of the **peec-ai-tracking-strategy-builder** skill (CC BY 4.0 — Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)). Section numbers are global across `SKILL.md` and `references/` — the section map in `SKILL.md` says where each § lives.

**Load trigger:** Read before executing any write wave against the Peec project.

---

## 12. Phase A — Write

Goal: turn the signed-off Strategy output into direct writes to the
Peec project. No intermediate operations artifact — the Strategy
sign-off (§9.8) is the audit trail.

### 12.1 Pre-flight

Before executing:

1. **User sign-off on the Strategy output.** The sign-off artefact
   (§9.8) must be in place — whatever format it took — before any
   write runs. Capture the sign-off timestamp in the persisted intake
   state.
2. **Freshness check.** Re-read `list_brands`, `list_tags`,
   `list_topics`, `list_prompts` immediately before the Write sub-phase
   to catch any drift since Intake.
3. **Count reconciliation preview.** For each entity type, predict
   `expected_final = initial − deleted + created`. Print the prediction.

### 12.2 Wave execution order

Execute in this order. Reversing the order leaves dangling references.
See `peec-ai-mcp` §6.5 for the wave pattern with concurrent batching.

1. **Create new tags.** Tags must exist before they can be applied.
2. **Create new brands.** Before deleting old brands, so domain report
   continuity is preserved.
3. **Update existing own brand** (add domains, aliases, regex).
   Triggers background recalculation — batch all fields into one call
   (see `peec-ai-mcp` §7.19).
4. **Delete old brands** (auto-selected non-commercial ones).
5. **Create or update topics.** Use `update_topic` in place where
   possible; otherwise create-new + migrate-prompts + delete-old.
6. **Update existing prompts.** Add tags via `update_prompt.tag_ids`
   (full replacement — see `peec-ai-mcp` §7.14; fetch current tags
   and merge before writing). Move topics via
   `update_prompt.topic_id`.
7. **Delete outgoing prompts.** Soft-deletes cascade to chat history.
   Do this after all retagging is confirmed. Reframe-deletes (§10)
   run in this wave; matching creates run in step 8.
8. **Create new prompts.** Each with `tag_ids` and `topic_id` set on
   creation (avoid round-trip to tag after creation). Set
   `country_code` explicitly — required (see `peec-ai-mcp` §7.15).
   Reframe recreations appear in this wave paired with their step-7
   deletes.

### 12.3 Intra-wave concurrency

Within a wave, batch independent writes in parallel — 5–10 concurrent
calls is the sweet spot (see `peec-ai-mcp` §6.5). Peec's published
rate limit is 200 requests/minute per project; 5–10 concurrent calls
translate to ~3–5 req/sec, well under the ceiling.

**Pre-fetch tag and topic IDs once per wave.** Before the prompt-create
wave, cache the `list_tags` and `list_topics` ID maps. Entities don't
mutate mid-wave.

**Pause between waves, not between individual operations.** Serial
pauses between `delete_*` calls are unnecessary. Pause between waves
to verify state.

### 12.4 Seed prompt lifecycle

Seed prompts created during Ring 2 Step A (§8.2):

- **Keep** any seed prompt that fits the final strategy (aligns with a
  kept topic, carries a meaningful tag, has non-zero visibility worth
  preserving). Retag from `seed:harvest` to production tags.
- **Delete** any seed prompt that doesn't fit. It served its intake
  purpose; delete cost is 0 beyond plan credits already spent on the
  24-hour harvest.

Record seed disposition in the Strategy sign-off so the credit spend
is visible to the user.

### 12.5 Verification

After the last wave completes:

1. **Arithmetic count reconciliation** against the Write prediction
   (§12.1). For each entity type, fetch a fresh `list_*` with
   `limit=10000` and verify `actual_final == expected_final`.
   Mismatches signal a missed or duplicated write.
2. **Spot-check 5–10 random prompts** — verify tags, topic_id, and
   country_code match the strategy.
3. **Brand list spot-check** — new brands present with correct
   `domains`/`aliases`/`regex`; old brands absent.
4. **Fresh aggregate brand report** — establishes the post-write
   baseline for the next Analyse loop.

### 12.6 Verification output

Verification results are persisted so the next Analyse loop can
reference them. One concrete format (illustrative — format is agent/user
choice per §3.7):

```
# [Brand] Peec Strategy — Post-Write Verification, Loop N
Date | Project | Strategy sign-off reference

1. Count reconciliation (table: entity, initial, delta_predicted, delta_actual, match)
2. Prompt spot-check (table: prompt_id, expected tags+topic, actual tags+topic, match)
3. Brand list spot-check
4. Baseline brand report (summary, full data attached)
5. Anomalies and follow-ups
```

If anomalies exist, link back to the specific wave and operation. A
clean reconciliation closes the Write sub-phase; a mismatch triggers a
replay of the missing operations against the recorded strategy.

### 12.7 Measurement window

Fresh prompts start collecting data within 24 hours (see `peec-ai-mcp`
§7.6).

- **Default pace: daily iterations.** After writing prompts, the next
  Analyse loop can run ~24 hours later once Peec has processed them.
  This is the normal working rhythm — don't introduce multi-day gaps
  unless a specific analytical need calls for it. Before 24 hours,
  newly written prompts haven't necessarily produced chats yet; after
  24 hours, the data is available.
- **7–14 day windows for trend-level analyses.** For cohort-level
  questions (SoV shifts, sentiment drift, baseline comparisons), a
  longer window produces more stable signal and reduces noise from the
  first 48 hours. Use these windows when the question requires it, not
  as a default gate on every loop.
- **Detection-pattern spot-checks (§13.1)** and hygiene questions need
  only 24 hours of data — they're checking configuration, not
  measuring trends.
- Comparisons against pre-write baselines should use matching window
  lengths (7-day post-write vs 7-day pre-write, not full history).

The principle is §3.4 — calendar time is a resource, and analyses
should be paced around signal maturity rather than an arbitrary
schedule. But signal maturity for most operational questions is 24
hours, not weeks. The agent and user pick the cadence that fits; the
skill doesn't bake in a prescriptive 30-day or 90-day schedule.

### 12.8 Hand-off to Analyse

Every Write sub-phase must hand off enough state to the next Analyse
loop (§13) that the loop can resume without re-deriving context. The
mechanism is agent/user choice (§3.7); the content is required:

- **Write-wave summary:** which entities changed, counts, any anomalies
  from verification.
- **Earliest re-analysis date** (per §12.7 — typically tomorrow for
  operational questions, longer for trend-level analyses).
- **Pending verification items:** anything that didn't reconcile
  cleanly and needs Analyse-time attention.
- **Findings file location (if used):** where the Analyse output from
  the previous loop lives, and where the new one will go.
- **Cohort maturity map:** for each prompt cohort, when it was written
  and how old the signal is.

In environments with persistence (§7), this lives in the intake state
or a dated findings-handover file. In environments without persistence
(web chat), this becomes a dedicated section of the session-end handoff
doc so the next session can pick up the loop.

---
