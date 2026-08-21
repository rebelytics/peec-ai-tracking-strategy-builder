# Phase A — Analyse (§13)

Part of the **peec-ai-tracking-strategy-builder** skill (CC BY 4.0 — Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)). Section numbers are global across `SKILL.md` and `references/` — the section map in `SKILL.md` says where each § lives.

**Load trigger:** Read at the start of every Analyse step, before pulling any report.

**Contents:**

- 13.1 Pre-flight — detection-pattern spot-check (quality gate)
- 13.2 Flexible sweep depth
- 13.3 Branded vs non-branded findings (cohort separation)
- 13.4 Empty-response chat handling
- 13.5 Sentiment outlier check
- 13.6 Findings hand-off
- 13.7 Analyse-phase maturity tiers (quantitative vs qualitative)
- 13.7.1 Cross-page verification before attributing content elements as "weakness"
- 13.8 Qualitative chat reading is peer analysis, not detection verification
- 13.9 URL-content reading converts gap URLs from leads into briefs
- 13.10 Fanout mining is a continuous refinement signal, not an intake-only tool
- 13.11 ChatGPT parametric behaviour in regulated verticals is a strategic pattern
- 13.12 Shopping queries (`list_shopping_queries`) — e-commerce only
- 13.13 URL/domain gap reports also surface brand roster candidates
- 13.14 Per-engine visibility variance requires per-engine chat reading
- 13.15 Own-brand URL citation map reveals content authority hot/cold spots
- 13.16 Deferred-items queue (mandatory hand-off to next loop)
- 13.17 `get_actions` — mandatory surface + critical-filter pipeline
- 13.18 `list_prompts.volume` in regulated verticals — low-discrimination caveat

---

## 13. Phase A — Analyse

Goal: close the loop on the Write sub-phase. Read what the Peec data
actually says, produce findings, feed them back into the next Strategy
iteration. The number and depth of analyses is not hardcoded — the agent
assesses what's needed each loop based on current priorities, data
maturity, and user input.

### 13.1 Pre-flight — detection-pattern spot-check (quality gate)

Before running any analysis, verify that brand detection is working.
This is a gate, not a peer analysis.

Procedure:

1. Sample ~5 chats where the own brand was detected, ~5 where it wasn't.
2. Read the actual response text.
3. For each false positive or negative, classify as:
   - **Systematic:** a pattern (all Spanish-language mentions missed,
     all initialised-brand aliases missed, regex misfiring on a
     sub-word match).
   - **One-off:** a genuine edge case, unusual phrasing.
4. Decision on systematic miss — **a gate blocks the analyses that
   depend on what it measures, not the whole phase**:
   - **Stop (blocked by broken detection):** quantitative cohort
     analyses (§13.3 branded vs non-branded findings), SoV and
     position computations, sentiment outlier checks (§13.5),
     gap-to-close cohort status — all depend on detection accuracy.
   - **Continue (not blocked):** qualitative chat content analysis
     (§13.8), fanout mining (§13.10), shopping-query audit (§13.12),
     domain/URL gap reports (§13.13 — gap data is text-and-URL based
     and independent of own-brand detection), `get_url_content`
     competitive content reads (§13.9).
   - **Fix in parallel:** apply the `update_brand` fix as a Wave 1
     write while qualitative analysis proceeds; re-run quantitative
     analyses after recalc completes (~24h).
   - **Only one-off misses** → continue all analyses; log the
     measurement-confidence floor in findings.

A blanket "stop" wastes analytical capacity on the unaffected half
of the surface area. Enumerate dependencies precisely so the agent
knows what can still be trusted and what can't.

No hard numeric threshold. Pattern classification is the discipline —
not rate calculation on tiny samples.

### 13.2 Flexible sweep depth

The analyses below are **core analyses** — not an exhaustive menu.
Each loop should also consider whether any data surface in the
`peec-ai-mcp` companion skill (its §7 gotchas and §8 recipes) would be
informative given this loop's questions. Treat §13.2 as "what's in
scope by default" and the peec-ai-mcp surface map as "what's
available if this loop's questions warrant it."

**Core analyses (pick subset per loop):**

- Branded cohort: visibility, SoV, sentiment, position (separated from
  non-branded per §3.1).
- Non-branded cohort: same metrics, as a separate instrument.
- Per-engine breakdown (leverages per-engine retrieval personality —
  see §11.11).
- Per-topic breakdown (health check for cohort size and signal).
- Source-authority audit (domain + URL reports for retrieval and
  citation gaps).
- **Peec-scored opportunities via `get_actions` (mandatory, §13.17).**
  Every Analyse loop must run `get_actions(scope=overview)` against
  the same window the loop is analysing, then drill into
  high-opportunity slices. Independent algorithmic lens that
  cross-checks the agent's manual mining.
- Sentiment outlier check (first-loop only on existing projects, or
  on-demand when a meaningful drop appears — see §13.5).
- Gap-to-close cohort status (blind-spot prompts).

