---
name: peec-ai-tracking-strategy-builder
description: Build or refine a Peec AI tracking strategy as an iterative workflow. Phase A loops Intake → Strategy → Write → Analyse, each producing concrete Peec configuration changes and findings that feed the next iteration. Phase B (optional, terminal) produces a stakeholder presentation once the strategy has stabilised. Works for brand-new projects and for existing projects that need rationalising. Load whenever the user mentions Peec AI strategy, Peec setup, Peec onboarding, "what should we track in Peec", "build/improve our Peec project", Peec prompt rationalisation, Peec rebuild, or AI visibility strategy for a brand using Peec. Also triggers on Phase B / retrospective requests: stakeholder presentation, deck, or brief that explains an existing Peec strategy; "explain our Peec setup", "document our tracking strategy", "analyse our current Peec project", "what are we tracking in Peec and why". Companion to `peec-ai-mcp` (recommended; tool mechanics).
version: 1.0.0
license: CC-BY-4.0
origin: https://github.com/rebelytics/peec-ai-tracking-strategy-builder
maintainer: Eoghan Henn / rebelytics (eoghan@rebelytics.com)
---

# Peec AI Tracking Strategy Builder

**Created by Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)**

An end-to-end workflow for taking a brand from "I've just connected the Peec
MCP" — or "our Peec project's structure no longer fits our current strategy"
— to a measured, well-structured tracking strategy that reflects commercial
reality. The skill runs as an iterative loop (Intake → Strategy → Write →
Analyse) that produces concrete Peec configuration changes and findings
each pass, with an optional terminal phase for a stakeholder presentation
once the strategy has stabilised.

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

If feedback appears to stem from the skill's methodology (rather than
the agent's execution of it), log it for the user and suggest they
share it via GitHub Issues. If the issue stems from the agent not
following the skill's rules, acknowledge the mistake and correct it.

---

## 1. What this skill is (and isn't)

**Is:** A layered, prescriptive workflow that takes a brand from Peec MCP
access to a tracking strategy that's live, documented, and explainable to a
stakeholder. Runs as an iterative loop (Intake → Strategy → Write → Analyse)
so the strategy gets sharper each pass. Covers both **new projects** (no
prompts yet) and **existing projects** (prompts already in place that may
need rationalising). The agent decides which path to take by inspecting the
project's current state, not by asking.

**Two classes of deliverable:**

- **Mandatory:** the configured Peec project itself — brands, topics, tags,
  and prompts written directly via the Peec MCP as Phase A loops produce
  them.
- **Optional (Phase B, terminal):** a stakeholder-facing presentation of
  the strategy and its findings. The agent should proactively offer this
  after the first full Phase A loop completes (see §14.5), rather than
  waiting for the user to ask. Produced once at the end of the
  engagement; not committed to upfront — see §14.

Working artefacts (intake state, strategy sign-off artefact, findings from
each Analyse loop, verification log) are produced by the workflow but are
not deliverables in their own right. Their format is agent/user choice
(§3.7).

**Isn't:** A Peec MCP tool reference. Tool mechanics, schema quirks, and
setup instructions live in the companion [`peec-ai-mcp`](https://github.com/rebelytics/peec-ai-mcp)
skill. Strongly recommended to load alongside; see §5.

**Isn't:** A content strategy skill. A Peec strategy build surfaces
content/distribution opportunities (Reddit gaps, Wikipedia defence,
listicle editorial issues) but doesn't execute them. Those land in the
strategy sign-off artefact and any Phase B deliverable for the content
team.

---

## 2. When this skill fires

Trigger when the user:

- Wants to set up tracking for a brand on a newly connected Peec project.
- Has an existing Peec project where the brand list, topics, tags, or
  prompts need rationalising.
- Says "what should we track in Peec", "build our Peec strategy", "fix
  our Peec setup", "our Peec project's structure needs review", or
  equivalents.
- Shops around and an agency pitches "we'd do it differently" — the
  output of this skill produces a defensible set of decisions to
  respond with.
- Asks for a tracking strategy review ahead of a renewal or budget
  conversation.
- Wants a stakeholder presentation for an already-stable Peec strategy
  (Phase B only — the Phase A loops have already produced the strategy in
  an earlier engagement).

The skill works with any project state: zero prompts, 50 prompts still
finding shape, or several hundred prompts that have grown organically.
The workflow routes automatically based on what it finds.

---

## 3. Cross-cutting principles

These seven principles apply across every phase and loop of this skill.
Re-read them at the start of each loop before planning work. They're the
rules of the game — phase-specific rules (§§4, 8–14) defer to these when
they conflict.

### 3.1 Instrument separation (branded vs non-branded)

Branded prompts and non-branded prompts measure different things. They're
not two columns of one KPI — they're two instruments with different
purposes. Never aggregate them into a single visibility score, never claim
"the brand leads on branded prompts" as a finding (tautological), and
always report them separately in analysis and strategy. If a combined
metric is requested, produce it only with explicit disclosure that it's an
anti-pattern.

**What each instrument actually measures** (name this in stakeholder
outputs whenever the split appears):

- **Non-branded prompts = commercial visibility scorecard.** These
  measure whether the AI surface names the brand when asked commercial
  questions that don't contain the brand's name. This is the genuine
  discoverability signal and the only valid basis for competitive
  comparison.
- **Branded prompts = reputation / representation instrument.** These
  measure how the AI describes the brand when asked about it by name.
  Visibility on branded prompts is near-tautological (the brand name is
  in the prompt, so the brand nearly always appears in the response);
  sentiment and source authority on branded prompts carry real
  information.

Three hard rules that follow from the split:

1. **Headline metrics split side-by-side.** Never report a blended
   visibility / SoV figure as the primary number. Blended figures are
   dominated by branded prompts — the brand looks stronger than it is,
   and the non-branded competitive picture disappears.
2. **Competitive comparisons exclude branded prompts.** SoV and position
   rankings only make sense on non-branded; branded prompts
   systematically favour whichever brand's name is in the prompt.
3. **Sentiment belongs to branded.** Non-branded sentiment is mostly
   noise — most non-branded responses don't name the brand at all.
   Sentiment as a headline metric comes from the branded cohort.

Peec has no native branded/non-branded concept. The standard
implementation is **user-defined tags** (`branded`, `non-branded`) set
at prompt creation, with topic-based splits as a fallback when all
branded prompts live under a single topic. See §9.7 for the tag schema
and §13.3 for the arithmetic recipes.

**Phase B additional rule — the branded visibility number does not
appear.** In analysis contexts (§13), the split-side-by-side rule above
is sufficient because the audience is reading the methodology carefully.
In stakeholder contexts (§14), attention filters captions and leaves
only the numbers behind — a branded visibility figure of 93% next to a
non-branded figure of 18%, even with a caption reading "branded measures
reputation, not discovery", anchors the stakeholder on the 93% as a
success signal. Split-screen framing is not a sufficient guard against
anchoring in a Phase B deliverable.

The Phase B rule is stricter than the Phase A rule:

- The branded visibility figure **must not appear** in any Phase B
  slide — not as a headline, not as a comparison, not in a caption, not
  as a sanity check, not on a methodology slide.
- The non-branded figure appears on its own, named clearly as the
  discoverability score (or equivalent stakeholder-register label, see
  §3.5).
- The branded cohort is acknowledged as "measured separately, reported
  on sentiment and source authority in the branded appendix" (or an
  equivalent one-liner); it is not presented with a percentage.

The anti-pattern this rule prevents: a well-intentioned "here's why we
split them" slide that shows both numbers to make the methodology
visible, and lands as "their branded visibility is 93% — they're doing
great" with the stakeholder. In stakeholder contexts, captions get
filtered, numbers survive, and only the bigger number is remembered.

### 3.2 Provenance

Every claim in findings must be traceable to a Peec tool call (which
report, which filter, which date window, which cohort). If a claim can't
be traced, it doesn't appear in findings. This is the discipline that
keeps Analyse honest.

### 3.3 Caveats are symptoms, not cover

If a finding requires a caveat to be defensible ("this is strong, but note
that the cohort was only 3 prompts"), the finding is weak — the caveat is
a symptom that the evidence is thin. Either strengthen the evidence,
reframe the finding, or drop it. Caveats shouldn't be used as a rhetorical
escape hatch.

### 3.4 Calendar time is a resource

Measurement windows, cohort age, and signal maturity are real constraints,
not formalities. "We wrote 25 deletes yesterday, let's analyse tomorrow"
is wrong because the cohort hasn't stabilised. Plan loops around the
signal, not the work rhythm. Communicate pacing expectations to the user
at intake (§8).

**Corollary — don't bake in cadences the skill hasn't itself validated.**
Specific review rhythms (30-day, 90-day, monthly) should be described as
user-configurable defaults with the rationale exposed, not hard-coded as
skill prescriptions. If the skill's validation timeframe hasn't covered
multiple cycles at the prescribed cadence, treat it as a hypothesis the
user can adjust, not a finding. Phrase cadence language accordingly
("Users typically review monthly; confirm what works for your team")
rather than as a rule ("Reviews happen monthly").

### 3.5 Audience separation

The agent's internal reasoning, the user-facing sign-off, and the Phase B
stakeholder deliverable are three different audiences with three different
registers. Don't leak internal methodology language into stakeholder
decks; don't leak stakeholder simplification into agent reasoning.

### 3.5.1 AskUserQuestion register rules

> **Tool note.** "AskUserQuestion" is the structured multi-choice user
> prompt used by Claude. Other agents expose equivalent primitives under
> different names. The rules below apply to any structured multi-choice
> user prompt, whatever the agent calls it.

The audience-separation principle (§3.5) applies to every user-facing
surface, including AskUserQuestion payloads. Skill-internal labels exist
for agent orientation; they must never appear as user-facing option
labels or question text.

1. **Option labels must be outcome-focused, not process-focused.** Prefer
   "I design the strategy, get your OK, then create everything in Peec"
   over "Phase A loop 1 — strategy + writes + verify".
2. **Never use skill-internal jargon in option labels or questions.**
   Forbidden terms as user-facing labels: "Phase A / Phase B",
   "Ring 1/2/3 intake", "strategy sign-off artefact", "disposition
   framework", "prompt disposition", "loop 1", "rationalise",
   "Branch A/B" (§9.6), "quality gate".
3. **Option descriptions should describe what the user will experience**,
   not what the skill's phases do. Good: "You end with a live,
   configured project collecting data from tomorrow." Bad: "Agent
   executes Write sub-phase after sign-off."
4. **When in doubt, assume the user has never seen the skill.** This
   mirrors §3.5 for stakeholder decks but applies during the work, not
   after. The skill runs with the user inside it — they don't benefit
   from the skill-internal vocabulary the agent uses to navigate the
   workflow.

### 3.6 Label travel

Tags, topic names, and strategy labels travel across deliverables and
sessions. A tag called `gap-to-close` in Peec needs to mean the same thing
in the strategy sign-off, in findings, and in a Phase B deck. Rename with
care, rename everywhere at once, and document what each label means in
the persisted intake state.

### 3.7 What, not how

This skill specifies what must happen, not how it's presented. Anything
format-dependent (file names, sign-off medium, findings structure, state
persistence mechanism) is agent/user discretion. The skill prescribes
outcomes and gates; users choose their own formats.

### 3.8 Intake shortcuts are the strongest failure mode this skill has seen

If the agent feels tempted to proceed to Strategy with "enough" from
Ring 1, treat that feeling as a prompt to re-read §4.1 and §4.2. The
skill's core claim — prompts earn their slots from data, not intuition —
is defeated the moment Intake is cut short. There is no "good enough
Ring 1" that justifies skipping Ring 3.

Observed failure mode: after completing Ring 1 (Peec MCP +
brand-context skill + web searches), the agent jumped directly to
Strategy with 70
prompts and a full allocation table based on commercial intuition — zero
external data, zero baseline seed-and-harvest, zero Ring 3 user ask.
The rule existed three times over (§6, §8.3, §15.1 checklist). All three
layers failed under context pressure. The fix is structural: §8.4 and §9
now enforce a visible intake summary gate (see those sections). This
principle explains *why* the gate exists.

**Recurrence after structural fix.** The same failure mode recurred even
after §8.3 was restructured into named paths (§8.3.1/§8.3.2/§8.3.3a–c).
When discovered mid-run, recovery is not optional. Three named shapes
govern what the failure looks like; §3.9 governs how to recover.

**The three named skip shapes:**

1. **Outright omission** — the intake step was simply not run, either
   by not asking at all or by asking with partial coverage of the
   required fields.
2. **Substitution on recovery** — the skipped step was noticed, but the
   agent filled the gap with web search / commercial intuition / another
   Ring's data rather than running the step it was meant to.
3. **Planned deferral to Loop 2** — the skipped step is **documented as
   a deliberate design choice**: "we'll layer external data in during
   Loop 2". This is the most dangerous shape because it passes every
   existing gate by accepting "documented" as equivalent to
   "dispositioned". The agent names the gap, justifies the deferral with
   workflow-aware language ("seed-and-harvest first; enrich later"),
   writes it into the strategy, and ticks §15.1's "external data
   acknowledged, not hidden" box — and none of that is the user actually
   sharing data.

All three are failures of the same rule: Intake is a prerequisite for
Strategy, not a parallel track. "We'll cover it later" is not a Strategy
input — it's deferred intake, and Strategy has to wait. If external data
genuinely isn't available for Loop 1, the user must **explicitly decline
in writing** and take ownership of the gap; the agent cannot self-grant
the deferral.

### 3.9 Recovery from intake-step skip is a user-ask, not a substitution

When a §8.3 intake step is discovered to have been skipped, the recovery
action is to **ask the user for the data the skipped step was meant to
elicit** — not to substitute web search, not to swap in a baseline-path
seed-and-harvest, not to claim the external data was covered elsewhere.
Substitutions move the skip from "obvious" to "hidden"; the user has no
way to see that Ring 3 was never completed. The structural enforcement
at §8.4 depends on the user-ask step producing real user input, not
synthetic substitutes.

**What recovery looks like:**

1. Pause whatever phase discovered the skip.
2. Name the skip in-session: "I see Ring 3 external data intake was
   skipped — I need to run that now before the Strategy can be trusted."
3. Run the user-ask in the mode the skipped step specifies (batched
   AskUserQuestion for Ring 3 §8.3.3b; attachment request for §8.3.1 data
   dump paths).
4. Only after the user has actually provided the data (or explicitly
   declined and taken ownership of the gap in writing) may the skill
   proceed.

Recovery via web search, baseline activities, or "I'll just note the
assumption" is a repeat of the original failure with a new coat of paint.

### 3.10 "Ring" numbering names a path, not a fixed step order

Ring 1 / Ring 2 / Ring 3 in §8 name **data-source paths**, not a linear
step sequence. Ring 1 = automated via connected tools, Ring 2 = baseline
seed-and-harvest, Ring 3 = batched user ask. They run in parallel or in
whatever order the project state demands — there is no "Ring 2 comes
after Ring 1". Skills referencing the rings should phrase them as paths
("the Ring 3 user-ask path", "the Ring 1 automation path"), never as
ordinals that imply completion of N before starting N+1.

The numbering is kept for continuity with existing references, not
because it encodes a sequence. When planning intake, pick the paths the
project state activates and run them concurrently where the tools allow.

---

## 4. Core principles

### 4.1 Quality over quantity

Fewer well-chosen prompts always beat many marginal ones. Every prompt
that doesn't earn its slot costs two things: Peec plan credits, and
analytical noise that obscures signal from the prompts that matter.

The test each prompt must pass: *"Does this prompt measure a question a
real customer would ask on the way to a purchasing decision?"* If the
answer is "only loosely" or "for completeness", cut it.

### 4.2 Every prompt earns its slot

Slots are allocated based on data, not intuition. Demand signals (GSC
click share, revenue-by-landing-page, CPC × SV, AI-fanout patterns)
determine how many prompts a category deserves. Categories without
external demand signals don't get slots just because the brand offers
the product — they may be a content problem, not an AI visibility
problem.

### 4.3 Automated sources first, user asks last

The agent must check what it can already access before asking the user
for anything. Connected MCPs (Peec, analytics, search-console tools, SEO
tools, commerce platforms, crawl tools), other loaded skills (brand
dossiers, business context), and earlier conversation context all take
precedence. The user is asked for manual input only as a batched fallback
when no automated path exists. When asking, ask once, ask for the maximum
useful batch, and never trickle questions across the session.

### 4.4 Data persistence

Any data the user provides manually — domain lists, market priorities,
taxonomies, customer-voice samples, regulatory notes — is saved to the
persistence store so the user never has to provide it twice. On
subsequent runs, the agent reads the saved data first, surfaces it to
the user, and asks only whether it needs refreshing. See §7 for the
persistence mechanism (and note that the mechanism itself is agent/user
choice per §3.7).

### 4.5 Prescriptive strategy, explicit overrides

The Strategy phase outputs a concrete recommendation the user accepts or
rejects — not a menu of options with trade-offs. Each recommendation is
paired with an explicit **"Override this if…"** block that lists the
common departures. The user reads the recommendation, accepts, or calls
out an override. No "which would you like" questions during the strategy
phase.

### 4.6 Brand list reflects reality

Peec's auto-selected competitors are usually wrong — they skew toward
information sites (forums, wikis, magazines) rather than actual
commercial rivals. The real competitors are domains that **actually get
cited** when AI models answer prompts in the brand's space. On day 0,
derive the competitor list from the brand's own commercial knowledge
plus any available SERP-competitor or citation data. On existing
projects, derive from `get_domain_report` + `get_url_report`.

When reading these reports to classify domains as own / competitor /
editorial, remember that classification is a **returned column**, not a
server-side filter (see `peec-ai-mcp` §7.31). Filter by `domain` with
the full owned-TLD list (see `peec-ai-mcp` §7.10 and §8.10 steps 1/3/5)
and do any OWN/CORPORATE/EDITORIAL slicing client-side. Column names
also differ between the two reports (`retrievals` on URL, `retrieval_count`
vs `retrieved_percentage` on domain — see `peec-ai-mcp` §7.39); check
the payload before sorting or summarising.

**Three categories, not two.** The brand roster has three shapes, not
the binary "competitor vs marketplace noise" the skill's earlier wording
implied:

1. **Commercial competitor** — a distinct business the own brand is
   fighting for the same customer's wallet. Add to the roster with
   `is_own=false`; it will drive share-of-voice, gap reports, and
   competitive narrative.
2. **Assortment brand** — a brand stocked *by the own retailer* (i.e.
   it appears as a product line on the own site, often at
   `/collections/{brand}` or as a category page). Retailing the brand
   does not make it a competitor; conflating the two inflates
   competitor counts and corrupts gap analysis. Assortment brands can
   still matter for visibility tracking (a user searching for the
   brand may land on the retailer) but should be **tagged as assortment**
   so the Strategy-phase output surfaces them distinctly.
3. **Marketplace / generic noise** — Amazon, Google Shopping, generic
   directory pages. Not a competitor; not stocked; just co-retrieval
   noise.

§9.2 codifies the classification step to run before adding any brand
to the roster, and §9.3 specifies how assortment brands should map to
topics (usually as sub-topics under the parent commercial category, not
as first-class topics of their own).

### 4.7 Tags serve analysis, not categorisation

A tag taxonomy should be 2–3 dimensional (intent × funnel × category is
a common shape) and total 15–25 tags. More than that and consistency
breaks down. Every prompt should carry at least one tag from each
dimension. Avoid tag-dimension duplication (e.g. a `transactional` tag
and a `funnel:decision` tag are the same signal — pick one).

### 4.8 Topics mirror business structure

Topics are the coarse grouping used for dashboards and reporting. They
should map to the brand's internal structure so stakeholders can find
their area. Don't duplicate categorisation between topics and tags —
let topics carry business structure and tags carry cross-cutting
dimensions. 5–8 topics covers most single-market e-commerce projects;
more than 10 usually signals either multiple markets mashed together
or topics acting as tags.

**Topics are categories, not brand names.** A topic should name a
category the own brand sells into (e.g. "running shoes", "specialty
coffee beans") — never a commercial competitor's brand name. A prompt
mentioning a competitor belongs under the category topic that prompt
is about; the competitor's name belongs on the brand roster (§4.6 /
§9.2). **Assortment brands are the nuanced case:** if the own retailer
merchandises by brand (e.g. a sneaker store with per-brand landing
pages for Nike, Adidas, New Balance), it may make sense to use those
brand names as sub-topics or as tags rather than as first-class topics.
Prefer the tag approach unless the brand roster is very small and
assortment-brand coverage dominates the commercial structure.

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

### 4.10 Branded and non-branded prompts are different KPIs

This section applies the instrument-separation principle (§3.1) to KPI
reporting specifically — see §3.1 for the underlying rule.

Rolling branded-prompt visibility (questions that name the brand) into
overall visibility inflates the headline number. A project with 10
branded prompts that score near 100% and 90 non-branded prompts at 10%
looks like it has ~20% visibility when the real "unprompted discovery"
visibility is 10%. Branded and non-branded metrics must be reported
separately. Keep branded prompts small in count (5–10) and consistently
tagged so filters work.

### 4.11 Direct writes, no dry-run artifact

The skill writes directly to the Peec project rather than producing an
intermediate operations list. The strategy sign-off artefact — produced
before writes begin — is the audit trail. User sign-off happens before
writes begin; verification happens after, using `list_*` reconciliation.

### 4.12 Fresh prompts need time

Prompts start collecting data within 24 hours. The default iteration
pace is daily — the next Analyse loop can run ~24 hours after writes.
For trend-level analyses (SoV shifts, sentiment drift), 7–14 days
produces more stable signal. Don't run Analyse in under 24 hours — the
data won't be there yet. See §12.7 for the full measurement-window
guidance and §3.4 on why calendar time is a first-class resource.

---

## 5. Relationship to other skills

| Skill | Role |
|---|---|
| `peec-ai-mcp` | Companion. Covers Peec MCP tool mechanics, OAuth setup, schema quirks, gotchas. **Strongly recommended** — the Peec MCP has non-obvious quirks that this skill's cross-references depend on. |
| External-data skills (varies) | Any skill the user has for pulling search-console data, SEO-tool data, crawl data, trends data, or analytics data. This skill consumes their output; doesn't reinvent them. |
| Brand-context skills (varies) | Any skill the user has loaded that holds accumulated knowledge about the brand being tracked (brand dossier, project-context skill, or similar — however it's named in the agent's environment). Contains prior-captured context (markets, TLDs, revenue profile, regulatory notes) that bypasses intake questions. Always check. |

The `peec-ai-mcp` companion is strongly recommended but not required. It's
published at
[github.com/rebelytics/peec-ai-mcp](https://github.com/rebelytics/peec-ai-mcp).
Running this skill without it still works, but several tool-level quirks
this skill cross-references (classification vs server-side filtering,
column name inconsistencies across reports, the 24-hour refresh cadence,
`update_prompt` partial-update semantics, and more) will be rediscovered
the hard way. Load both when possible.

---

## 6. Workflow overview

The workflow runs as **Phase A** (iterative) plus an optional **Phase B**
(terminal). Phase A is where all the Peec configuration work happens;
Phase B is an opt-in stakeholder deliverable at the end.

### Phase A — iterative loop

Each loop runs four sub-phases in sequence:

1. **Intake (§8).** Gather everything the strategy needs. Three
   concentric rings on the first loop, run as parallel paths (§3.10):
   automated tools (Ring 1), a baseline path as the floor (Ring 2),
   and a batched user-provided data ask (Ring 3). Outputs a populated
   intake state, persisted per §7.
2. **Strategy (§9).** Convert intake into a prescriptive recommendation
   with explicit override callouts. User signs off (format agent/user
   choice). Outputs the strategy sign-off artefact.
3. **Write (§12).** Execute the signed-off strategy as direct writes to
   the Peec project via the Peec MCP. Outputs a configured project and
   a verification log.
4. **Analyse (§13).** Read what the Peec data actually says after the
   writes have settled. Produce findings (format agent/user choice) that
   feed the next Strategy iteration.

**Loop-awareness.** The **first loop** runs the full three-ring intake
(§8). **Subsequent loops** typically run a Peec-only intake refresh —
re-reading current project state — unless findings or the user surface a
gap that needs external data, in which case Ring 2 or Ring 3 is re-entered
for that specific gap. Strategy, Write, and Analyse run every loop, with
scope narrowed to what changed.

**Termination.** Phase A ends when findings no longer produce material
action for the next Strategy iteration — the strategy has stabilised.
That's also when Phase B becomes relevant.

### Phase B — optional, terminal

Once Phase A has stabilised, the agent proactively offers a
stakeholder-facing presentation of the strategy and its findings (see
§14.5 for the timing rules). Phase B is a single pass from existing
Phase A artefacts — it doesn't re-do any Phase A work. See §14 for the
full rules, including why Phase B is intentionally terminal rather than
mid-engagement.

### Routing by project state

Phase A's Write sub-phase routes on project state:

- **New project** (zero prompts at first loop) → first Write is pure
  creates.
- **Existing project** → first Write uses the prompt disposition
  framework (§10) — six buckets covering what to keep, retag, reframe,
  or remove.

Subsequent loops always run in existing-project mode; the disposition
framework is reapplied to the prompts that have been in place long enough
to have meaningful data.

### Pacing disclosure at intake

At the start of the first Intake loop, the agent surfaces the iterative
model and the iteration cadence to the user:

- The default pace is daily iterations — Peec processes new prompts
  within 24 hours, so the next Analyse loop can run the following day
  (see §12.7).
- For trend-level analyses (SoV shifts, sentiment drift), a 7–14 day
  window produces more stable signal and can be used when the question
  requires it.
- The number of loops is driven by findings, not by a preset timeline.
- Once the strategy has stabilised across a few iterations, the agent
  can produce a polished stakeholder-facing presentation of the
  strategy and its findings (Phase B, §14). That comes later — first
  the tracking needs to be right. Phase B is not committed to at
  intake, but the user should know the capability exists.

This framing sets expectations before the user has anchored on a linear
"do it once and we're done" model.

---

## 7. Data persistence

User-provided data is saved between runs so the user provides context
once, not once per session.

**What, not how.** This skill requires *that* intake state persists; it
doesn't require a specific file format or directory layout. The
`intake.yaml` schema below is one concrete example. Other equally valid
approaches: handoff docs pasted session-to-session, memory systems,
journal-style markdown notes, or whatever fits the user's environment.
In environments without persistent storage (web chat interfaces), a
detailed handoff doc at the end of each session is required so the next
session can resume without re-entering data.

The principle: every loop should be able to read the previous loop's
intake state, strategy sign-off, and findings without asking the user
to re-provide them. The mechanism is agent/user choice.

### 7.1 Where data is saved (illustrative layout)

```
<workspace>/<brand-folder>/peec-strategy/
  intake.yaml                — current intake state (canonical, read first)
  intake-history/             — dated snapshots for diffing
    intake-YYYY-MM-DD.yaml
  strategy-YYYY-MM-DD.md     — strategy sign-off artefact for loop N (optional)
  findings-YYYY-MM-DD.md      — Analyse output for loop N (optional)
  verification-YYYY-MM-DD.md  — post-write reconciliation log
```

This is one concrete example, not a mandated structure. If no brand-specific
folder exists yet, ask the user which folder name to use (defaulting to a
slugified form of the brand name) and create the structure. If a
brand-specific directory already exists from other work, nest
`peec-strategy/` inside it rather than creating a parallel tree.

### 7.2 What `intake.yaml` holds (illustrative schema)

The schema below uses a fictional brand — Northwind Coffee Co., a specialty
coffee retailer with European markets and sister brands in tea and brewing
equipment — purely as an illustrative example. Adapt the field set to the
brand being tracked.

```yaml
brand:
  name: Northwind Coffee Co.
  primary_domain: northwindcoffee.com
  owned_domains:
    - northwindcoffee.com
    - northwindcoffee.de
    - northwindcoffee.co.uk
    # ... all TLDs
  aliases:
    - Northwind Coffee
    - Northwind Roasters
  regex: null
  regulatory_context: null   # populate if the brand operates in a regulated
                             # vertical (pharma, financial products, etc.);
                             # null for non-regulated brands like this one

markets:
  - country_code: DE
    priority: 1
    revenue_share_pct: 42
  - country_code: UK
    priority: 2
    revenue_share_pct: 28
  - country_code: NL
    priority: 3
    revenue_share_pct: 15

competitors_known:
  - name: Contoso Coffee
    domains: [contoso-coffee.com]
    aliases: []
    relationship: direct_competitor
  - name: Northwind Tea
    domains: [northwindtea.com]
    relationship: sister_brand   # ← persisted, routes differently in reports

sister_brands:
  - Northwind Tea
  - Northwind Brewing Equipment

existing_taxonomy:
  source: supplied_csv
  provided_on: 2026-04-20
  tag_dimensions:
    - intent
    - funnel
    - category

customer_voice_samples:
  provided: false
  last_asked: 2026-04-20

data_sources_connected:
  - peec_mcp
  - brand_context_skill: northwind-coffee-context   # generic name — use
                                                    # whatever the loaded
                                                    # brand-context skill is
                                                    # called in the agent's
                                                    # environment
  - web_search_web_fetch
  # optional enrichments marked absent if not available:
  - gsc: not_connected
  - seo_tool: not_connected
  - analytics: not_connected

# Per-source disposition — mandatory table, one row per Ring 3 data category.
# Each row records whether that data was received, explicitly declined in
# writing, or is still outstanding. No row may be "deferred to Loop 2" as a
# self-granted skip — only the user can decline.
ring3_data_disposition:
  - source: xml_sitemap
    status: received       # received | declined_by_user | outstanding
    provided_on: 2026-04-20
  - source: gsc_queries
    status: declined_by_user
    declined_reason: "No GSC access; will run on later loop after access set up"
  - source: gsc_pages
    status: outstanding
  - source: keyword_tool_export
    status: outstanding
  - source: revenue_by_landing_page
    status: outstanding
  - source: margin_by_product_line
    status: outstanding
  - source: crawl_export
    status: not_applicable  # use when no crawl tool is available for this project
  - source: customer_voice_samples
    status: outstanding
  - source: competitor_faq_urls
    status: outstanding
  - source: brand_positioning_doc
    status: received
    provided_on: 2026-04-20
  - source: regulatory_notes
    status: not_applicable  # non-regulated vertical

last_refreshed: 2026-04-20
```

The schema is illustrative. Use whatever structure fits — the requirement
is that future loops can read it without re-asking.

**The `ring3_data_disposition` table is not illustrative — it is
mandatory.** Every Ring 3 data category must appear as a row with a
disposition of `received`, `declined_by_user`, `outstanding`, or
`not_applicable`. "Deferred to Loop 2" is **not a valid disposition**
— if data isn't available for Loop 1 and the user hasn't declined in
writing, the correct state is `outstanding` and Strategy cannot proceed
until it becomes `received` or `declined_by_user` (see §3.8 / §3.9 /
§8.4). This is the persistence-layer enforcement that closes the
"planned deferral" skip shape (§3.8).

### 7.3 Refresh logic

On each loop, the agent:

1. Reads the persisted intake state.
2. Calculates age: `last_refreshed` vs today.
3. If age > 90 days, prompts the user to confirm refresh of any field
   that may have drifted (markets, competitors, revenue share).
4. If age ≤ 90 days, surfaces the existing data to the user in the
   next Strategy iteration without re-asking.
5. Persists any new fields provided this session, saving a dated snapshot
   before overwriting the canonical file.

The user should never be asked for data that's already persisted unless
the data is stale or the user explicitly wants to update it.

### 7.4 Handoff-doc fallback for non-persistent environments

In environments without persistent storage (web chat interfaces), the
agent must produce a handoff doc at the end of each session that captures:

- Current intake state (brand config, markets, competitors,
  regulatory context).
- Latest strategy sign-off summary.
- Latest findings and what they imply for the next loop.
- Earliest sensible re-analysis date (per §12.7).
- Pending verification items.

The next session starts by reading the handoff doc back in. This is less
seamless than a persistent file, but preserves the core requirement:
don't re-ask for data the user has already provided.

---

## 8. Phase A — Intake

Goal: populate the intake state and the project-state snapshot for this
loop.

**First loop** runs the full three-ring intake below: Ring 1 first,
Ring 2 as the floor, Ring 3 as the only user-facing ask.

**Subsequent loops** typically run a Peec-only refresh — re-reading
`list_brands`, `list_topics`, `list_tags`, `list_prompts`, and the
current-state summary — unless a finding from the previous Analyse (§13)
surfaces a gap that external data could close. In that case, re-enter
Ring 2 or Ring 3 for that specific gap only. The skill does not require
re-running the full three-ring intake on every loop.

Pacing disclosure (see §6): at the first Intake, the agent tells the user
that the default iteration pace is daily (Peec processes new prompts
within 24 hours), that longer 7–14 day windows are available for
trend-level analyses, that the number of loops is driven by findings
rather than a preset timeline, and that once the strategy stabilises the
agent can produce a stakeholder-facing presentation (Phase B, §14).

### 8.1 Ring 1 — Automated via connected tools and skills

Before touching the user, check each of these. Skip any that isn't
available; don't ask the user whether they're available — inspect the
environment.

#### Known MCP gaps — check before asking the user

Before asking the user for any information, the agent must attempt to
derive it from the Peec MCP. Several fields that seem like they should
be available are not. The table below enumerates the known gaps so the
agent checks, confirms the gap, and frames the user ask as "the MCP
doesn't expose this" rather than "tell me":

| Field | MCP tool to check | What it returns | Gap |
|---|---|---|---|
| **Project country / market** | `list_projects` | `{id, name, status}` only | No country, language, or market metadata at project level. Ask the user: "Peec's MCP doesn't expose project-level market data, so I need to confirm: which markets should this strategy prioritise?" |
| **Plan tier / prompt credits** | `list_projects` | `{id, name, status}` only | No plan data. `get_credit_balance` does not exist (see `peec-ai-mcp` §7.37). Ask the user: "Peec's MCP doesn't expose plan credits. How many prompts does your plan allow?" Single precise question, not a multi-tier multiple choice. |
| **Active engine count (proxy for plan gating)** | `list_models(is_active=true)` | Active engine list | ✓ Available. 3 or fewer = likely plan-capped (§9.6). |

When asking the user about a known MCP gap, always prefix with the
reason: "The Peec MCP doesn't expose this at the project level, so I
need to confirm with you:" — this reframes the ask from "I didn't check"
to "the platform doesn't expose this" and teaches the user where the gap
lives. Persist every answer to the intake state (§7) so subsequent loops
don't re-ask. Surface plan-related questions (prompt credits + engine
count) together so the user answers all plan constraints in one go.

#### Peec MCP (always available when this skill runs)

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

Resulting project-state classification:

- **New project:** zero prompts, auto-suggested brands still present.
  The Write sub-phase (§12) will be pure creates.
- **Existing project:** prompts already in place. The Write sub-phase
  (§12) will use the disposition framework (§10).

#### Loaded skills

Scan the active skill list. Specifically check for:

- **Brand-context skills** — any skill that holds accumulated knowledge
  about the brand being tracked (brand dossier, project-context skill,
  or similar — however it's named in the agent's environment). Captures
  markets, TLDs, revenue profile, regulatory notes, sister-brand
  relationships.
- **Portfolio / group-context skills** — where the brand is part of a
  larger group, the agent may have a shared-context skill covering
  sister brands, shared infrastructure, and group-level strategic themes.
- **Business-strategy / commercial-context skills** — any skill capturing
  the commercial priorities, capacity, or revenue goals that inform
  allocation decisions.

If a brand-context skill exists, read it first and populate intake state
from it before asking the user anything.

#### Conversation context

Scan the current session transcript for already-provided context —
brand name, markets, strategic priorities, known competitors.
Populate intake state from this before re-asking.

#### Optional connected MCPs (skip if not connected)

- **GSC MCP** — pull top queries over 12 months per market via the API
  (not subject to the ~1000-row UI export cap; pull several thousand if
  useful). Filter for question-shaped queries
  (`how|what|why|is|does|should`) and commercial intent. Feeds the
  volume-signal and allocation steps in the Strategy sub-phase (§9).
- **SEO-tool MCPs** — CPC × SV for commercial weighting, SERP competitors
  for the brand's keyword universe, AI Overview presence flags.
- **Analytics MCPs (GA4, Adobe, Matomo, Piwik Pro, or equivalent)** —
  landing-page sessions, key events, revenue. The **strongest commercial
  signal** for e-commerce — prioritise over keyword-demand signals where
  both are available (see §8.2).
- **Commerce-platform MCPs** — revenue by category, product catalogue.
- **Crawl-tool MCPs (Screaming Frog, Sitebulb, or equivalent)** — content
  inventory, page type distribution, structural content gaps.
- **Community/forum access (MCP or agentic browser extension)** —
  community question-mining. If any such tool is connected, use it;
  Reddit in particular is not directly accessible via the baseline web
  tools (see Ring 2 notes).

### 8.2 Ring 2 — Baseline path (always works, no external dependencies)

When Ring 1 returns a sparse picture, the baseline path produces enough
signal to build a competent strategy. Every agent running this skill
can execute all six steps — no MCPs beyond Peec, no user-specific tool
access.

#### Step A: Peec seed-and-harvest (primary day-0 signal)

This is the load-bearing step for new projects and the richest
free-standing demand signal available.

1. **Day 0:** create 10–15 deliberately broad discovery prompts across
   the brand's rough topic clusters. Prompt shapes should be generic:
   "what is X", "best X for Y", "X vs Y", "recommendations for X",
   "how to choose X". Tag them `seed:harvest` and set `country_code`
   from the primary market. These prompts join the 24-hour cycle
   immediately (see `peec-ai-mcp` §7.6).
2. **Day 1 (~24 hours later):** for each seed chat, call
   `list_search_queries(chat_id=...)`. This returns the sub-queries the
   AI engine fanned out to when answering. **This is the highest-fidelity
   demand signal available without paid tools** — it's what the AI
   itself considers the adjacent questions in the category.
3. Also call `list_shopping_queries(chat_id=...)` for any shopping-mode
   chats — commercial-intent queries, often product-specific.
4. Mine the fanout for platform patterns: `site:reddit.com`,
   `site:amazon.*`, `site:youtube.com`, `site:quora.com`. These are the
   platforms the AI believes hold answers in the vertical. High Reddit
   fanout with no brand presence on Reddit is a distribution signal
   (see §11 pattern library).
5. **Seed prompt lifecycle:** at Strategy time (§9), decide per
   seed prompt — keep it if it fits the final strategy and the topic
   survives, delete it if not. Seed prompts are not automatically
   preserved.

For existing projects, the seed-and-harvest step is optional — fanout
data can be mined from existing chats instead. Use seed-and-harvest
when the existing prompt set doesn't cover a category the strategy
needs to explore (e.g., the brand is entering a new vertical).

#### Step B: Competitor FAQ scraping

For each of the brand's top 3–5 known commercial competitors:

1. `WebSearch("[competitor name] FAQ")` — finds the competitor's FAQ
   URL.
2. `WebFetch(<faq url>)` — extracts the question headings.
3. Supplement with `WebSearch("[competitor name] blog")` for top
   listicles; `WebFetch` the most prominent for H2/H3 question
   structures.

Competitor FAQs are the most direct supply-side signal of "we get
asked this a lot". Limits: corporate domains are nearly always
accessible via WebFetch; community platforms (Reddit, Quora) are often
blocked — see Step E below.

#### Step C: AI self-report (coverage check)

The agent uses its own reasoning inline during the session. Prompt:

> *"For a [brand type] operating in [markets], list 30 question-shaped
> queries someone might ask an AI model before buying in this
> category. Include a mix of awareness, consideration, comparison,
> and purchase-intent queries. Flag any queries that touch regulated
> or grey-area topics."*

Generated inline — no external model call, no artifact, just reasoning.
Used as a coverage check: after baseline-path outputs are aggregated,
compare against this list to spot blind spots.

This signal is weak relative to fanout data and competitor FAQs, but
it's free and instant. Treat it as a gap-check, not a seed source.

#### Step D: YouTube autocomplete + top video titles

For the brand's top 2–3 categories:

1. `WebSearch("[category] YouTube reviews")` — surfaces top-ranked
   videos whose titles are question-shaped.
2. Optionally `WebFetch(<youtube url>)` on the top 3 results — some
   video pages return transcripts / descriptions that reveal the
   question space.

Video is a major AI citation source (ChatGPT and AI Overview cite
YouTube heavily in product categories). Video titles are a secondary
demand signal.

#### Step E: Reddit / community signals (only via connected tooling)

**Reddit is not directly accessible via the baseline WebFetch or
WebSearch tools.** As of this skill's current release, WebFetch
refuses `reddit.com` and `old.reddit.com` at the tool level;
WebSearch doesn't support `site:` operators and doesn't surface
Reddit organically for typical queries. Re-test if a later agent /
tooling generation unblocks Reddit access.

So Reddit stays out of the baseline path. Three fallbacks:

1. **Connected Reddit tooling, if present.** If a Reddit MCP is
   connected, use it. If the agent has an agentic browser extension
   or equivalent that can browse Reddit directly, use that. Mine the
   2–3 relevant subreddits for top-year threads.
2. **Peec fanout proxy.** If the seed-and-harvest (Step A) shows
   `site:reddit.com` in the fanout queries, that's the Reddit-demand
   signal — delivered through Peec without needing direct Reddit
   access.
3. **User-provided.** If the user has scraped or exported Reddit data
   (rare), consume it via Ring 3.

Quora and Stack Exchange are usually accessible via WebFetch — try
them for technical/niche verticals, but don't rely on them for general
e-commerce.

#### Step F: XML sitemap baseline (see §8.3.2)

URL-structure baselining from the XML sitemap is a Ring-2 automated
step but is documented once, in §8.3.2, because it precedes the Ring 3
user ask directly. Run it before composing Ring 3.

#### Step G: User's domain knowledge (batched ask — see Ring 3)

The one user-facing ask happens at the end of intake, not throughout.
See Ring 3 for the question set.

### 8.3 Ring 3 — User-provided data (batched ask at end of intake)

**On the first loop, Ring 3 is fired regardless of Ring 1 richness.**
"The user has no data" is a valid answer; "the agent decided Ring 1 was
enough" is not. See §3.8 for why this is a hard rule.

Ring 3 runs in four sub-steps. The sequence enforces §4.3 ("automated
sources first, user asks last") structurally rather than by prose alone.

**Two structurally different asks — don't collapse into one widget.**
The Ring 3 user-facing ask has two functionally distinct parts that
must not be merged into a single AskUserQuestion call:

- **The data request** (§8.3.3a) — "please attach these files /
  confirm each data category is unavailable". Attachment-centric,
  plain-text, cannot be compressed into a multiple-choice widget.
  The user needs to see the full list of what they could provide and
  respond item-by-item.
- **The scoping widget** (§8.3.3b) — "how many prompts? which
  markets? which brand priorities?". Binary or short-choice questions
  well-suited to AskUserQuestion, with explicit override slots per
  §4.5.

Collapsing these into one widget is the known failure mode: the
widget surface can represent scoping choices but not attachment
requests, so the data request is silently dropped. The split is
load-bearing; keep it explicit.

#### 8.3.1 Automated inventory (before composing the ask)

Before composing the Ring 3 question, the agent must enumerate what data
is already accessible for this brand. Check each of:

- **Brand-context skills** — read each loaded brand-context skill for
  referenced files, existing tool access, data-layer docs, crawl
  configs, taxonomy documents.
- **Tool-reference skills** — check each tool-specific skill for
  credentials, project IDs, and access patterns that bypass a user
  export (e.g. a search-console MCP reference, an AI-visibility-platform
  API reference, an analytics MCP reference).
- **Workspace folders** — scan the brand-specific folder for CSVs,
  JSONs, MDs, XLSXs that predate this session.
- **Connected MCPs** — list every connected MCP tool and note which
  could supply Ring 3 data (analytics, search console, rank tracking,
  CRM, community/forum access).

Output: an "already accessible" block in the Intake summary (§8.4),
with file paths and tool names. This block feeds the targeted ask below.

#### 8.3.2 Automated URL-structure baseline (sitemap-first)

Before composing the user-facing ask, run the sitemap-based URL
structure baseline. This replaces the earlier framing of "ask the user
for crawl data" — crawl tools (Screaming Frog, Sitebulb, or equivalent)
are the *bonus*, not the source.

1. `WebFetch("<domain>/sitemap.xml")` or fetch via bash. If it returns
   a sitemap index, fetch the top 2–3 nested sitemaps (typically
   `sitemap_pages.xml`, `sitemap_posts.xml`, `sitemap_products.xml` or
   similar).
2. Extract URLs and classify by path pattern (product URLs, category
   URLs, editorial URLs, help-centre URLs). This produces a coarse
   page-type distribution without crawling content.
3. Feed the distribution into §9.5 (topic structure) and the source
   authority inputs for §13.9 (URL gap analysis) / §13.15 (own-brand
   URL citation map).

**This is available on every commercial site without user effort and
should be the starting point for URL-structure discovery on every
project.** Where Screaming Frog, Sitebulb, or equivalent crawl data is
available (via Ring 1 brand-context skills or Ring 3 user provision),
use it to *enrich* the sitemap baseline — adding
page-type-segmentation, broken-link detection, and indexability flags
that the sitemap alone can't give you. Crawl data is never a
substitute for the sitemap baseline because it's not always available;
it's additive when it is.

**What the sitemap doesn't replace:** internal linking, crawlability
data, rendered content, structured-data coverage. When a full crawl is
available, prefer it for those specific signals.

**Scope caveat:** sitemaps may be truncated, outdated, or exclude
noindex URLs. Treat the URL list as "publishable pages the site wants
indexed", not as the full content footprint.

#### 8.3.3a Targeted data request (attachment-centric, plain text)

After sitemap baselining (§8.3.2), the agent composes the data request
as **plain text** listing every category the user could provide an
attachment or export for. This is the "please send me these files"
ask — not a widget. Use AskUserQuestion ONLY for the scoping widget in
§8.3.3b; the data request must be a separate plain-text message so the
user can respond item-by-item with attachments.

Frame each option with what's already known — e.g. "I already have
your crawl export and taxonomy doc from the brand folder; do you also
have a fresher search-console export or should I use the existing
one?" instead of "Do you have crawl data?"

**Mandatory template for the data request.** The plain-text ask must
enumerate every row from the `ring3_data_disposition` table (§7.2) that
is currently `outstanding`. For each row, state:

- What the data is (plain language).
- How to provide it (CSV attachment, URL, pasted text).
- Why it matters (one sentence connecting to Strategy).
- That "I don't have this / not applicable" is a valid response.

The user's reply then populates the `ring3_data_disposition` table —
each row becomes `received` (data provided), `declined_by_user` (user
explicitly declined in writing), or `not_applicable`. Rows the user
doesn't address remain `outstanding`, and Strategy cannot proceed.

The standard categories to check for gaps (skip any already covered by
§8.3.1):

1. **SEO tool data** — Ahrefs, Semrush, DataForSEO, or equivalent.
   Adds AI Overview presence flags, CPC × SV, SERP competitor context.
2. **Search Console data** — GSC export or direct access. Top queries
   over 12 months. Note: the GSC UI export is capped at ~1000 rows.
   To go beyond that — pull several thousand queries if useful — use
   the GSC API (directly, or via a connected GSC MCP).
3. **Web analytics revenue data** — GA4, Adobe, Matomo, Piwik Pro.
   Landing-page revenue by category. **Strongest commercial signal
   for e-commerce.** Handling notes:
    - **Exclude navigational / utility from the denominator.** Homepage
      (`/`), checkout, account, login, password recovery, wishlist, site
      search results, and loyalty program URLs capture revenue from
      users who already know the brand and aren't discoverable via AI
      visibility. Compute category allocation against the "discoverable
      content" subset only.
    - **Classify product detail pages into categories.** Product detail
      pages typically outnumber category pages 10:1 and aggregate to a
      significant share of revenue. Use URL keywords, breadcrumb
      structure, or crawl data to classify each product URL into a
      category — otherwise the allocation table underweights categories
      that earn their revenue through individual product pages.
    - **Watch for transient / price-driven revenue clusters.** Sale,
      clearance, outlet, and discount sections often concentrate
      significant revenue into a single URL (`/sale`, `/clearance`,
      `/outlet`). These represent a *buying behaviour* signal that no
      keyword tool will surface because they aren't keyword-driven —
      users respond to price framing. Treat them as their own intent
      cluster (see §11.15).
    - **Understand attribution.** GA4's default is last-non-direct
      click. Categories that sit late in the purchase journey (checkout,
      account, loyalty) get over-credited; categories early in the
      journey (blog, informational content) get under-credited. The
      AI-visibility journey is typically early-to-mid-funnel, so tilt
      the allocation toward categories that convert from first-visit or
      mid-journey landings.
    - **Don't shrink a category to zero on revenue alone.** A category
      at €0 revenue may still be strategically important (emerging
      category, category the client wants to enter). Keep 2 diagnostic
      prompts so the AI visibility gap stays measurable.
    - **Cross-check against GSC.** If GSC clicks and GA4 revenue agree
      on rank order, confidence is high. When they disagree, GA4 is
      usually the better guide for allocation on e-commerce — but note
      the disagreement in the intake record so the reasoning is
      traceable.
    - **Non-e-commerce equivalent:** for services, B2B, or SaaS, the
      mirror signal is "leads/signups per landing page" or "pipeline
      value per landing page" from CRM/analytics. Same principle —
      measure outcome, not demand.
4. **Website crawl data** — Screaming Frog, Sitebulb, or equivalent
   crawl export. Page type distribution, content gaps,
   product/category inventory.
5. **Customer-voice data** — sales call transcripts (Gong, Chorus,
   Fireflies), support tickets (Zendesk, Intercom), site search query
   logs, post-purchase survey text, live chat logs. Even 100–500 rows
   matters.
6. **Brand-specific context** — buyer personas, common prospect
   questions, existing taxonomy, brand guidelines, regulatory notes,
   competitor intelligence.

#### 8.3.3b Scoping widget (AskUserQuestion)

The scoping widget uses AskUserQuestion for short-choice questions
about scope, budget, priority, and positioning. This is a **separate
call** from the plain-text data request in §8.3.3a — don't merge them.

Typical scoping questions (omit any already answered by existing
intake / brand-context skill):

- Country scope — single market, primary + secondary, multi-market?
- Prompt budget — how many prompts should the plan cover in Loop 1?
- Engine priority — should the strategy privilege specific engines (e.g.
  ChatGPT + AI Overview for a DE e-com site) or spread evenly?
- Brand positioning — customer-facing vs. enterprise-facing, premium vs.
  value, etc.
- Assortment-brand handling (§4.6 / §9.2) — should stocked brands be
  tracked as roster brands, used as tags, or omitted entirely?

Each AskUserQuestion question should carry an explicit "Override this
if…" slot per §4.5, so the user can divert from the recommended default
without being trapped in a multiple-choice menu.

#### 8.3.3c Open-ended data-source invitation

At the end of the scoping widget, include an open-ended option: "What
other data sources do you think would help? Examples: existing prompt
sets in other AI-visibility platforms, industry report subscriptions,
internal taxonomy docs, CRM segmentation exports, sales call
libraries, etc." This invites user expertise rather than assuming the
skill's enumerated list is complete. The user has context the skill
doesn't — the skill should invite that context, not cap it.

#### Ring 3 handling

Two distinct messages in sequence: §8.3.3a (plain-text data request)
followed by §8.3.3b (scoping widget). Both must fire before Strategy.
Per §3.8, the data request is not optional even if Ring 1 is rich.

If the user provides data, save it to the persistence store (§7) so
subsequent runs don't re-ask. Specifically:

- CSV/XLSX exports go under the persistence store's user-provided
  subdirectory (or equivalent per §3.7 — mechanism is agent/user
  choice).
- Free-text responses go into the intake state in the relevant field.
- Dated snapshots preserve what was provided when.
- The `ring3_data_disposition` table (§7.2) is updated row-by-row
  based on the user's reply — `received`, `declined_by_user`, or
  `not_applicable`. Rows without explicit disposition remain
  `outstanding` and block the Strategy gate.

### 8.4 Intake summary output (mandatory gate for §9 Strategy)

At the end of the Intake sub-phase, the agent writes:

- Updated intake state (persisted per §7).
- A **mandatory** intake summary block in the session output. Strategy
  (§9) cannot begin until this block has been written and contains
  non-empty entries — or explicit "skipped because …" rationales — for
  every row below. If any Ring 2 or Ring 3 row is absent rather than
  explicitly skipped, return to Intake.

Required rows:

  - Loop number (first loop vs subsequent).
  - Project state (new / existing).
  - Ring 1 tools that populated data.
  - Ring 2 steps completed (first loop only). Each of Steps A–F must
    appear with a result or an explicit skip rationale.
  - Ring 3 automated inventory (§8.3.1) — what data was already
    accessible.
  - Ring 3 sitemap baseline (§8.3.2) — URL structure extracted from
    `<domain>/sitemap.xml` with page-type distribution.
  - **External data received — itemised.** Reproduce the
    `ring3_data_disposition` table (§7.2) verbatim, one row per data
    category, each marked `received` / `declined_by_user` /
    `outstanding` / `not_applicable`. This row is the authoritative
    check that closes the "planned deferral" skip shape (§3.8 shape 3):
    a blanket "Ring 3 user-provided data: confirmed unavailable" is no
    longer an acceptable summary — every category must be listed with
    its specific disposition. If any row is `outstanding`, return to
    §8.3.3a.
  - Gaps: which data sources returned nothing (and how the strategy
    will compensate).
  - Pre-Write quality gates (§15.1) — the agent must write the
    checklist results into the Intake summary block verbatim, including
    ✓ / ✗ marks and the rationale for any ✗. This turns the checklist
    from an internal practice into a visible artefact the user can
    inspect.

This block feeds the Strategy sub-phase (§9) and eventually any
stakeholder-facing intake-and-data-sources summary in Phase B.

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

The funnel split above is the **primary** axis. As of April 2026, Peec
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

## 11. Pattern library

Meta-patterns observed across strategy builds. Each is a recognisable
signature with a diagnosis and a recommended action. Extend as new
patterns emerge.

### 11.1 Multi-TLD footprint underspecification

**Symptom:** Own brand's `domains` array contains only the primary TLD,
but the brand operates multiple TLDs.

**Diagnosis:** Domain classification in `get_domain_report` /
`get_url_report` treats non-listed own-TLDs as `CORPORATE`, not `OWN`.
Cross-TLD mentions silently misread as competitive (see `peec-ai-mcp`
§7.10).

**Action:** During Intake (§8), enumerate all owned TLDs from the
brand-context skill or direct user ask. Populate `domains` fully at
project
setup, persist in the intake state. For existing projects, surface as a
required `update_brand` in the Write sub-phase (§12) — this triggers
background recalculation (see `peec-ai-mcp` §7.19), so batch all
`name`/`aliases`/`regex`/`domains` changes into a single call.

### 11.2 Sister-brand misclassification

**Symptom:** Brands owned by the same parent group tracked as
competitors without any "sister" distinction. Share-of-voice and gap
reports show sister brands as taking share.

**Diagnosis:** Peec's data model has no `is_sister` flag. All non-own
brands are "tracked" in the same bucket. AI responses don't understand
corporate ownership — they optimise for brand-name match against the
query.

**Action:** Two options:
1. **Prefix sister-brand `name` with `[Sister]`** in Peec so reports
   self-document. **Critical:** the `aliases` array must be populated
   in the same update call with the original brand name, or brand
   detection breaks silently (see the "Critical detail" below).
2. **Maintain a separate sister-brand list** in the intake state and
   post-filter reports.

Either way, the intake state records which competitors are sisters.
Surface in the Strategy sign-off: sister-brand visibility is not a
competitive loss at group level; group-level strategy discussions should
precede any "divide territory" recommendations.

**Critical detail on option 1 — aliases must be set simultaneously.**
Peec's brand-matching respects the `name` field for detection. Prefixing
the name with `[Sister]` without also populating the `aliases` array to
preserve the original brand name causes **detection to break silently**:
the prefixed name no longer matches mentions in AI responses, and the
brand's visibility metrics drop to near-zero. The prefix must be a
pure cosmetic label, not a detection-changing edit.

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
don't guarantee atomicity across separate calls). The verification
(step 3) is the only reliable signal that the cosmetic change didn't
break detection.

### 11.3 Tag taxonomy dimension duplication

**Symptom:** Multiple tags measure the same dimension (e.g.
`transactional` + `funnel:decision`, or `informational` +
`funnel:awareness`). Reports filtered by one give different counts
than reports filtered by the other.

**Diagnosis:** Taxonomy grew by accretion. Older flat-tag scheme was
never retired when the newer structured scheme was added. The two
schemes measure overlapping but non-identical prompt sets.

**Action:** Run the taxonomy hygiene check (§9.4). For each duplicated
dimension, propose a single tag to retain, retag affected prompts, and
`delete_tag` the retired one. Apply **before** reports are built on
the new taxonomy.

### 11.4 Zero-visibility = gap-to-close, not dead weight

**Symptom:** 25–50% of prompts show 0% visibility after 7–30 days.

**Diagnosis:** Not all zero-visibility prompts are equal. Some are
educational (prompt produces Wikipedia, not retailer mentions — §10
"Remove"). Some are structural (category that the model doesn't
understand the brand operates in — §10 "Keep as diagnostic"). But a
substantial chunk are legitimate commercial prompts where the brand
**ought** to appear but doesn't yet. Those are gap-to-close.

**Action:** Strategy (§9) classifies each zero-visibility prompt into
the right bucket. The gap-to-close bucket feeds the content strategy
findings section — each gap becomes an editorial opportunity.
Don't delete gap-to-close prompts; they're the metric that tracks the
content strategy working. Subsequent Analyse loops watch this cohort
for movement.

### 11.5 Branded / non-branded reporting split

**Symptom:** Headline visibility metric looks reasonable (~15–20%) but
drops sharply when branded prompts are filtered out (<5%).

**Diagnosis:** Branded prompts score near 100% by construction. Mixing
them into the overall visibility number inflates the headline and
obscures the "unprompted discovery" signal — which is the real metric
for a commercial strategy.

**Action:** Strategy recommendation §9.7 — report branded and
non-branded as separate KPIs with the strict-with-disclosure rule
(§3.1, §9.7). Tag branded prompts consistently (`funnel:branded` +
`intent:branded`). Keep branded prompt count low (5–10) and
concentrated in one topic to avoid spray-bias.

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

### 11.7 Regulatory-aware sentiment

**Symptom:** Small cluster of prompts shows sentiment well below 50
(neutral) — 15–30 range. Inspection shows AI models mention the brand
with cautionary framing ("be aware of legal status", "not available
for legal purchase in…", etc.).

**Diagnosis:** Brand operates in a regulated or grey-area vertical
(cannabis, pharma, gambling, adult, certain financial products). AI
models correctly add legal-caution framing, which Peec scores as
negative sentiment. This is not a reputation problem.

**Action:** Add a `regulatory:restricted` (or similar) tag on day 0.
Filter this tag out of headline sentiment reports. In the Strategy
sign-off, note that for this brand "sentiment" has two distinct
cohorts: a commercial-reputation cohort where the metric is
actionable, and a regulatory-framing cohort where low sentiment is
expected and correct model behaviour.

### 11.8 Fanout exposure without platform presence

**Symptom:** `list_search_queries` shows frequent `site:reddit.com`,
`site:amazon.*`, or `site:youtube.com` fanout queries, but the brand
has zero presence on those platforms.

**Diagnosis:** AI models believe the answer to the question lives on
those platforms. The brand is structurally absent from the retrieval
surface the model expects.

**Action:** This is NOT a Peec operation. Flag in the Strategy
sign-off's content strategy findings section. Either invest in
platform presence (curated Reddit profile, Amazon seller presence,
YouTube channel) or accept the category isn't winnable via AI
visibility without that investment.

### 11.9 Dead-weight listicle

**Symptom:** Own-brand URL has high retrieval rate but zero citation
rate. Usually a blog listicle.

**Diagnosis:** Listicle is "about competitors" rather than "about
us" — the model fetches it for context about named competitors but
doesn't credit the host brand.

**How to detect (report recipe):** Use `get_url_report` with `filters:
[{field: "domain", operator: "in", values: <own_brand.domains>}]` — **not**
a classification filter; classification isn't server-side filterable
(see `peec-ai-mcp` §7.31). Sort by `retrievals` (integer, URL report
column — see `peec-ai-mcp` §7.39). Then calculate
`citation_rate = citations / retrievals` client-side and flag URLs
where retrievals are high and citation_rate ≈ 0. `peec-ai-mcp` §8.10
steps 1, 3, 5 walk through this exact recipe.

**Action:** Editorial rewrite recommendation (not a Peec operation).
Add own-brand centred sections; tilt editorial voice to position the
brand as an answer, not a tour guide. Surface in content strategy
findings.

### 11.10 Structural-zero category

**Symptom:** A category has 0% visibility across all prompts and all
models, but the brand genuinely sells products in that category.

**Diagnosis:** Either the brand's positioning isn't understood by the
model (general retailer trying to show up in a vertical retailer's
space) or the category's content/authority on the brand's site is too
thin.

**Action:** Keep 2 diagnostic prompts in the category — don't delete
them, so the gap stays visible. Flag for content audit. Don't expand
the slot allocation until the content gap is closed.

### 11.11 Per-engine retrieval personality profiling

**Symptom:** Aggregate visibility masks very different per-engine
behaviours. ChatGPT retrieves heavily from UGC; AI Overview leans on
editorial domains; Perplexity favours documentation/primary sources.
Headline metrics average these into meaningless numbers.

**Diagnosis:** Each engine has a distinct "retrieval personality" —
preferences for source types, citation behaviours, and query
reformulation patterns. Treating the engine layer as homogeneous hides
structural differences.

**Action:** In the Analyse sub-phase (§13), run per-engine breakdowns
on at least visibility, SoV, and source-domain classification. In the
Strategy sign-off, note engine-specific recommendations where the
difference is actionable (e.g. "win AI Overview by increasing editorial
domain coverage; win ChatGPT by addressing Reddit absence").

### 11.12 Narrative ossification risk

**Symptom:** Phase B deliverable was committed to early (at intake or
after loop 1). By loop 3, Phase A findings have evolved, but the
stakeholder narrative is locked — editing it means "breaking the
story". Findings get softened to fit the deck.

**Diagnosis:** Committing to a stakeholder narrative before the
underlying Phase A data has stabilised creates a reverse-causality
pressure: findings get shaped by the story instead of the other way
round. The skill's integrity decays.

**Action:** Strictly enforce §14.1 — Phase B runs *only* at the end of
Phase A, as a terminal pass. If a stakeholder presentation is
unavoidable mid-engagement, produce it as a point-in-time snapshot and
label it so; don't treat it as the canonical narrative for the rest of
the build.

### 11.13 ChatGPT parametric behaviour in regulated verticals

**Symptom:** In regulated verticals (cannabis, pharma, specific
jurisdictions for legal/financial services), ChatGPT shows reasonable
visibility for the own brand but fanout data (`list_search_queries`) is
empty and the chat `sources` array is empty or near-empty. Other
engines (AI Overview, Copilot) behave normally.

**Diagnosis:** ChatGPT answers regulated-vertical queries almost
entirely from parametric memory — no retrieval at generation time. The
own brand's visibility is a function of the model's priors, which
update on training-cycle timescales (months to a year), not on content
published this quarter.

**Action:**

- Document the split in the strategy deliverable: per-engine note
  specifying ChatGPT = parametric, AI Overview / Copilot = retrieval.
- Shift content-strategy focus toward the retrieval-based engines where
  published content can move the needle in observable timeframes.
- Set long-horizon measurement expectations for ChatGPT visibility in
  these verticals. Quarter-over-quarter stability is the baseline, not
  the exception; a movement there is a meaningful signal.
- Revisit the pattern when Peec exposes training-cycle metadata or
  when the vertical's regulatory posture shifts.

**The broader pattern — high-salience parametric fallback.** Regulated
verticals are the sharpest case of this behaviour but not the only one.
Across multiple cross-vertical strategy builds spanning regulated,
editorial-ranked, and commerce verticals, the pattern appeared in
three non-regulated or weakly-regulated contexts: certification /
standards bodies on well-known international standards (own-brand
queries on widely-discussed certifications), branded queries in
consumer e-commerce on strongly-recognised brand names, and
category-shop queries in a vertical with genuine regulatory
ambiguity. In some sampled cohorts, effectively all branded ChatGPT
chats on strongly-recognised brands returned `sources: []` — a
pointer to how complete the parametric collapse can be on
high-salience brand queries. The common thread is *queries with
strong parametric priors* — well-known brands, well-known standards
or categories, commercial queries where the model can answer from
training data alone. ChatGPT (and to a varying degree Grok) will skip
retrieval on these; retrieval-first engines (Perplexity, AI Overview,
Copilot) will not.

Keep the section named around regulated verticals because that is the
most acute case and the one that needs the sharpest strategic response.
But when diagnosing ChatGPT's "empty sources + meaningful visibility"
combo on a new project, don't rule the pattern out just because the
vertical isn't regulated. Ask instead whether the *queries* carry high
parametric salience — branded queries almost always do, and commercial
categories well-represented in the pre-training corpus often do.

See §13.10 (parametric-bias detection) for the diagnostic procedure and
§13.11 for when this rises from pattern to strategic pattern. For the
per-engine architectural reasons, see the peec-ai-mcp skill §7.5.

**Sampling discipline caveat.** Before labelling a large share of a
project's chats as parametric, sample a handful of chat payloads to
confirm the response body is substantive — `sources: []` has multiple
causes (see peec-ai-mcp §7.8), and confusing engine-no-answer or empty-
placeholder responses for parametric retrieval will inflate the
apparent share of parametric chats.

### 11.14 Roster candidates surfacing from gap reports

**Symptom:** URL / domain gap reports (`gap >= 2`) consistently surface
the same external brands or domains co-appearing with tracked
competitors, but those brands / domains aren't in the tracked roster.

**Diagnosis:** The roster was built from intuition or from Peec's
auto-suggestion, which skews toward information sites. Real commercial
peers are visible in the gap data but haven't been added to `list_brands`.

**Action:** After each Analyse loop, cross-reference `mentioned_brand_ids`
in the top-N gap URLs / domains against `list_brands`. Any frequently-
appearing brand ID or `classification=CORPORATE` domain not associated
with a tracked brand is a roster candidate for the next §9.3 Brand
roster review. See §13.13 for the full procedure.

### 11.15 Sale / discount intent cluster (revenue-informed)

**Precondition:** Web analytics (GA4 or equivalent) with landing-page
revenue is available. Without it, this pattern cannot be applied — skip
it rather than guess at intent split. See §8.3.3a item 3 for the full
revenue-data handling rules (navigational exclusion, don't-shrink-on-
revenue-alone, transient/price-driven cluster caveats).

**Symptom:** Landing-page revenue analysis shows 10-25% (or more) of
commerce revenue concentrating on sale / clearance / outlet / discount
URLs. No prompts in the tracked set address that intent explicitly —
the existing Category and Product topics cover steady-state discovery
but not discount-driven buying.

**Diagnosis:** A distinct intent cluster (price-sensitive buyers) is
generating meaningful revenue, and AI assistants do surface sale /
discount-oriented answers when users ask for them. Not tracking those
queries leaves a silent blind spot in the strategy.

**Action:** Add 2-4 dedicated prompts that mirror the discount-buying
intent (e.g. "Where can I find [category] on sale?", "Best deals on
[brand] [product type] this month"). Tag them with a shared
`intent:price` tag so performance can be sliced as one cluster.
Consider grouping them under a dedicated Topic ("Deals & Sales") if
the cluster is big enough to warrant first-class reporting.

**Transience caveat:** Revenue on sale URLs can swing between seasons
or around campaigns. Before allocating topic-level capacity, cross-
check the same cluster against steady-state search-demand signals
(GSC, Ahrefs, Semrush, or equivalent). If the revenue is
campaign-transient and
search demand is low, keep the cluster small (2 prompts, no dedicated
topic). See §8.3.3a item 3 on transient / price-driven revenue
clusters and §11.12 narrative-ossification risk.

### 11.16 Sub-brand revenue defence (revenue-informed)

**Precondition:** Web analytics with landing-page revenue available
(same rule as §11.15). Additionally: the brand operates one or more
own-brand / house-brand sub-brands that have their own landing-page
subfolder or URL pattern.

**Symptom:** Landing-page revenue analysis shows an own-brand /
house-brand sub-brand subfolder (e.g. `/our-brand/*`,
`/brand-x-exclusive/*`) ranking inside the top 3 revenue pages — often
the single most lucrative subfolder after the homepage — but the
tracked prompt set treats that sub-brand as a product line rather than
a brand entity, or doesn't track defensive sub-brand discovery
queries at all.

**Diagnosis:** The sub-brand is doing commercial heavy lifting that
the strategy isn't defending. Competitor AI answers that steer users
toward alternatives at the sub-brand's price tier can erode this
revenue without showing up in any tracked prompt.

**Action:**

1. Add the sub-brand as a first-class entry in `list_brands` (own
   brand alias or separate brand entity depending on domain overlap
   — see `peec-ai-mcp` brand-domain rules).
2. Expand the brand and competitive topic allocation to include
   2-4 dedicated sub-brand prompts: direct discovery
   ("What is [sub-brand]?"), comparative
   ("[sub-brand] vs [nearest competitor]"), and use-case-anchored
   ("Best [sub-brand] for [primary job-to-be-done]").
3. Tag with an existing brand / competitive tag plus — if the
   sub-brand sits at a distinct price tier — an `intent:price` tag
   so its performance can be cross-analysed with §11.15's sale
   cluster.

**Don't shrink elsewhere to fund this.** Per §8.3.3a item 3: revenue
evidence justifies adding, not cutting. If the prompt budget is
tight, flag the trade-off for user decision rather than quietly
reallocating.

### 11.17 High SoV / low visibility divergence (narrow-but-deep presence)

**Symptom:** A brand with materially higher SoV than visibility — e.g.
9% visibility but 16% SoV, with a citation rate above 2.0 on the own
domain. Visibility is the share of chats that mention the brand at
all; SoV is the brand's share of all mention events across the cohort.
When SoV runs well ahead of visibility, the brand is mentioned many
times per chat it appears in, but appears in fewer chats overall.

**Diagnosis:** The content the model does retrieve for this brand is
dense and citable (long-form, listicle-friendly, authority-signal-
bearing) — hence the high mentions-per-chat. But the brand's
*discoverability surface* — the number of queries and source pages
the model reaches for — is too narrow for the category.

**Distinguish from other shapes:**

- *Low visibility + low SoV* → brand not known at all. Action: roster,
  awareness content, foundational listicle placements.
- *High visibility + high SoV* → market leader with matching reach and
  depth. Action: defend, don't over-invest.
- *High visibility + low SoV* → brand is named often but shallowly (1
  mention per chat). Action: improve the detail the model can surface
  (structured data, detailed product/service pages).
- *High SoV + low visibility* (this pattern) → deep content, narrow
  reach. Action: **widen reach** — more third-party mentions in
  category listicles, more Wikipedia/industry-reference presence, more
  pages that answer adjacent queries. Don't pour more depth into the
  existing hot URLs; the model is already citing them hard.

**Reporting implication:** When this pattern appears, the Phase B
narrative should resist "we're #N in SoV" as a standalone claim — pair
with visibility rank to avoid misleading a stakeholder into thinking
the brand has broad presence. See §14.2.

### 11.18 Product-knowledge vs retailer-discovery intent mismatch

**Precondition:** E-commerce Peec project where the tracked brand is a
retailer / reseller (not a manufacturer of the products).

**Symptom:** A high-allocation category topic scores 0% or near-0%
visibility despite the brand carrying the category strongly in its
assortment. Chat inspection shows the model answers the prompts with
manufacturer brand recommendations, product test results from
consumer-review publications (e.g. Which?, Consumer Reports,
Stiftung Warentest), or editorial "best X" lists rather than
retailer mentions. Example: a query like "best [product category]
this year" returns manufacturer brand comparisons, not retailer
URLs.

**Diagnosis:** The prompts are phrased as *product-knowledge* queries
("which X is best", "X vs Y", "most reliable X") rather than
*retailer-discovery* queries ("where can I buy X online", "best online
shop for X", "where to order X with fast delivery"). AI engines
activate fundamentally different answer structures for the two intent
classes in e-commerce categories: product-knowledge queries retrieve
editorial and manufacturer content; retailer-discovery queries
retrieve shop-level content and listicles of retailers. A retailer
site can rank strongly in traditional search on the product-knowledge
phrasing and still score 0% in Peec.

**Distinguish from §11.10 (structural-zero category):** Structural
zeros are categories the AI simply doesn't name *any* retailer for —
the fix is to keep 2 diagnostics and accept the ceiling. This pattern
is different: retailers *are* named in the space, just not on the
prompts this strategy wrote. The fix is on the prompt side, not the
allocation side.

**Action:**

1. Reframe the bulk of the topic's prompts toward shop-level intent:
   "Wo kaufe ich [category] online?", "Best online shop for
   [category]", "Where to order [category]", "[category] with next-day
   delivery", "[retailer] alternatives for [category]".
2. Keep 2-3 product-knowledge prompts as diagnostics — they still
   reveal whether the retailer is named in editorial comparisons.
3. Tag the shop-level cluster so performance can be sliced separately
   from the product-knowledge diagnostics.
4. Cross-reference §11.8 (fanout exposure without platform presence):
   a retailer being invisible on product-knowledge queries but
   fanning out to shop-level queries is a partial discoverability
   win.

**Reporting implication:** Phase B should frame the split as *"we
measure where people who want to buy look, not where people who want
to research look"* — keeps the prompt choice defensible.

### 11.19 Category-ranking mirror (ranking-dominated verticals)

**Precondition:** The vertical has a small number of authoritative
third-party category rankings that dominate what AI engines surface
for "who's good at X" questions — see the list in §13.2
(*Category-ranking-dominated verticals*).

**Symptom:** A topic's visibility decomposes into a per-prompt split
that mirrors the brand's third-party ranking position in each
sub-category. Illustrative example — a professional-advisory vertical
with a dominant third-party ranking body (call it "Contoso Advisory
Index"): three topics in the tracked set each show a middling average
(roughly 25%, 40%, and 45% visibility) but decompose per-prompt into
dramatic tier-gated splits. The 25%-avg topic breaks into ~70% on a
sub-area where the firm holds a Tier 1 ranking and ~0% on a sub-area
with no Tier. The 40%-avg topic splits on the same pattern: ~90% on a
Tier 1 sub-area, ~0% on an unranked sub-area. The 45%-avg topic shows
~75% on a Tier 2 sub-area and ~10% on an unranked sub-area. Each
topic average looks like a middling position but actually describes
a Tier-1-or-zero pattern per sub-area.

**Diagnosis:** In ranking-dominated verticals the AI's mental model
of "who's good at X" collapses toward the top tiers of the relevant
third-party ranking for sub-area X. Brand visibility is a near-
deterministic function of ranking position per sub-area, not a
smooth property of the practice as a whole. Topic-level metrics
mask this completely — a 40% topic average could mean "consistently
mid-tier across the practice" or "top-tier in 40% of sub-areas, absent
in the rest", which are materially different strategic positions.

**Action:**

1. Run `get_brand_report(dimensions=[prompt_id])` on every INVEST /
   borderline topic in Analyse — promoted to core in §13.2 for these
   verticals.
2. In findings, report the split alongside the average and identify
   which sub-areas are ranking-backed vs ranking-gap.
3. In Phase B, lead with the topic average and let the per-prompt
   split do the reveal (§14.2 — do not lead with the flattering niche
   stat).
4. Strategy work targets ranking-gap sub-areas as distinct initiatives
   — PR placements, submission to next cycle of the relevant ranking,
   content that signals category authority to the ranking reviewers.
   Pouring generic content into the topic as a whole will not move
   the sub-area that's invisible; the pattern is sub-area-specific.

**Reporting implication:** Phase B attribution should disclose that
a single-firm (or single-brand) Peec project in a ranking-dominated
vertical is inherently biased toward the own brand's focus sub-areas
(§14.14). The #1 position this project reports describes "most
coverage within our chosen sub-areas"; a firm measured against its
own sub-areas would show a different picture.

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
3. **Engine-scope caveat.** Fanout data surfaces only from ChatGPT
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

#### 13.17.1 Mandatory call pattern

Every Analyse loop:

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

#### 13.17.2 Critical-filter step (data vs interpretation)

For every recommendation returned by `get_actions`, record three things
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

#### 13.17.3 Legitimacy check — fallback chain

Business-model verification is part of the critical filter for any
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

#### 13.17.4 EDITORIAL/COMPARISON default — assume affiliate-driven

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

## 14. Phase B — Stakeholder deliverable

Goal: produce a stakeholder-facing presentation of the Peec strategy
and its findings. Phase B is **optional** and **terminal** — it runs
once Phase A has stabilised, at the user's request, not before.
Pre-committing to Phase B at intake risks ossifying the narrative
(§11.12).

### 14.1 Opt-in timing

- At **intake** (§8), acknowledge Phase B exists as an option at the
  end of the engagement. Don't ask for a commitment.
- At **end of Phase A** (once strategy has stabilised across one or
  more Analyse loops), offer Phase B as a discrete next step: "Strategy
  is done — do you want a stakeholder presentation?"
- If the user says yes, Phase B runs as a single pass from existing
  Phase A artefacts.

"Stabilised" is a soft term; the concrete signal is §13.6 — findings
no longer produce material action for the next Strategy iteration.

### 14.2 Anti-patterns (two-tier severity)

*Never (hard rules):*
- Tautological findings (§13.3).
- Aggregating branded and non-branded into a single metric without
  explicit "this is an anti-pattern" disclosure (§3.1).
- **Summing visibility, SoV, or any chat-share metric across
  brands.** A single chat can mention multiple brands simultaneously,
  so these metrics overlap and cannot be summed. "Own brand 35% +
  sister brand 31% = group 66%" is mathematically wrong — the
  answer is *somewhere between 35% and 66%*, and the only way to
  compute the actual group figure is to re-query the underlying
  chat set with both brand IDs filtered into one call
  (`get_brand_report` with `brand_id IN (…)`) or to union the chat
  ID sets directly. The same rule applies to mention rates,
  retrieval share, citation share, and any rate metric whose
  denominator is "chats in scope". Sentiment and position are
  per-mention aggregates and don't have this overlap problem; the
  ratio metrics (§7.37 in peec-ai-mcp) do. The failure mode is
  attractive because the sum stays below 100%, so it doesn't trip
  any obvious plausibility check — the rule has to be a hard
  "never".
- **Contrasting AI search with Google when any Google-family engine
  is on the tracked roster.** AI Mode, AI Overview, and Gemini are
  part of what's being measured. Any "AI search, not Google"
  framing (in German: "KI-Assistenten, nicht Google"; in English:
  "AI assistants, not Google") is factually wrong when two of the
  three engines on the deck ARE Google. Use "classical search",
  "traditional search", "klassische Suchmaschinen", "legacy SERP"
  instead — the contrast is AI-era search experiences vs the blue-
  links SERP, not AI vs "the company Google". Before drafting any
  explainer slide that contrasts AI search with something else,
  read the engine roster (see §14.3) and pick contrast language
  consistent with what is actually measured. The rule
  generalises: any binary framing "X, not Y" — Y cannot be a name
  that appears inside X's measurement scope.
- **Lead with the flattering niche stat when a topic has a dramatic
  per-prompt split.** When a topic's per-prompt breakdown reveals a
  large spread (e.g. 93% on a niche sub-prompt, 0% on a broad
  sub-prompt, averaging 40%), the slide's lead number must be the
  topic-level figure — 40% — and the split must be revealed in
  supporting cards or body. Leading with 93% buries the harder
  story, breaks parallelism with sibling "beneath the average"
  slides in the same deck, and invites challenge when the
  stakeholder cross-checks the topic-level number elsewhere. The
  niche figure still appears — as the reveal, not the lead.
  Exception: single-prompt topics where there is no split to reveal
  (e.g. a standalone "Insurance 76%" headline). See §14.10 on
  topic-priority claims and §11.19 on the category-ranking mirror
  pattern that produces this shape.
- **Showing the branded visibility number in any form** — headline,
  comparison slide, caption, methodology slide, sanity check. Phase
  B reports only the non-branded figure, named clearly as the
  discoverability score; the branded cohort is acknowledged as
  "measured separately, reported on sentiment and source authority
  in the branded appendix" (§3.1 Phase B rule).
- **Internal methodology vocabulary in any stakeholder-facing
  surface.** Off-limits as terms of art: "rig", "instrument",
  "dimension" (as the skill's five axes), "Phase A / Phase B",
  "loop", "Ring 1/2/3", "non-branded cohort", "branded cohort",
  "analyse phase", "write phase". Replace with plain-English
  equivalents appropriate to the audience — "the setup", "what we
  measure", "the signal streams", "iteration", "brand-name prompts",
  "category prompts", "the review", "updates to the setup". If a
  slide would only land with someone who had read the skill file, it
  is the wrong slide for a stakeholder.
- **Methodology-proving content as a primary slide.** Blind-spot
  case studies, "here's how the setup catches its own errors",
  "here's where every number comes from" provenance tables, "how
  the setup evolves" narratives — all are Phase A artefacts. They
  belong in back-pocket / Q&A, not in the body of the deck. If the
  stakeholder asks, have the answer ready. Don't spend a primary
  slide on it.
- **Structure-only or process-only slides.** "Here's how the setup
  works" is not a slide. "Here's how the setup works, and the one
  decision in it that made finding X possible" is a slide. Every
  structure / process / data-source slide must pair architecture
  with a specific audience-relevant finding. A good positive
  example: a data-source breadth slide listing every Peec surface
  used (domain report, URL report, `get_actions`, fanout mining,
  chat content analysis) paired with the one concrete finding each
  surface produced — structure-with-insight and serves as a
  "strategy backed by multiple data sources" value prop. The same
  slide listing surfaces without findings is not a stakeholder
  slide.
- **Tracking-infrastructure work on stakeholder slides.**
  Detection-regex fixes, brand-roster additions, prompt additions,
  topic reorganisations, alias repairs, and any other change to the
  Peec project configuration belong in the internal findings doc,
  not on stakeholder slides. These are legitimately interesting and
  belong in the findings record so the analyst doesn't lose track of
  what's been done — but they are noise to the stakeholder, and
  worse, they draw attention away from the brand-visibility story
  the deck is supposed to tell. The filter: *if a bullet describes a
  change to the measurement apparatus rather than a signal about
  the thing being measured, it does not belong on the stakeholder
  slide.* Before/after example: "The tracking strategy identified
  and fixed a detection gap within 48 hours — sister brand aliases
  were silently breaking" → drop from deck, keep in findings. See
  §14.3 positive rule "every bullet answers a brand-visibility
  question". Applies equally to primary slides, caption lines,
  footers, and methodology-slide supporting copy.
- Using caveats as rhetorical cover (§3.3).
- Cherry-picking date windows to inflate headlines.
- Leaking internal methodology language into stakeholder register
  (§3.5).

*Avoid unless justified:*
- Round-number windows (e.g. "last 30 days" when signal maturity
  argues for a different cut).
- False precision (visibility=14.3% when sample supports one decimal
  at best).
- Simplification-beyond-reframing (simpler framing for a non-technical
  audience is fine; dropping essential caveats to make the story
  cleaner is not).

### 14.3 Positive rules

- **Provenance preserved** (§3.2). Every stakeholder claim traces back
  to a Peec tool call in the Phase A findings.
- **Label travel intact** (§3.6). Labels used in the deck match labels
  in Peec, the Strategy sign-off, and findings.
- **Audience separation** (§3.5). Register is stakeholder-appropriate;
  internal methodology stays out unless explicitly requested.
- **Read the engine roster before drafting contrast copy.** Before
  any explainer or positioning slide that contrasts AI search with
  something else ("AI search, not X"), list the active engines on
  the Peec project (`list_projects` / the roster entry in intake
  state). Choose contrast language consistent with what is actually
  on the roster. If Google AI Mode or AI Overview is tracked, the
  contrast is with classical SERP / "klassische Suchmaschinen",
  never with "Google". If Perplexity is tracked, the contrast is
  with "traditional search", not "search engines" (Perplexity *is*
  a search engine). See §14.2 hard rule on Google-family engines.
- **Topic-average lead on split topics.** When a topic has a
  dramatic per-prompt split (§11.19 category-ranking mirror, or
  any case where best and worst prompts sit more than 40 percentage
  points apart), the slide leads with the topic-average figure and
  reveals the split below. See §14.2 anti-pattern on flattering-
  niche leads.
- **Every bullet answers a brand-visibility question.** For every
  line on every stakeholder slide (including footers and captions),
  ask: *does this tell the stakeholder something about the brand?*
  If the line answers "what did we change about the tracking
  setup?", "what does the measurement apparatus look like?", or
  "how did the tracking itself evolve?", it belongs in the findings
  doc, not on the deck. Stakeholders pay for brand-visibility
  insights, not for visibility into the tracking apparatus. This is
  the positive-rule companion to the §14.2 anti-pattern on
  tracking-infrastructure content, and it generalises beyond Peec:
  it applies to any analytics engagement where the author does both
  the instrumentation and the reporting.

### 14.4 Post-delivery feedback

Accept user edits to Phase B deliverables in place. If a requested
change would soften the methodology or misrepresent findings (e.g.
"drop the branded/non-branded split", "move the bottom line up"),
flag the risk and explain. Don't block — advise. The user is the
decider; the agent's job is to make sure they know what they're
trading off.

### 14.5 Proactive timing check-in (each loop)

§14.1 says "don't ask for Phase B commitment at intake" and "offer
Phase B at stabilisation." Between those two points, which may be
4–8 weeks apart, the skill has historically been silent on timing.
In practice, stakeholder-deck needs are often driven by commercial
triggers (plan-upgrade decisions, board meetings, client sign-off
gates, procurement cycles) that arise mid-engagement and don't
follow the skill's natural stabilisation signal. If the agent waits
for stabilisation before discussing timing, users with compressed
timelines end up having to raise it themselves.

**Rule:** At the end of each Analyse loop's findings hand-off
(§13.6), the agent surfaces Phase B timing proactively. Not as a
sales push — as a routine check that avoids the user having to
raise it. One sentence is enough:

> "Any stakeholder deliverable timeline I should be aware of?
> Stabilisation criteria suggest Loop N+2 at the earliest, but
> commercial triggers can compress that — if so, I can produce a
> baseline+methodology deck sooner with appropriate caveats."

- If the user signals **no commercial trigger**, proceed with
  normal stabilisation-driven timing.
- If the user signals **a compressed timeline**, document the
  trigger (plan gate, board meeting, committed reporting cadence,
  procurement deadline) and produce a deck calibrated to that
  trigger — with explicit caveats about data maturity and likely
  number movement.

**Two deck modes.** Phase B is not one-size-fits-all. The agent
should recognise and name which mode is being produced:

- **Compressed deck (pre-stabilisation):** baseline +
  methodology-focused. Explicit "numbers will move" disclaimer.
  Focus on the strategic framework and what's being measured, not
  on findings. Appropriate when Loop 2–3 and a commercial trigger
  forces it.
- **Stabilised deck (normal Phase B):** findings-focused,
  trajectory-over-time, roster and prompt set treated as stable
  inputs, recommendations derived from several loops of converged
  signal. Appropriate at Loop 4+ once stabilisation gates pass.

**Check-in cadence once a compressed timeline is known.** If the
user flags a compressed timeline at any loop, the agent briefly
confirms Phase B scope at the start of each subsequent session
until the deck is delivered: "Still targeting [date] for the deck?
Any scope change?" This prevents drift and keeps the two parallel
tracks (iterating strategy + preparing deck) from diverging.

**Principle:** Optional terminal phases that depend on user
triggers still need proactive agent prompts at regular intervals.
"Don't ask for commitment at intake" (§14.1) is correct — but the
mirror-image failure mode is "never ask again until stabilisation,"
which makes the skill reactive to commercial realities that don't
follow its timeline. A low-friction periodic check-in neither
pushes for Phase B nor assumes it will never happen. This pattern
generalises: any multi-loop skill with an optional terminal
deliverable should prompt on timing at each loop boundary, not
just at intake and at stabilisation.

### 14.6 Pre-build data-surface enumeration (before any slide is drafted)

Phase A findings files can have blind spots the Phase B build will
inherit unless the gap is broken explicitly. A deck drawn only from
a narrative findings file inherits whatever that findings file
missed — and the cure is a structural enumeration step before
drafting, not more care during drafting. When the user's feedback on
an early draft is "not enough substance," the correct first response
is almost always "which Peec surfaces haven't we touched?" — not "how
do I rewrite what we already have more punchily?"

**Rule:** Phase B starts with a data-surface enumeration, not a slide
outline. Before any slide is drafted, produce a short table naming
each Peec data surface and whether the engagement has touched it.
Pull the untouched surfaces **before drafting the deck**, not as
enrichment during revision.

Minimum surfaces to enumerate:

| Surface                          | Touched?  | Touched in which loop / artefact?  |
|----------------------------------|-----------|------------------------------------|
| `get_brand_report`               |           |                                    |
| `get_domain_report`              |           |                                    |
| `get_url_report`                 |           |                                    |
| `get_actions` (mandatory §13.17) |           |                                    |
| `get_url_content` on top gap URLs|           |                                    |
| `list_shopping_queries` (ecomm)  |           |                                    |
| Fanout via `list_search_queries` |           |                                    |
| Per-engine chat reading (§13.14) |           |                                    |
| Own-brand URL citation map (§13.15)|         |                                    |

Any untouched surface is pulled, analysed, and integrated into the
findings before drafting starts. The deck's data backbone is
enumerated-and-pulled, not enumerated-and-hoped-for. Record the
enumeration outcome in the findings file so future sessions inherit
the provenance.

### 14.7 Composition gate — before any slide is rendered

Density constraints get violated because the prose has already been
written by the time the agent sees the render. Density is a
composition-time concern, not a review-time concern. The check must
fire before ink hits the slide.

**Rule:** Before rendering any slide, list every text block the slide
will contain and its intended size. Then reject the slide if any of:

- **More than four text blocks.** (Title, headline, body, caption
  are four. Additional eyebrows, footers, methodology notes,
  attribution, source credits turn a scannable slide into a dense
  one — each feels minor, together they drown the signal.)
- **More than one text block below 14pt.** Source citations, page
  numbers, and legal lines are exempt from the count.
- **URL/domain/entity-as-caption** when the URL, domain, or entity
  name is the slide's main signal. For slides whose primary signal
  is a URL, domain, or entity name, that element must be the
  visual (≥24pt or equivalent prominence), not a supporting
  caption in small type.
- **Fails the 15-second read test in composed-prose form.** Apply
  the read test before the render call, not after.

**Visual PNG inspection (mandatory for multi-column layouts and
non-English labels).** Logic-based layout checks ("will this fit in
the column width I defined") systematically underestimate rendered
width for non-English strings, for compound nouns, and for labels
mixing narrow and wide letters. Visual inspection is the cheapest
reliable check.

After the first render of any multi-column slide, and of any slide
containing labels in languages the agent has not previously
built-to-column-width (German, Dutch, Finnish, Swedish, and other
languages that produce systematically longer strings than English),
convert the slide to PNG (`pdftoppm -r 200` is sufficient) and
inspect for:

1. Text overflow into adjacent columns.
2. Text bleeding past page margins.
3. Baseline collisions with horizontal dividers, separators, or
   arrows.
4. Font-size inconsistency across similar elements.

If any check fails, adjust column widths, font sizes, or line
breaks, re-render, and re-inspect. Do not rely on logic-based
layout checks alone.

### 14.8 Cover-slide composition — combine two angles when both hold

When the data produces two headline angles where each is strong
individually but each leaves a defensible objection unaddressed,
combine them on the cover with a visual divider rather than picking
one or offering the user a menu.

Worked example (illustrative): a specialty-retailer deck could pair
a ranking-gap angle ("Northwind Coffee is #2 — around a point from
#1") with an inverted-headroom angle ("~18% visibility — ~82% up for
grabs"). Ranking alone reads as "we're already winning"; headroom
alone reads as "we're losing". The combined cover tells the real
story: close-to-leader on position, lots of opportunity on volume.

**Rule:** If each of two candidate cover angles independently passes
the defensibility test (§14.9 backing) but each leaves a natural
counter-question hanging, compose a combined cover:

- Top half = ranking / competitive-position statement.
- Horizontal divider line.
- Bottom half = volume / headroom statement.
- Typography: bold heading for each half, with the more surprising
  angle accented. Both halves carry equal visual weight.

Prefer the combined cover over offering the user an A/B/C menu —
menus push composition work onto the user, and the combined cover is
strictly stronger when both halves hold up. Reserve single-angle
covers for cases where the second angle actively weakens the first
or doesn't pass scrutiny on its own.

### 14.9 Stakeholder-deck attribution — procedural, not personal

Agency language in stakeholder output carries strategic weight.
First-person attribution ("we filtered", "we declined", "we kept")
frames decisions as personal judgement and invites the easiest
stakeholder counter-move: "that was just your call." Procedural
attribution ("our tracking strategy filtered", "the methodology
kept", "the criteria flagged") makes the decision procedure visible
and shifts the conversation from the consultant's taste to the
rules of the decision.

**Rule:** For any sentence in a stakeholder deck that describes
filtering, keeping, declining, accepting, or otherwise making a
selection decision, attribute the action to the tracking strategy
or the methodology — not to "we" or the consultant personally.

- "we filtered X" → "our tracking strategy filtered X"
- "we declined" → "our tracking strategy declined"
- "we kept" → "the methodology kept" / "our strategy retained"
- "we chose" → "the criteria selected"

First-person is still fine for observational sentences ("we see
X"), interpretive sentences ("we read this as Y"), and
recommendation framing ("we propose Z"). The shift applies
specifically to selection / filtering / inclusion / exclusion
actions.

### 14.10 Topic-priority claims — external backing required

Any topic-priority claim in a stakeholder deck ("these are our four
highest-priority gaps", "these topics represent our biggest
opportunity", "these topics matter most") must be backed by a
signal the stakeholder can verify independently, without the
agent's help.

**Preferred backing sources, in priority order:**

1. **Live product category mapping** (topic → category page on the
   client's own site, with URL). For ecommerce clients this is the
   first-line methodology — see §14.11.
2. **Active SKU count** in the mapped category.
3. **Published brand position or editorial commitment** on the
   client's site or public channels.
4. **External search data** (GSC for the brand's own site; Semrush,
   Ahrefs, or equivalent for competitive volume).
5. **Industry report or analyst coverage** that names the category
   as material.

**What is not sufficient:**

- Peec's `list_prompts.volume` ordinals on their own — they
  collapse to "low" / "very low" across most commercial topics in
  regulated verticals and don't discriminate between priority and
  noise (§13.18).
- Consultant assertion alone ("we decided these matter"). The
  assertion is the conclusion that needs backing, not a backing.
- Visibility or SoV numbers from within the engagement — these are
  the claim, not the evidence behind it.

**If a candidate topic can't be backed by at least one external
source,** downgrade it to an observation, demote it off the
priority slide, or drop it from the deck. The deck's stakeholder
defence rests on backing; unbacked topics are indefensible under
pushback.

### 14.11 Topic-to-shop-category mapping (ecommerce recipe)

For ecommerce Phase B decks, the topic-priority backing in §14.10
has a specific, reusable methodology that is concrete enough to
earn its own recipe: every tracked topic in the deck's priority set
maps to a live product category on the client's site. The mapping
produces a stakeholder-facing defence that reads as "we track these
because you sell these" — a claim grounded in commercial reality
rather than consultant judgement, and verifiable by the stakeholder
in two clicks.

**Recipe (run at deck-build time):**

1. For each tracked topic in the deck's priority set, find the
   matching live product category on the client's site (category
   URL, active product count, visible brand commitment).
2. Verify each mapping by **WebFetch** or **Claude in Chrome** —
   don't rely on agent inference from the topic name alone. The
   category must exist, must be live, and must be discoverable
   from the site's main navigation.
3. Record the mapping in a two-column table in the deck: left
   column = tracked topic (stakeholder-register label), right
   column = live product category (with URL, or at minimum the
   category name visible in the site's main navigation). Add an
   SKU count column where the site exposes one.
4. Any tracked topic that fails to map to a live product category
   is a red flag — either the category was removed, the topic
   wasn't aligned with the client's actual commercial footprint,
   or the topic is too generic. Surface these for user review
   before including them in the deck.

The methodology is not decorative. It produces the deck's
defensibility: the stakeholder can verify every mapping in two
clicks, and the consultant-judgement claim "these topics matter"
is replaced by the commercial-reality claim "these topics map to
live categories on your own site."

### 14.12 Vendor-tool review — surface where the strategy improved on the tool

When a slide reviews a vendor tool's recommendations (Peec's
`get_actions`, Ahrefs opportunity scores, Semrush gap suggestions,
etc.), the implicit stakeholder question is always "what are we
paying the consultant for that the tool doesn't already do?" The
KEEP column is the natural surface to answer that question — it's
where the tool was right but the consulting layer added timing,
sequencing, reinterpretation, or prioritisation. Letting the KEEP
column read as pure agreement wastes the answer space.

**Rule:** On any slide reviewing a vendor tool's recommendations,
the KEEP column should surface at least one callout showing where
the strategy's interpretation improved on the tool's literal
recommendation.

Composition pattern:

- Coloured tint block around the highlighted KEEP item.
- "Tool said: [original recommendation]" in a warning / attention
  colour.
- "Our strategy said: [reinterpreted action]" in the deck's accent
  colour.
- Stacked below the headline KEEP line.

Worked example: Peec said "publish on comparison sites"; the
strategy said "join the affiliate network" (§13.17.4). The
reinterpretation is the consulting layer. Surface it visibly.

**If no reinterpretation exists in the KEEP set** — i.e., every
kept recommendation was accepted as-literally-written — surface
that fact too: "five recommendations kept as written, no
reinterpretation needed." This is better than letting the
stakeholder infer the unasked question. Never invent differentiation
where none exists; say so directly.

### 14.13 Closing-slide — strategy commitments, not tool dependence

A strategy deck is a defence of a direction, not an advertisement
for the vendor tool that was used to build it. Defaults produced by
the agent tend to reference the tool that surfaced the data ("next
Peec review in two weeks", "next crawl snapshot", "next rank-tracking
update") because that's the natural cadence surface — but the
stakeholder audience may not have made the tool-renewal decision
yet, and a closing that hinges on tool renewal reads as
tool-dependent rather than direction-dependent.

**Rule:** Closing commitments in a stakeholder deck must be phrased
to survive any vendor-tool renewal decision.

- Good: "Four moves. Four measurements."
- Good: "Three priorities. One quarter."
- Good: "[Named outcome] by [date]."
- Weak: "Next Peec review in two weeks."
- Weak: "Continue tracking via [vendor]."

The tool may be named in earlier slides where its data is the
subject; the closing is where the strategy, not the tool, needs to
be the subject.

**Test:** "If the client decides not to renew [the vendor tool]
tomorrow, does the closing still hold?" If no, rewrite. If yes, the
closing is tool-independent and stakeholder-defensible.

If the vendor tool's ongoing use is a firm commitment (contract
renewed, budget locked, included in a multi-year agreement), the
tool can be named in the closing — but confirm that commitment
status before defaulting to a tool-dependent close. Default to
tool-independent phrasing when the renewal status is unknown.

**Closing commitments must be outcome-shaped or backed by booked
timing.** Alongside the tool-independence rule, the closing slide
must pass a cadence-commitment test:

- **Outcome-shaped (always safe):** "Five moves. Five measurements.",
  "Three priorities, one quarter.", "[Named outcome] by [result]".
- **Timing-shaped (only when booked):** "Next review: 14 May, as
  agreed." — the date must be in writing in the engagement scope
  (contract, kickoff notes, or a confirmed follow-up booking).
- **Weak (avoid):** "Next analysis in 7 days.", "Trend data by
  early May.", "We'll review monthly." These phrasings commit the
  agent (and the client relationship) to cadences that nobody has
  explicitly agreed. The skill's internal iteration cadence (§12.7,
  §13.7) is an agent-side working assumption, not a stakeholder
  commitment, and it must not leak into deliverables as a
  time-bound promise.

**Test:** "If we don't actually run that analysis on the promised
day, will the stakeholder think we missed a commitment?" If yes,
drop the timing or replace with outcome language. Closings are the
highest-stakes slide for inferences-from-language — small phrasings
like "the next" or "in 7 days" land harder than the same words mid-
deck.

### 14.14 Single-firm focus-area bias disclosure

**Applies to:** Phase B decks for single-firm or single-brand Peec
projects — any engagement where the prompt set was built to cover
what *this* brand sells or does, and where the stakeholder audience
might interpret a "we rank #N" claim as a market-wide ranking.

**Issue:** Single-firm Peec projects produce a visibility figure
that is, by construction, biased toward the own brand's chosen
market. The prompts cover the brand's practice areas, product
categories, service lines — not an unbiased sample of what
competitors focus on. When such a deck reports "we're #1 of N
tracked competitors at X%", the unstated assumption is that the
comparison is fair across all N competitors' business lines. It
isn't: competitors measured against *their* own focus areas would
likely show different — possibly higher — numbers. Without
disclosing this, the #1 claim reads as "we're the best firm /
retailer / provider in the vertical" when it actually means "we
cover our own chosen market best among these tracked competitors."

**Rule:** Every headline visibility slide in a single-firm deck
carries a procedural focus-area-bias disclosure. Suggested
language (adapt to register):

> *"These figures reflect [brand]'s focus areas. Other [firms /
> brands / retailers] measured against their own focus areas would
> show different visibility pictures — this is a measure of
> [brand]'s coverage of its chosen market, not an absolute
> [industry] ranking."*

Place as body-text / italic note directly beneath the headline
stat. This is a procedural caveat (§14.9 procedural attribution),
not a weakening hedge. The purpose is to turn an indefensible
absolute claim ("we're #1 in our vertical") into a defensible
relative one ("we have the strongest coverage of our chosen market
among the N firms this project tracks"). Defensibility under
stakeholder cross-examination is the test.

**Does not apply to:** multi-brand portfolio projects where the
prompt set deliberately spans multiple brands' focus areas, or
portfolio-level reporting where the scope is explicitly stated as
cross-industry. The disclosure is specifically for the shape where
one brand's business defined the prompt universe.

**Pair with §11.19 category-ranking mirror** in ranking-dominated
verticals: the focus-area disclosure is additional protection
against the natural but misleading inference that the brand's
visibility is a market-wide quality signal rather than a coverage-
of-chosen-niches signal.

### 14.15 Phase B data-to-deck handoff format (for subagent delegation)

Phase B deck composition is commonly delegated to a subagent (a
dedicated PPTX or docx build agent) because the parent agent is
running Phase A in parallel or the deck build is mechanically
intensive. The skill prescribes what Phase B decks must and must not
contain (§14.2, §14.3) and where each claim must come from (§3.2
provenance) — but the parent agent still has to translate Phase A
findings into a form the subagent can render without re-deriving
anything.

This section specifies the handoff format so the translation is
consistent across projects and the subagent receives a
self-contained brief. Without a standard format, each Phase B
delegation produces a bespoke ~3,000-word prompt that re-invents
the structure; most of that re-invention is redundant, and the
inconsistency across projects makes review harder.

**Required structure of the handoff document**

For every slide in the intended deck, the handoff captures:

1. **Slide ID and purpose** — one sentence describing what this
   slide achieves in the narrative arc. (e.g. "Slide 3 — headline
   visibility result, anchors the rest of the deck.")
2. **Headline stat (if any)** — the exact number, the exact label,
   the exact unit (%, rank, count). Pre-formatted, not derived.
   The subagent does not compute — it renders.
3. **Supporting body copy** — the exact text that appears on the
   slide, in stakeholder register (§3.5). If the deck is in a
   non-English target language, write the target-language copy
   verbatim; do not ask the subagent to translate.
4. **Data sources (provenance)** — the Peec tool call(s) that
   produced each number on the slide, referenced for audit but not
   for the subagent to re-query. Format: `get_brand_report(…) →
   visibility = 37%`.
5. **Compliance checks already applied** — explicit notes
   confirming which §14.2 hard rules were checked in the
   translation: no branded visibility number, no methodology
   vocabulary, no unsummed-across-brands figures, no Google-vs-AI
   contrast if Google-family engines are tracked. The subagent
   should not re-verify these — it should trust them — but the
   parent agent should have verified them at translation time.
6. **Visuals** — for any chart or data table, the *exact* dataset
   the visual should display, pre-filtered, pre-sorted, in final
   display order. Do not hand the subagent a broader dataset and
   ask it to filter. Subagents fabricate or reorder data when
   given more than they need (cross-cutting observation).
7. **Layout directives** — any composition rules that aren't
   visible from the data (e.g. "three stat callouts in a row; all
   boxes must fit the widest label", "disclosure italic, 14pt,
   directly beneath the headline stat per §14.14").

**Structure template (YAML sketch):**

```yaml
deck_metadata:
  project: [brand / project name]
  loop_reference: Loop N findings, YYYY-MM-DD
  register: [language and tone register]
  deck_mode: [compressed | stabilised | benchmark-snapshot]

slides:
  - id: 1
    purpose: cover — title, branding, date
    headline: [exact title copy]
    body: [exact subtitle copy]
    visuals: none

  - id: 2
    purpose: [one-sentence narrative purpose]
    headline:
      value: [exact number]
      label: [exact label]
      unit: [unit]
    body: [exact body copy in target language]
    visuals:
      - type: [chart|table|none]
        data: [exact pre-filtered rows]
    provenance:
      - [tool call → number]
    compliance_checks:
      - branded_visibility_excluded: yes
      - methodology_vocab_absent: yes
      - google_vs_ai_contrast: not applicable (engines: ChatGPT, Perplexity, Claude)
      - summed_across_brands: no (all figures single-brand or pre-computed group query)

  # …one entry per slide
```

**Rule:** Phase B delegation without this handoff format is a
last-resort pattern, not a default. The parent agent bears the
cost of the translation; the subagent bears the cost of the build.
Splitting the work at the wrong seam produces bespoke briefs and
inconsistent decks.

**Delegation decision: when to delegate at all.** Short decks
(≤6 slides) and decks that reuse a template the parent agent has
already produced twice can be built in-line without delegation.
Delegate when (a) the deck is ≥ 8 slides, (b) the rendering
toolchain is different from what the parent has open (e.g. the
parent agent's environment lacks a PPTX runtime and a subagent has
it), or (c) the deck involves ≥ 3 charts where rendering fidelity
matters.

See also: in-environment constraints around PPTX rendering engines,
chart rendering parity across viewers, and cover-slide composition
rules — these live in the `pptx` / `pptx-extras` skills and are
subagent concerns, not parent-agent concerns. The parent agent's
job is the translation; the subagent's job is the render.

---

## 15. Quality gates

Gates are grouped by phase so they can be applied at the right point
in the loop. Missing any gate is grounds to pause and address before
proceeding.

### 15.1 Pre-Write gates (before §12 runs)

**Visible-in-output rule:** The agent must write these checklist results
into the Intake summary block (§8.4) verbatim, including ✓ / ✗ marks
and the rationale for any ✗. This turns the checklist from an internal
practice into a visible artefact the user can inspect and challenge.

- [ ] Intake state is populated for every critical field (brand,
      markets, owned domains, known competitors, regulatory context)
- [ ] Each applicable ring's path was attempted; skipped paths have a
      documented rationale (§3.10 — rings are paths, not an ordinal
      sequence) — first loop only
- [ ] Ring 3 was attempted on first loop (§3.8, §8.3) — "user had no
      data" is valid; "agent decided Ring 1 was enough" is not
- [ ] Ring 3 automated inventory (§8.3.1) was completed before
      composing the user ask
- [ ] Sitemap baseline (§8.3.2) was fetched and parsed before any
      user-facing ask about URL structure or crawl data
- [ ] Ring 3 data request (§8.3.3a) was sent as plain text, separate
      from the scoping widget (§8.3.3b) — not collapsed into one
      AskUserQuestion call (§3.8 shape 1)
- [ ] **External data — itemised disposition.** Every row of the
      `ring3_data_disposition` table (§7.2) is in one of three
      resolved states: `received`, `declined_by_user`,
      `not_applicable`. The fourth state, `outstanding`, is explicitly
      disallowed at this gate — no rows may be `outstanding`, and none
      may be marked "deferred to Loop 2", because that
      framing is the planned-deferral skip shape (§3.8 shape 3) and
      the gate rejects it. A blanket "Ring 3 confirmed unavailable" is
      not acceptable; each category must be individually
      dispositioned.
- [ ] Every Strategy recommendation in the output is **tagged with its
      evidence source** — Ring 1 (tool), Ring 2 step letter, Ring 3
      user-provided file, or "no direct evidence — recommendation is
      the skill default". Recommendations without evidence-source tags
      pass too easily; the tag forces the agent to confront whether
      the recommendation is data-driven or intuition-driven (§4.1,
      §4.2, §3.8).
- [ ] Known MCP gaps (§8.1 table) were checked before asking the user
      for market, plan credits, or other derivable fields
- [ ] Every Strategy recommendation (§9.1–9.7) has a Recommended,
      Reasoning, and Override block
- [ ] Prompt disposition table (existing projects only) classifies
      every existing prompt into one of six buckets
- [ ] Brand roster specifies `domains`, `aliases`, and optional `regex`
      for own brand and every tracked competitor
- [ ] Tag taxonomy has no more than 3 dimensions and 25 total tags
- [ ] Every proposed prompt has at least one tag from each taxonomy
      dimension and a `country_code`
- [ ] Branded-prompt count is within 5–10 and tagged consistently
- [ ] Model coverage section detected plan tier and routed to the
      correct branch (§9.6)
- [ ] Sister-brand relationships are recorded in the intake state and
      reflected in the brand roster configuration
- [ ] Regulatory context (if applicable) has a dedicated tag and a
      reporting-split note
- [ ] Content strategy findings section is populated with any
      non-Peec actions surfaced during intake
- [ ] Intake state is saved with `last_refreshed` set to today's date
- [ ] Strategy sign-off artefact is in place (§9.8)

### 15.2 Pre-Analyse gates (before §13 runs)

- [ ] Write verification (§12.5) completed cleanly; any reconciliation
      mismatches were replayed and closed out
- [ ] Earliest re-analysis date (§12.7) has passed — don't analyse
      before signal maturity
- [ ] Detection-pattern spot-check (§13.1) is ready to run as the
      pre-flight step; the sample-5-detected + 5-not-detected approach
      is understood, not skipped
- [ ] Cohort maturity map is available (which prompts are how old)
- [ ] Hand-off from the previous Write sub-phase (§12.8) is in hand
- [ ] `get_actions(scope=overview)` has been called for the loop's
      window, with high-opportunity slices drilled into (§13.17.1)
- [ ] Each `get_actions` recommendation has been run through the
      critical-filter step with signal / action / review-outcome
      recorded separately (§13.17.2)
- [ ] Every `EDITORIAL`/`COMPARISON` target surfaced by `get_actions`
      in a commercial vertical has been legitimacy-checked via the
      WebFetch → Claude-in-Chrome fallback chain (§13.17.3), with
      action-shape routed to PR (genuine editorial) or network-join
      (affiliate-driven) accordingly (§13.17.4)

### 15.3 Pre-Phase-B gates (before §14 runs)

- [ ] Phase A has stabilised — the most recent Analyse loop produced
      no material action for the next Strategy iteration (§13.6)
- [ ] Every finding included in Phase B traces back to a Peec tool
      call recorded in Phase A findings (§3.2 provenance)
- [ ] Labels used (tag names, topic names, strategy terms) match
      across Peec, the Strategy sign-off, and Phase A findings
      (§3.6 label travel)
- [ ] The deck's register is stakeholder-appropriate — the audience
      separation check (§3.5) has been run on the draft
- [ ] Data-surface enumeration table (§14.6) is complete — every Peec
      surface has been touched or explicitly marked not-applicable,
      with no untouched surfaces remaining before drafting begins
- [ ] Branded visibility figure does not appear in any Phase B slide
      (§3.1 Phase B rule, §14.2)
- [ ] No internal methodology vocabulary ("rig", "instrument",
      "dimension", "Phase A/B", "loop", "Ring 1/2/3", "cohort")
      appears in any stakeholder-facing slide (§14.2)
- [ ] No methodology-proving slide (blind-spot case study, provenance
      table, "how the setup evolves") is used as primary content —
      all are moved to back-pocket / Q&A (§14.2)
- [ ] Every structure / process / data-source slide pairs
      architecture with a specific audience-relevant finding (§14.2)
- [ ] Every topic-priority claim is backed by at least one external
      source from the §14.10 list (product category mapping, SKU
      count, published brand commitment, external search data, or
      industry report) — not `list_prompts.volume` alone
- [ ] For ecommerce decks: topic-to-category mapping table (§14.11)
      is populated and every mapping is verified by WebFetch or
      Claude in Chrome; unmappable topics are surfaced for review
- [ ] Any vendor-tool review slide (KEEP/CUT of `get_actions` or
      equivalent) has at least one KEEP callout showing where the
      strategy improved on the tool — or an explicit "no
      reinterpretation needed" statement (§14.12)
- [ ] Selection / filtering / inclusion-exclusion sentences use
      procedural attribution ("our tracking strategy filtered",
      "the methodology kept") rather than personal attribution
      ("we filtered", "we kept") (§14.9)
- [ ] Closing slide is tool-independent — survives a decision not to
      renew Peec (§14.13); test applied before sign-off
- [ ] Composition gate passed for every slide (§14.7): ≤4 text
      blocks, ≤1 block below 14pt exempting citations/page numbers,
      URL/domain/entity-as-primary-visual rule honoured, 15-second
      read test passes in composed prose
- [ ] Visual PNG inspection (§14.7) has been run on every
      multi-column slide and every slide with non-English labels;
      no overflow, margin bleed, baseline collision, or font-size
      inconsistency remains
- [ ] Branded and non-branded are reported separately in every
      headline slide (§3.1, §9.7)
- [ ] No tautological findings (§13.3) are used as lead lines

---

## 16. Output deliverables

### 16.1 Mandatory deliverable

**The configured Peec project itself.** Brands, topics, tags, and prompts
are live in Peec as the result of the Phase A Write sub-phase. No
intermediate operations list or execution log — the write is the
execution, and verification (§12.5) is the closure.

Each Phase A loop adds to or refines the configured project. The
"deliverable" grows with the loops; there is no single point at which
it's "done" until the user decides Phase A has stabilised.

### 16.2 Optional deliverable

**Phase B stakeholder presentation.** Produced once, at the end of the
engagement, if the user requests it (§14). Format is whatever suits the
audience — a deck, a narrative document, a one-page summary, or
something else. The *that* is optional; the *what* is covered by §14.

### 16.3 Working artefacts

Working artefacts are produced by the workflow but are not deliverables
in the client-facing sense. Their format and storage mechanism are
agent/user choice (§3.7). The skill requires they exist; it does not
require a specific file layout.

- **Intake state.** Whatever format persists the brand config, markets,
  competitors, regulatory context, and data-source inventory (§7.2 is
  one example schema).
- **Strategy sign-off artefact.** The per-loop confirmation of accepted
  recommendations — a markdown document, a structured chat message, an
  interactive list, or a handoff doc (§9.8).
- **Findings.** The per-loop output of Analyse (§13.6). Markdown file,
  chat message, updates to the intake state — whichever fits.
- **Verification log.** The Write-time reconciliation output (§12.6).
- **User-provided data.** Any CSVs, exports, or documents the user
  supplied during Ring 3. Kept so future loops don't re-ask.

In environments without persistent storage, the working artefacts live
inside handoff docs at session end and are re-seeded at session start.
The persistence mechanism is the user's choice; the persistence itself
is a requirement.

---

## 17. Contributing back

This skill improves when users feed patterns back. Two paths:

1. **If you already have a skill-improvement mechanism** (like an
   observer layer that captures patterns across skills), let it do
   its job on your local copy — and when it surfaces something that
   isn't user-specific, open a GitHub issue at
   [github.com/rebelytics/peec-ai-tracking-strategy-builder](https://github.com/rebelytics/peec-ai-tracking-strategy-builder)
   so the open-source version captures it too.

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