**Extended analyses (consider each loop; surface area listed here
isn't exhaustive either):**

- Qualitative chat reading (§13.8) — what the AI actually says, not
  just whether the brand is detected.
- Fanout mining (§13.10) — continuous refinement signal for the
  prompt set.
- `get_url_content` on top gap URLs (§13.9) — converts gap URLs from
  leads into briefs.
- Shopping queries (§13.12) — e-commerce only.
- URL/domain gap reports as roster-candidate source (§13.13).
- Per-engine chat reading (§13.14) when per-engine visibility variance
  is material.
- Own-brand URL citation map (§13.15) — content-authority hot/cold
  spots across own domain.

**Untapped-surface check (each loop):** before closing the sweep,
ask: "which peec-ai-mcp surfaces haven't I touched this loop, and
would any of them answer a question the core analyses can't?" The
goal is to catch agents who default to the literal core list and
miss strategically valuable qualitative surfaces. Listed enumeration
shapes behaviour — without an explicit prompt to look wider, agents
stop at the core menu.

**Category-ranking-dominated verticals — per-prompt drill-down is
core, not extended.** For verticals where a small number of
authoritative third-party category rankings dominate what the AI
surfaces ("who's good at X"), topic-level averages systematically
hide the mirror pattern described in §11 (see *category-ranking
mirror* below). Treat `get_brand_report(dimensions=[prompt_id])`
on every INVEST or borderline topic as a **core analysis** in these
verticals, not an extended one. Verticals where this is the default
include:

- **Legal services** — Chambers & Partners (global + regional
  editions), Legal 500, Best Lawyers, Who's Who Legal, industry
  jurisdiction-specific rankings.
- **Management consulting / strategy** — industry awards, Vault,
  Gartner-style positioning reports where they exist.
- **Enterprise software / SaaS** — Gartner Magic Quadrant, Forrester
  Wave, G2 category leaders, IDC MarketScape.
- **Higher education** — US News, Times Higher Education, QS rankings.
- **Certification bodies / standards** — named ISO/IEC accreditation,
  major named awards or "Stars of the Industry"-style national
  rankings.
- **Financial services (fund management, wealth)** — Morningstar-style
  category ratings, industry awards.
- **Healthcare (hospitals, practice groups)** — US News Best Hospitals
  / Best Lawyers-style regional rankings.

The pattern to expect: for every topic where the brand holds a top-
tier category ranking in a niche sub-area *and* lacks one in the
broader category, topic average will obscure a dramatic high-on-
niche / zero-on-broad split. Report per-prompt before you report
topic average. See §11 *category-ranking mirror* pattern and §14.2
anti-pattern "lead with topic average, not the flattering niche
stat".

Non-category-ranking verticals (e-commerce, most B2C consumer goods,
most direct-response services) don't need this default promotion —
per-prompt drill-down stays in the Extended analyses list.

### 13.3 Branded vs non-branded findings (cohort separation)

Produce two parallel findings sections — one per cohort. For each,
legitimate findings include:

*Branded cohort legitimate findings:*
- Coverage gaps in branded queries (engine where own brand isn't named
  even with brand in prompt).
- Sentiment drift on branded prompts (reputation monitoring signal).
- Detection failures (prompt mentions brand but Peec didn't count it).

*Non-branded cohort legitimate findings:*
- Visibility share vs tracked competitors (the real competitive signal).
- Gap-to-close progress (which blind spots are closing).
- Per-engine disparities (engines where the brand underperforms).
- Source-retrieval patterns (which own-domain URLs are cited, which
  are retrieved-but-not-cited).

**Forbidden ("tautological") findings:**
- "Brand leads on branded prompts" — branded prompts score ~100% by
  construction.
- Any finding that aggregates branded and non-branded into a single
  number without disclosure (§3.1, §9.7).

**Computation recipe — how to derive the non-branded cohort when Peec
doesn't filter it natively.** In order of cleanness:

1. **Tag filter (preferred).** If the project carries `branded` /
   `non-branded` tags (§9.7), call `get_brand_report` with
   `filters=[{tag_id: <non-branded-tag-id>}]`. Cleanest and most
   resilient — survives topic restructuring.
2. **Topic filter (fallback).** If all branded prompts live under a
   single dedicated topic, filter by `topic_id` excluding that topic.
   Works but brittle: a single branded prompt drifting into another
   topic breaks the math silently.
3. **Arithmetic subtraction (always available).** Compute
   `non_branded_visible = total_visible - branded_visible`, and
   `non_branded_chats = total_chats - branded_chats`, then
   `non_branded_visibility = non_branded_visible / non_branded_chats`.
   Uses only the total and branded-cohort subtotals from two separate
   report calls. Works even when tags and topics aren't organised for
   filtering, and is the fallback when the first two approaches aren't
   available.

**Cross-tool calibration (first Peec Analyse after an external-tool
strategy).** When Loop 1 is the first Peec data following a strategy
built from external baselines (another AI-visibility platform, a
general-purpose SEO tool's AI-visibility module, or any competitive
tool with an AI-visibility surface), include a **baseline
calibration** step as part of Analyse. Compare the external metric
against the equivalent Peec metric topic-by-topic. Any divergence >15 percentage
points is a calibration signal — flag the topic for posture review.
Cross-tool metrics rarely agree closely: different prompt universes,
different engine coverage, and different detection methods produce
materially different numbers for the same brand. Treat the calibration
step as posture-validation, not data-quality debugging.

### 13.4 Empty-response chat handling

Some chats come back with no meaningful response (engine refused, timed
out, returned generic "I can't help"). Handle them two ways:

- **Filter out** empty-response chats when computing headline visibility
  — treat as missing data, not negative data.
- **Report** the empty-response rate as a separate metric, so the user
  sees it.

Regulated verticals (cannabis, pharma, gambling, adult) are especially prone
to engine refusals. This is a structural reality of the niche, not a
detection problem. Distinguishing empty-response from brand-not-mentioned
in the Analyse output prevents the visibility math from silently
punishing the brand for an engine refusal.

### 13.5 Sentiment outlier check

- **First-loop on existing projects:** always run. Pull bottom-decile
  sentiment chats, read the text, classify each as real negative
  coverage / detection artefact / sentiment-model artefact. Calibrates
  the sentiment signal for this brand.
- **Subsequent loops:** on-demand only. Trigger when a sentiment drop
  of meaningful size shows up vs prior loop.

The first-loop calibration matters because sentiment is the metric most
likely to mislead without context — a bottom-decile chat that's actually
a legal-caution framing (§11.7) looks like a reputation problem
until the text is read.

### 13.6 Findings hand-off

Findings feed the next Strategy iteration. Format is agent/user choice
(§3.7) — a markdown findings file, a structured chat message, an
update to the intake state, whatever fits.

What's required, regardless of format:

- Each finding traceable to a Peec tool call (§3.2 provenance).
- Separate branded and non-branded sections (§3.1).
- Concrete implications for the next Strategy iteration — what would
  change, what needs more data, what can be closed out.
- **Strategy proposals section (mandatory).** After writing the
  findings, enumerate the specific Peec MCP writes the findings
  justify. The user should not have to ask "now translate this into
  actions." Three buckets:
    1. **Low-risk additive writes proposed for immediate execution**
       — new tags, new competitor brands, tag applications, new
       topics, added prompts. Each item includes the rationale that
       justifies it (which finding, which tool call, which threshold).
       Framing: "Ready to execute these now?"
    2. **Higher-risk writes proposed with a decision gate** — prompt
       reframes, deletions, topic restructuring, roster removals.
       Each item includes the evidence threshold that would trigger
       execution (e.g., "wait 7 days; execute if visibility stays
       <20%", "re-check at Loop N+1 after data accumulates").
    3. **Writes explicitly considered but NOT recommended** — with
       reasoning. Surfacing the negative space matters: the user sees
       what was weighed and why it was ruled out, which calibrates
       trust in the positive proposals.

  The agent closes the Analyse → Strategy loop transition by
  actively proposing, not by leaving the user to translate findings
  into actions. A reporter stops at "here's what the data shows"; a
  strategist produces "here's what we should do about it, and here's
  what we shouldn't."

**Execute-now handoff (mandatory).** Writing a proposal into the
findings file is *not* the same as surfacing it to the user as a
pending decision. At the end of any Analyse loop that produces one
or more items in bucket 1 (low-risk additive writes proposed for
immediate execution), the agent's handoff message MUST close with an
explicit yes/no execution offer. The offer names each ready-to-
execute item inline — the user should not have to open the findings
file to know what's being asked.

Required shape:

> *"Two writes are ready to execute now: (1) add `Competitor Inc.` to
> the competitor roster as `CORPORATE`; (2) apply `branded` tag to
> prompt IDs 4821, 4822. Shall I run both now, or hold for the next
> loop?"*

For loops that also have decision-gated writes (bucket 2) or deferred
items (§13.16), surface those separately — they are context, not part
of the execute-now ask:

> *"Two writes are ready to execute now: …. Shall I run both now, or
> hold? (Separately: three prompt reframes are decision-gated — see
> findings section X; two items are deferred pending day-7 read — see
> §13.16 queue.)"*

Rules:

- If bucket 1 is empty, omit the ask entirely — no need to theatrically
  confirm there's nothing to execute.
- If the user is absent (scheduled run, batch job, non-interactive
  environment), default posture: **do not execute autonomously**.
  Flag the ready-to-execute bucket clearly in the deliverable message
  and wait for the next interactive turn.
- Closing an Analyse loop without asking the execute-now question on a
  non-empty bucket 1 is equivalent to not closing the loop. The loop
  is not complete until the items are either executed or explicitly
  declined.

**Mid-loop additive writes pattern.** When the executed items in
bucket 1 are strictly additive and low-risk (new `create_brand`,
`create_tag`, domain additions, tag applications — not deletes, not
reframes, not topic restructures), execution **does not require a
fresh Strategy sign-off artefact (§9.8)**. The findings file's
Strategy proposals table already captures the rationale and per-item
justification that the sign-off artefact would repeat. Run the write
wave per §12.2 and capture verification output per §12.6 — the
findings file IS the audit trail for this class of change.

This applies only to the additive/low-risk bucket. Anything that
removes state, reframes prompts, or restructures taxonomy still goes
through the full §9.8 sign-off cycle — the ceremony is calibrated for
reversibility risk, and deletes / reframes are harder to undo. Do
not let "mid-loop additive writes" expand into shortcuts for higher-
risk changes.

**Prompt-set-stability check before any trend / drift finding.**
Before producing any trend, time-series, or drift finding (visibility
over time, position drift, SoV trajectory, sentiment trend, etc.),
confirm that no `create_prompt`, prompt-text `update_prompt` (i.e.
reframe), or `delete_prompt` operations landed inside the measurement
window. The intake state's write-wave timestamps are the source of
truth for this check. If a write wave landed inside the window, the
metric's denominator changed mid-flight — day N's number is computed
against a different prompt cohort than day N+1's, and a continuous
time series visualises a measurement-shift artefact rather than a
real movement. Either truncate the trend to the longest sub-window
where the prompt set was stable, or drop the trend finding and use
snapshot metrics only. Document the dates of the write waves
alongside the finding so a future loop can see the constraint.

**Carry-forward claim re-verification.** Findings files are working
memory, not source-of-truth: a claim that travelled from a previous
loop's findings, a previous session, or a different analyst into the
current findings does not inherit the original analyst's verification.
Any factual claim carried forward from a prior findings file must be
re-verified before it appears in a stakeholder-facing surface (Phase B
deck, written summary, client email). The original analyst's
confidence does not transfer; the new authority context resets the
verification bar. The verification record is added to the new findings
file with date and source.

A practical labelling pattern when consuming a prior findings file:
mark each carried-forward claim as one of (a) re-verified in this
session against fresh evidence; (b) carried forward without
re-verification — to be caveated or dropped before any stakeholder
surface; (c) data-driven and self-verifying because pulled fresh from
Peec in this session (e.g. a current visibility figure). Only (a)
and (c) belong on stakeholder slides. Carrying (b) into a deck
unflagged is a quiet credibility leak — the claim looks right, nobody
catches it, and when a stakeholder asks "is this true?" the chain of
evidence stopped several sessions ago.

### 13.7 Analyse-phase maturity tiers (quantitative vs qualitative)

Analyse maturity has two axes that progress at different rates:
**quantitative** (aggregated metrics — visibility, SoV, per-engine,
per-topic) and **qualitative** (chat reading, URL reading, fanout
inspection). Treat them as separate tiers so an agent doesn't defer
qualitative work until quantitative metrics stabilise.

| Tier | Quantitative threshold | Qualitative threshold | Agent posture |
|---|---|---|---|
| Tier 0 — Seed | < 7 days of data, OR within the first 24h after a major `create_prompt` / `update_prompt` wave (dimensioned reports may return null labels — see `peec-ai-mcp` §7.40) | Sample 3–5 chats total, enough to confirm detection is firing | Don't report metrics as findings; use chat reading to spot-check config |
| Tier 1 — Directional | ≥ 7 days, ≥ 1 chat per prompt per engine | Read 1 chat from the highest-mention-count engine per topic; read top-5 gap URLs | Metrics directional; named findings must be corroborated by chat reading |
| Tier 2 — Analysable | ≥ 30 days, ≥ 10 chats per prompt per engine | Read 1 chat per active engine per priority topic; read all gap URLs with ≥3 competitor presence | Metrics can stand as findings with the usual cohort/separation discipline |
| Tier 3 — Stable | ≥ 60 days, cross-loop trend visible | As Tier 2 plus longitudinal narrative — what moved, what stuck | Trend findings are legitimate; comparative deltas valid |

The tiers are not a gate — Tier 0 doesn't block Tier 1 qualitative work.
Qualitative analysis (chat reading, URL reading) can produce legitimate
findings at any tier, because it's interpreting specific responses, not
aggregating across them. The gate is on quantitative findings only.

**Observable Tier 0 signal.** Earlier versions of this table gated
Tier 0 on `volume_status: QUEUED`. That field isn't exposed on
`list_prompts` (see `peec-ai-mcp` §7.42), so don't rely on it as a
gate. The observable Tier 0 signal is: **time since the last major
write-wave**. If the agent ran a `create_prompt` / `update_prompt`
wave < 24h ago, treat the project as Tier 0 for quantitative findings
even if aggregate chat count looks sufficient. Track the write-wave
timestamp in the persistence store so the next loop can read it
without re-inferring.

**Stable-cohort gate for the trend tier (Tier 3 and any time-series
finding).** Trend findings — visibility, SoV, position, sentiment,
retrieval-share, or any rate plotted day-over-day — require the
tracked prompt set to have been **unchanged for the full measurement
window plus a 24-hour buffer before the start of the window**. If a
`create_prompt`, prompt-text `update_prompt`, or `delete_prompt`
operation landed inside the window (or inside the buffer), the time
series is measurement-confounded: day N's number is computed against a
different cohort than day N+1's, and the chart visualises an
instrument shift rather than a real movement. In that state, the
trend tier collapses to Tier 0 *for trend purposes* regardless of
how mature the cohort is in absolute terms. Use snapshot metrics
instead, or truncate the window to the longest sub-window with a
stable cohort. The same rule applies to cross-loop deltas: if the
prompt set changed between Loop N and Loop N+1, the delta is not a
movement, it's an apples-to-oranges comparison and must be flagged
as such or dropped.

**Engine-side drift — the gate's second instrument.** The stable-cohort
gate holds the tracked prompt set constant, but rate trends need stable
instruments on BOTH sides: the tracked cohort and the measuring engine.
The engine's own behaviour drifts over time — model updates change how
many fanout queries it issues per chat and how it phrases them — and
that drift can move fanout-derived ratios substantially with no change
on the brand side. Observed shape: a named-source share in fanout
queries rose from single digits to ~30% across a six-week window on an
unchanged prompt set, while fanout volume per day roughly halved — the
engine issued fewer, more source-anchored queries later in the window.
Trend claims on fanout-derived ratios must therefore report the per-day
fanout volume alongside the share, and frame shifts as "engine
behaviour + content landscape" movement, not brand movement. When the
engine itself drifts, report the drift as context, not the brand as
mover. This applies with most force to fanout mining (§13.10) but in
principle to any metric whose denominator is engine-generated.

**Day-window as a recommendation, not a hard gate.** The 7-day / 30-day
/ 60-day thresholds above are recommended defaults for signal maturity,
not blocking rules. Deadline-driven projects (stakeholder presentation
before the 7-day window closes) should run Loop 2 earlier with the
signal that's available, state the window size explicitly in findings,
and flag that longitudinal claims require more data. The alternative —
presenting an unanalysed strategy because the 7-day clock hasn't elapsed
— is strictly worse than presenting a thinner-but-honest read. This
aligns with §3.4 ("calendar time is a resource") and its corollary
against baked-in cadences the skill hasn't validated.

**Benchmark-goal vs trajectory-goal projects.** Data maturity tiers
describe what the data can support; they do not dictate when a
stakeholder deliverable is appropriate. Two project shapes diverge here:

- **Trajectory-goal projects** (stakeholder wants to see *movement over
  time*): Phase B is most useful at Tier 2+ when trend comparison
  exists. A Tier 0 / Tier 1 deck on a trajectory-goal project asks
  stakeholders to judge movement from a single point, which Phase B
  cannot honestly do.
- **Benchmark-goal projects** (stakeholder wants to know *where we
  stand today*): the benchmark IS the deliverable. A Tier 0 / Tier 1
  deck framed as a snapshot ("this is where we are on day N") answers
  the question legitimately. Waiting for Tier 2 delays the deliverable
  past its usefulness.

The §14.5 compressed-deck mode covers the Tier 0 case mechanically.
What changes with project goal is the framing: benchmark decks lead
with the position ("#N of M in category X"); trajectory decks lead
with movement ("+X pts since prior loop"). An agent running Phase B
on a benchmark-goal project at Tier 0 is not circumventing maturity
discipline — it is matching deliverable shape to the project's
stated goal. Capture the project goal in the intake state so Phase B
timing decisions can reference it without re-asking.

### 13.7.1 Cross-page verification before attributing content elements as "weakness"

When attributing a content element on a low-performing page as a
**weakness** ("this page underperforms because it has X" / "this page
underperforms because it lacks Y"), verify that element across at
least three other pages — including at least one strong performer —
to confirm the element actually patterns with performance.

A single low-performing page does not validate a causal claim. The
element may be a **template feature** (a default carousel, a stock
sidebar block, a navigation widget) shared across many pages
regardless of citation outcomes — in which case its presence /
absence on the low-performing page is correlation, not causation.
Cross-page verification is the cheapest way to distinguish
element-as-cause from element-as-noise.

**Procedure:**

1. Identify the element being framed as a weakness on the low-
   performing page.
2. Pick at least three other pages from the same page genre /
   template family. Include at least one strong performer (top-tier
   citation rate) and at least one mid-performer.
3. For each, check whether the element is present and at what
   prominence.
4. Pattern-test: does the element's presence pattern with citation
   performance? If a strong performer also has the element, OR a
   mid-performer lacks it, the element is unrelated to performance —
   drop the framing.

**Worked example.** A carousel of news items at the top of a page
was framed as a weakness on a low-citing expertise page. Cross-
checking five expertise pages revealed: low-citer (carousel
present), mid-citer 1 (carousel present), mid-citer 2 (no
carousel), strong performer (no carousel), weak performer (no
carousel). The carousel doesn't pattern with citation performance
— it's a template option some pages turn on. The actual driver of
the citation difference was the specific quantitative content
(deal numbers, named awards, named directories with years, client
quotes) per the §11.20 expertise-page recipe. The carousel framing
was dropped from the deck.

**Anti-pattern.** Naming a content element as a weakness based on a
single low-performing page, without checking the element across
other pages of the same genre. False-positive content critiques
erode the deck's credibility (the user can spot-check one and find
it doesn't replicate) and waste optimisation effort (the dev team
spends cycles changing an element that won't move the metric).

**Why this is its own gate.** §13.7's maturity tiers govern when
quantitative findings can stand; this rule governs when *causal*
content claims can stand. The maturity-tier gate doesn't catch this
class of error because the underlying numbers are defensible — the
problem is in the element-attribution layer above the numbers. The
fix is structural: every "page X underperforms because of element Y"
claim runs the cross-page check before being framed in a deliverable.

### 13.8 Qualitative chat reading is peer analysis, not detection verification

Reading chat payloads is prescribed in §13.1 as a detection-pattern
quality gate — but that under-specifies the activity. Chat reading is
also **first-class analysis**: it surfaces engine parametric biases,
source authority patterns, competitive positioning, and narrative
framings that aggregates can't express.

**Minimum chat-count target per topic.** For any topic with more than
five prompts, read **at least one chat per active engine** across a
systematic sample (i.e. full topic × engine matrix, not single-
dimension sampling). This is a floor, not a ceiling. Earlier
framings in this skill pulled "one chat from the highest-mention-count
engine per topic"; the per-engine-per-topic matrix is what surfaces
engine personality differences within a topic — and those differences
are typically where the strategic insight lives. Empirical evidence:
reading 20 chats across 10 topics × 3 engines (vs 7 chats
opportunistically) surfaced a set of findings (parametric ChatGPT
fabrications, retrieved-but-not-cited authority signals, generalist vs
specialist sentiment asymmetries) that did not appear in the smaller
sample at all. The second dimension costs only marginally more data
pulls but produces categorically different findings.

For topics with ≤5 prompts, the matrix is small enough that single-
per-topic sampling is adequate. The floor applies to the size range
where topic complexity outruns single-chat coverage.

Prescribed chat-reading activities (beyond detection verification):

1. **Per-engine chat comparison.** For each priority topic, pull one
   chat per active engine and read them side by side. What differs:
   which brands are named, in what order, with what framing, with what
   sources? Engine personalities become visible quickly (AI Overview
   leans on aggregator sites, ChatGPT leans on community-adjacent
   content, Copilot leans on corporate/editorial).
2. **Competitive positioning reading.** When a competitor outperforms
   the own brand on a topic, read 2–3 chats from that topic and examine
   how the competitor is positioned. Is it the first-named? Praised?
   Linked to authoritative sources? The aggregate tells you "they beat
   us"; the text tells you "why".
3. **Narrative framing audit.** For regulated verticals, read the
   language engines use around the category (cautionary framings,
   legal-aware language, hedging). These framings propagate into brand
   representation and matter for positioning — they don't show up in
   sentiment scores.

Chat reading scales differently from metrics: one well-read chat often
produces a stronger insight than a thousand aggregated rows. Budget
qualitative time as a first-class analysis activity, not as a
verification tax.

### 13.9 URL-content reading converts gap URLs from leads into briefs

URL gap analysis (`get_url_report` with `gap >= 2`) surfaces pages where
competitors co-appear and the own brand is absent. Without reading the
actual content, the recommendation is "check if own brand is listed."
That's a lead, not a brief.

Use `get_url_content` on the top 3–5 gap URLs per priority topic (by
retrieval count) and answer:

- Is the own brand **genuinely absent**, or is it present but missed by
  Peec's detection? (Detection failures are a separate finding — see
  §13.1.)
- What is the page's **taxonomy**? A listicle with named categories
  ("Magic Circle", "National firms", "Boutique") tells you exactly
  where the own brand would fit.
- Is the editorial format **amendable**? A listicle on a trade
  publication is an outreach candidate; a Chambers-style ranking is
  not.
- Which **direct peers** are present? The gap means more when the
  absent brand's named competitors are there.

The deliverable per URL: a one-paragraph brief naming the page, the
insertion point, the peer set, and the outreach angle (editorial pitch,
correction request, sponsored placement, nothing).

A URL gap with unread content is a data point. A URL gap with read
content is a brief. Build the brief.

**Host classification before treating any gap URL as an editorial
target.** Gap reports surface every URL where competitors appear and
the own brand doesn't — including URLs on competitor-owned domains
where the own brand cannot realistically be added. Before treating any
gap URL as an editorial target, classify the URL's host against the
tracked brand roster (`list_brands.domains`). Three sub-cases apply:

- **Competitor-owned homepage / category page / product page** — out
  of scope. The competitor cannot realistically include a rival brand
  on its own commercial pages. Drop from outreach analysis. Recording
  the URL in findings is fine; surfacing it as an editorial gap to the
  stakeholder is not.
- **Competitor-owned listicle / comparison / "best of" page that ranks
  multiple brands including direct rivals** — in scope, *but flag
  explicitly as competitor-owned* in any deliverable. Outreach
  feasibility depends on the competitor's editorial policy; treat as a
  case-by-case judgement rather than a default outreach target.
- **Competitor-owned editorial / blog content that doesn't rank
  competitors** — out of scope (typical case). The competitor's
  editorial control over its own blog is not negotiable.

The unfiltered gap list from `get_url_report` / `get_domain_report`
is a data surface; the filtered list — competitor own-domains
removed, multi-brand listicles flagged — is the actionable outreach
surface. Conflating the two ("you want me to ask Linda Seeds to
feature their direct competitor on their homepage?") surfaces
unactionable recommendations and erodes credibility even though the
underlying retrieval data is correct. The question that actually
needs answering is *"is it realistic for us to appear here?"*, and
the answer depends on who controls the page, not just on retrieval
volume.

**Brand-detection verification — "brand X absent" has at least three
different causes.** When a URL gap report flags "brand X absent" on a
page that survives host classification (i.e. is a legitimate
editorial target, not a competitor-owned page), a "brand absent"
signal can come from at least three distinct causes — only one of
which is a real action:

- **(a) Cache-extraction miss.** Peec's `get_url_content` uses
  Mozilla Readability + Turndown GFM, which can silently strip
  mid-page sections (see `peec-ai-mcp` §6.6 Failure mode C). If
  brand X sits in a section Readability removed, both the cached
  content AND Peec's `mentioned_brand_ids` detection layer (running
  on the same truncated content) miss it. The brand is genuinely on
  the page; the gap signal is a tooling artefact. **No action — log
  as a Peec brand-detection limitation in findings.**
- **(b) AI-engine retrieval bias.** The page contains brand X, but
  the AI engines that retrieve this URL preferentially cite earlier
  / more prominent sections of the page and skip the section
  containing brand X. The brand is on the page but not in the
  *resulting AI response text* — `mentioned_brand_ids` correctly
  records absence in chats, while the page itself is fine. The
  underlying issue is the page's structural hierarchy, not absence.
  **Out of own-brand control — no actionable outreach.**
- **(c) Genuine absence.** Brand X is not on the page. **This is the
  only case where an editorial outreach action is warranted.**

Before treating any gap URL as an editorial action item, run a
browser-verification step (open the URL in a real browser, search
the rendered DOM for brand X) to disambiguate cases (a)/(b) from
(c). The cost of one browser-verification call per candidate URL is
much lower than the cost of an embarrassing wrong-action item that
turns out to rest on a Readability extraction bug. See §14.6 for
where this verification fits in the deck-build pipeline and §15.3
for the pre-Phase-B gate that codifies it.

**Anti-pattern.** Treating a Peec `mentioned_brand_ids` "brand
absent" signal on a gap URL as automatically actionable. The signal
is rolled up from text matching against retrieved chat content
(case b) plus cached page content (case a) — multiple distinct
phenomena produce the same downstream signal. Without
disambiguation, action items are betting on the wrong cause and
damage stakeholder trust when the user spot-checks one and finds
the brand is, in fact, on the page.

### 13.10 Fanout mining is a continuous refinement signal, not an intake-only tool

`list_search_queries` (see `peec-ai-mcp` §7.41) surfaces what ChatGPT
searched for while generating a chat. The intake phase uses fanout to
seed the prompt set — but the data keeps producing signal post-intake,
and most projects never exploit it.

Run fanout mining as a standing Analyse activity:

1. **Parametric-bias detection.** If a topic has high visibility but
   zero fanout (ChatGPT answered from parametric memory without
   retrieving), the own brand's presence is independent of retrievable
   sources. That's a strategic finding: the brand lives in the model's
   priors, not in content the brand controls. The reverse — zero
   visibility with rich fanout — means the retrieval surface is busy
   but the brand isn't on it.

   The strategic implication applies *wherever the pattern holds* —
   not only in regulated verticals. High-salience parametric fallback
   (§11.13) covers branded queries generally, well-known category
   or standards queries, and any query space well-represented in the
   pre-training corpus. The action set is the same regardless of
   vertical: (a) shift content-strategy focus toward retrieval-based
   engines where published content can move the needle observably,
   (b) set long-horizon measurement expectations for the parametric
   engine, (c) prioritise training-data-influencing work (Wikipedia
   presence, authoritative third-party listicles, schema markup that
   consistent reference sites pick up). Regulated verticals are the
   sharpest case; they are not the only case.
2. **Adjacent-intent discovery.** Fanout sub-queries reveal the
   adjacent-intent space around a tracked prompt. A prompt for "best
   law firms for fraud recovery" that fans out to "UK firms
   specialising in asset tracing" is telling you where to place the
   next prompt slot.
3. **Platform-mention mining (ranking-dominated verticals).** Grep the
   fanout `query_text` for the vertical's candidate ranking bodies,
   directories, and platforms by name. A high named-source share
   confirms — at the query level, one step earlier in the causal chain
   than §11.19's visibility decomposition — that the vertical is
   ranking-dominated, and identifies WHICH bodies gate visibility.
   Observed shape: in a professional-services vertical, ~20%+ of all
   fanout queries named one of exactly two ranking directories, with
   tier vocabulary and year qualifiers in the query text and zero
   mentions of any other directory or platform. That sharpens the
   §11.19 action considerably: submission and PR effort targets the
   named bodies only, not the long tail of directories. The general
   principle: when an engine names a specific gatekeeper source class
   in its queries, that source class is the visibility gate, with
   higher confidence than any inference from outcome metrics. §11.19
   cross-references this as its confirming diagnostic.
4. **Trend caveat — engine behaviour drifts.** Fanout-derived
   time-series carry a confounder the stable-cohort gate (§13.7)
   doesn't cover: the engine's own query-generation behaviour changes
   over time (fanout volume per chat, query phrasing style). Any trend
   claim on a fanout ratio must report the per-day fanout volume
   alongside the share, and frame shifts as engine-behaviour +
   content-landscape movement rather than brand movement. See the
   engine-side drift extension in §13.7.
5. **Engine-scope caveat.** Fanout data surfaces only from ChatGPT
   currently (peec-ai-mcp §7.41). For AI Overview and Copilot, fall
   back to the chat-level `sources` array as the retrieval signal.
   Phrase findings as "what ChatGPT searches for", not "what the AI
   searches for".

### 13.11 ChatGPT parametric behaviour in regulated verticals is a strategic pattern

In regulated verticals (cannabis, pharma, legal services in specific
jurisdictions, some financial products), ChatGPT often answers entirely
from parametric memory — no retrieval, no sources. Fanout returns
empty; the `sources` array on the chat is empty or near-empty. This is
a structural feature of the vertical, not a per-project anomaly.

Strategic implications:

- **Source authority audits underestimate ChatGPT's importance** in
  these verticals. The brand's ChatGPT visibility depends on the
  model's priors — updated via training data cycles, not via content
  the brand publishes this quarter.
- **Content strategy focus shifts to other engines.** AI Overview and
  Copilot retain retrieval-based answering in regulated verticals and
  are the channels where published content can move the needle in
  observable timeframes.
- **Long-horizon measurement.** ChatGPT visibility in these verticals
  moves on training-cycle timescales (six months to a year). Don't
  expect quarter-over-quarter changes to show up there.

Flag this explicitly in the strategy deliverable whenever the vertical
qualifies: document the parametric-vs-retrieval split per engine and
the content-strategy implications that follow.

**The broader pattern — where else this applies.** Regulated verticals
are the sharpest case of high-salience parametric fallback but not
the only one. Data across multiple projects from early 2026 shows
the same parametric signature on branded queries where the brand is
strongly recognised, on well-known certification / standards queries,
and on commercial categories where the model has strong priors from
the pre-training corpus. See §11.13 for the enumerated conditions,
§13.10 for the diagnostic procedure, and `peec-ai-mcp` §7.5 for the
per-engine architectural reasons.

Treat regulated verticals as the guaranteed case for this pattern —
the strategic-deliverable flag is mandatory there. Non-regulated
projects may still hit the pattern on branded / high-salience query
subsets; when they do, apply the same strategic implications per-
query-cluster rather than project-wide. Don't force a non-regulated
project into the "whole project is parametric" framing just because
a single cluster within it shows the signature.

**Sampling discipline.** Before declaring a project (or cluster)
parametric, sample chat payloads and confirm the response body is
substantive text that shows training-data-style reasoning. `sources:
[]` has multiple causes (peec-ai-mcp §7.8); engine refusals, empty
placeholder responses, and genuine parametric answers all share the
empty-sources signature. Differentiating them matters: the strategic
action for "engine refuses to answer" is almost never the same as the
strategic action for "engine answers from parametric memory".

### 13.12 Shopping queries (`list_shopping_queries`) — e-commerce only

`list_shopping_queries` surfaces the product-shopping intent behind a
chat. It is **structurally empty for B2B verticals and most
service/information verticals** — this isn't a data gap, it's absence
of commercial-product intent in the prompt space. Don't flag empty
shopping-query data as a problem in B2B / services / regulated-vertical
contexts.

For e-commerce projects, shopping queries carry direct commercial
value: they reveal the exact product language the engine is filtering
by, independent of the prompt phrasing. Mine them:

1. Run `list_shopping_queries` per tracked chat.
2. Aggregate the queries per topic — which product attributes the
   engine is filtering by.
3. Cross-reference against the own-brand product catalogue — where are
   the engine's filters finding competitor products but not the own
   brand's SKUs?
4. Surface the findings as product-feed / catalogue recommendations
   alongside the prompt-level visibility findings.

This is an entirely separate surface from prompt-level analysis and
deserves a dedicated section in e-commerce findings deliverables.

### 13.13 URL/domain gap reports also surface brand roster candidates

Gap reports (`get_url_report`, `get_domain_report` with `gap >= 2`)
identify content targets — pages/domains where the own brand is absent
but competitors appear. The same data also identifies **brand roster
candidates**: brands that keep co-appearing in competitive contexts but
aren't in the tracked roster.

Cross-reference the gap reports against `list_brands`:

- Any `mentioned_brand_ids` appearing frequently across the top-N gap
  URLs that **isn't** in `list_brands` is a roster candidate.
- Any domain in the top-N gap domains that isn't associated with a
  tracked brand is a roster candidate (use `classification=CORPORATE`
  as a first filter).

Surface roster candidates as a distinct finding from content targets —
they feed the next loop's §9.3 Brand roster decision, not the content
strategy output.

### 13.14 Per-engine visibility variance requires per-engine chat reading

Aggregated visibility hides per-engine variance. A topic showing 50%
visibility overall might be 90% on ChatGPT, 30% on AI Overview, and 15%
on Copilot — each telling a different story. Setting a single
topic-level posture (defend / build / close) on the aggregate is
unreliable; the right posture differs per engine.

When per-engine visibility differs by more than 30 percentage points
within a topic, posture decisions require **per-engine chat reading**
before they're finalised. Read one chat per engine on that topic and
ask: why is engine X different? Different prompt interpretation?
Different retrieval surface? Different parametric bias? The answer
shapes whether the topic needs content, outreach, prompt refinement,
or is simply not addressable in that engine.

### 13.15 Own-brand URL citation map reveals content authority hot/cold spots

Run `get_url_report` filtered to the own brand's domain(s) and cross-
reference against the site map. The output is a content-authority map:

- URLs with **high retrieval + high citation** = content the AI surface
  has baked into its answers. Protect and update.
- URLs with **high retrieval + low citation** = the engine reads them
  but doesn't cite them — often because the content is relevant but
  not authoritative. Upgrade the authority signals.
- URLs with **low retrieval + high citation** = niche content the
  engine reaches for on specific queries. Understand what those
  queries are via fanout.
- URLs with **zero retrieval** that the site keeps prominent = a
  content/audit mismatch. The site thinks the page matters; the AI
  surface doesn't.

Produce this map as a standard Analyse deliverable for any project
with a substantial content footprint. It's the content-audit artefact
that most closely translates AI visibility data into editorial
priorities.

### 13.16 Deferred-items queue (mandatory hand-off to next loop)

§12.8 specifies hand-off requirements after the Write sub-phase
(write summary, earliest re-analysis date, pending verification,
findings location, cohort maturity map). §13.16 is the equivalent
for the Analyse sub-phase: every Analyse loop must produce a
**deferred-items queue** as a discrete artefact — not as a bullet
inside the findings file, but as a named queue the next loop reads
first.

Without this, items that are "review next loop" (deferred brand
candidates, deferred analyses, content strategy threads to revisit,
items waiting for more data) live only in the agent's session
memory. The next loop starts, reads the latest findings file,
doesn't find the deferred items list, and quietly loses them. The
failure mode is invisible until weeks later when someone notices
the thread was dropped.

**Required sections of the deferred-items queue:**

1. **Verification required** — anything the next loop must re-check
   (pending recalcs, new entity propagation, fix verification).
2. **Deferred decisions** — items considered this loop but waiting
   for more data (brand candidates, prompt cuts, taxonomy changes,
   with the evidence threshold that would trigger execution).
3. **Strategy-level items** — content strategy findings,
   engine-specific recommendations, prompt set candidates that need
   the next Strategy iteration.
4. **Outstanding analyses** — analyses not run this loop because of
   data maturity (e.g., 7-day window not yet available; Wave-N
   writes not yet propagated).
5. **Recommended loop sequence** — what the next loop should do
   first, so the agent doesn't have to re-derive the order from
   scratch.

**Mandatory `deferral_type` on every queue item.** Each item in the
queue must be tagged with the *reason* it's deferred, because the
strategic response differs by reason. Allowed values:

- `awaiting_signoff` — executable now, blocked only on a user yes/no.
  Surface these for action at the next loop's hand-off (§13.6
  execute-now rule) rather than leaving them queued indefinitely.
- `awaiting_data` — blocked on signal maturity. Revisit at the named
  data window (e.g., "revisit at day 7" or "revisit after Wave 3
  propagation completes").
- `awaiting_external` — blocked on a dependency outside the agent's
  control (client internal approval, third-party publication, a data
  source the agent can't trigger).

Without typed reasons the queue drifts: an `awaiting_signoff` item
sits dormant because the next loop assumes it's data-blocked; an
`awaiting_data` item gets executed prematurely because the next loop
assumes it's signed-off. Typing the reason makes the queue
scannable — a user or agent reading it can act on the
`awaiting_signoff` items immediately, slot the `awaiting_data`
items against the cadence calendar, and set reminders or nudges for
`awaiting_external` items.

Queue items without a `deferral_type` fail the Pre-Analyse gate —
treat as a hard validation requirement, not a format nicety.

**Format and storage:** agent/user choice (§3.7), but it must be a
discrete, named artefact the next loop can find and consume. A
section buried in the findings file doesn't count — the queue is
the primary hand-off, findings are the reference material. A
simple three-column table (`item`, `deferral_type`, `trigger/owner`)
works; YAML, a named section in the intake state, or a per-loop
handoff doc all work. What doesn't work is a prose paragraph that
mixes the three classes without labelling them.

**Rule:** the queue is a required output of every Analyse loop,
produced even when it contains nothing for some categories. An
empty section is itself a signal ("nothing deferred; all items
this loop resolved").

If no action emerges from the findings, that's a signal the skill hit
a stable-state phase and Phase B may be ready (see §14).

### 13.17 `get_actions` — mandatory surface + critical-filter pipeline

`get_actions` is Peec's own opportunity-scoring surface. It combines
reliable data (gap percentages, named URLs, named domains, slice
classifications, opportunity scores) with an automated action
interpretation (the `text` column's "Incorporate X", "Participate in
Y", "Take inspiration from Z"). The data surface is trustworthy; the
interpretation surface is generic and unaware of brand positioning,
platform norms, sister-brand relationships, or commercial context.
Skills that reference `get_actions` without separating the two trust
surfaces will propagate Peec's generated action framing into strategy
and client recommendations — often with material errors.

**13.17.1 Mandatory call pattern.** Every Analyse loop:

1. Call `get_actions(scope=overview)` against the same window the
   loop is analysing.
2. Drill into any slice with `opportunity_score ≥ 0.05` **or** any
   slice with `gap_percentage ≥ 0.7` regardless of score.
3. Record outputs in the findings file under a dedicated
   **"Peec-scored opportunities"** section — kept separate from manual
   findings so cross-referencing is explicit.
4. Where manual findings and Peec-scored findings agree, confidence
   goes up. Where they diverge, investigate before either overrides
   the other.

Skip only if no prompts in the loop's window have matured
(§12.7) — in which case the loop itself is usually premature.

**13.17.2 Critical-filter step (data vs interpretation).** For every
recommendation returned by `get_actions`, record three things
separately in the findings file:

1. **The signal** — the named URL, the named domain, the
   `opportunity_score`, the `gap_percentage`, the slice
   classification (OWNED, EDITORIAL, COMPARISON, UGC, etc.).
2. **The action as Peec phrases it** — verbatim `text` column.
3. **Commercial-common-sense review outcome** — pass, fail, or
   reframe, with one-line rationale.

Actions that fail review still have their signal preserved (the data
isn't wrong; the interpretation is). They must not propagate into
Phase B deliverables or client recommendations under Peec's original
framing. Where the review reframes an action (see 13.17.4 for the
affiliate case), the reframed version replaces Peec's original.

**Known anti-pattern actions to flag automatically on sight:**

- **Homepage vocabulary stuffing.** "Incorporate [wellness/niche
  keywords] into the homepage copy" is almost always wrong for a
  brand homepage — the homepage is the primary brand-positioning
  surface, and stuffing it with keyword vocabulary dilutes the brand
  regardless of what the opportunity score says. Preserve the signal
  (homepage doesn't register for query X); drop the action.
- **"Mention your own brand favorably" on Reddit or UGC.**
  Platform anti-pattern that generates bans and negative sentiment.
  Preserve the signal (UGC surface retrieves for this query);
  reframe the action as a genuine participation/content path on
  the platform, or flag for a platform-appropriate alternative.
- **"Take inspiration from" sister-brand pages.** Peec has no way
  to know the brand roster includes sister brands. If the "competitor"
  URL is a sister brand's category page, the signal is "family-of-
  brands share" not "competitive gap"; drop the action.
- **PR outreach to affiliate-driven "vergleich"/"best-of" domains
  classified EDITORIAL/COMPARISON** — handled by 13.17.4 below.

**13.17.3 Legitimacy check — fallback chain.** Business-model
verification is part of the critical filter for any
`EDITORIAL`/`COMPARISON`/`LISTICLE` target in the shortlist. Run
the chain in order:

1. **WebFetch** on the target's `/about`, homepage, or a
   representative content URL. Look for affiliate disclosure,
   outbound-link patterns, byline and editorial-team presence.
2. **Claude in Chrome** (or equivalent browser-extension tool) if
   WebFetch fails (redirects, 403, anti-bot, Cloudflare challenge).
   Render the page through a real browser, inspect outbound-link
   parameters, check for disclosure copy.
3. Only after **both** tools fail may a target be marked
   `unverified` in the shortlist. Even then it stays in the
   shortlist with an unverified status — it is not silently
   dropped.

**Rule:** a target is dropped only when the target itself fails
(business model misaligned, content not relevant, platform anti-
pattern). A target is never dropped because a single verification
tool returned an error — that's a tool blocker masquerading as a
finding, and it silently reduces the shortlist based on agent tool
access rather than target quality.

**13.17.4 EDITORIAL/COMPARISON default — assume affiliate-driven.**
Peec's `EDITORIAL`/`COMPARISON` classification for comparison and
listicle domains in commercial verticals is unreliable. Peec
classifies by surface-level markers (article structure,
editorial-looking content) and cannot see the affiliate monetisation
layer underneath. In commercial verticals with high affiliate base
rates — ecommerce, regulated categories (cannabis, pharma, nutra,
gambling, adult, weapons), SaaS review, VPN, hosting, finance —
assume an `EDITORIAL`/`COMPARISON` target is affiliate-driven until a
browser check confirms otherwise.

Confirmation requires **all** of:

- No affiliate parameters on outbound links (`?aff=…`, `?a_aid=…`,
  `/ref/…`, tracked redirects through adcell / awin1 /
  tradedoubler / daisycon / ShareASale / Impact / CJ).
- No commission/affiliate disclosure copy ("provisionen", "as an
  affiliate", "partner programme", "we may earn a commission").
- Visible editorial byline and separation from commercial
  interests.

Action-shape routing based on the verification result:

- **Genuine editorial** (independent publication, e.g. trade
  magazines, verified byline, no affiliate layer): PR outreach is
  the right action. Pitch the site.
- **Affiliate-driven** (any of the disqualifying markers present):
  the **network** is the vehicle, not the site. The correct action
  is to join the relevant affiliate network — one programme places
  the brand across every affiliate comparison site on that network
  that covers the category, at scale. The specific URL Peec
  surfaced is one instance of a network-wide distribution channel.

The default flip from "verify each case" to "assume affiliate unless
proven editorial" saves check cycles in verticals where affiliate is
the base rate. Two independent confirmed instances in a vertical
flip the prior for that vertical; keep a note in the findings file
so later loops inherit the updated prior.

### 13.18 `list_prompts.volume` in regulated verticals — low-discrimination caveat

Peec's `list_prompts.volume` ordinals (`very high`, `high`,
`medium`, `low`, `very low`) are a useful signal in high-variance
commercial verticals (SaaS, consumer electronics, travel,
high-variance ecommerce). In **regulated verticals** — cannabis,
pharma, nutra, gambling, weapons, adult, and any vertical where
search volume is suppressed by content restrictions — the
distribution collapses to the low end (`low` / `very low`) across
most commercial topics. Volume becomes a near-constant and does not
discriminate between high-priority topics and noise topics in the
brand's candidate set.

**Rules:**

- Do not use `list_prompts.volume` as the backing for topic-priority
  claims in regulated verticals. The signal doesn't discriminate.
- In mixed-vertical brands, segment by vertical and note the
  low-discrimination caveat for the regulated segments.
- When volume is needed for topic prioritisation in a regulated
  vertical, route to the external-backing sources named in §14.9
  (live product categories, SKU counts, published brand
  commitments, GSC / external search data). The external path is
  more reliable and more defensible in stakeholder output.

The anti-pattern this rule prevents: an agent reaching for volume
as a topic-priority backing source, finding it collapses across the
candidate set, and either forcing a "topic A is higher priority than
topic B" claim on near-identical volume ordinals or silently giving
up and picking topics on intuition.

---
