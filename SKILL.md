---
name: peec-ai-tracking-strategy-builder
description: Build or refine a Peec AI tracking strategy as an iterative workflow. Phase A loops Intake → Strategy → Write → Analyse, each producing concrete Peec configuration changes and findings that feed the next iteration. Phase B (optional, terminal) produces a stakeholder presentation once the strategy has stabilised. Works for brand-new projects and for existing projects that need rationalising. Load whenever the user mentions Peec AI strategy, Peec setup, Peec onboarding, "what should we track in Peec", "build/improve our Peec project", Peec prompt rationalisation, Peec rebuild, or AI visibility strategy for a brand using Peec. Also triggers on Phase B / retrospective requests: stakeholder presentation, deck, or brief that explains an existing Peec strategy; "explain our Peec setup", "document our tracking strategy", "analyse our current Peec project", "what are we tracking in Peec and why". Companion to `peec-ai-mcp` (recommended; tool mechanics).
version: 2.0.0
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

**The no-filter `get_brand_report` blend silently inflates the own
brand against competitors.** A `get_brand_report` call with no tag /
topic filter returns visibility computed across **all** prompts in
scope — branded and non-branded combined. The blended figure is
mathematically valid for any single brand individually, but as a
**competitive comparison** it is structurally inflated for the own
brand:

- On branded prompts, the own brand scores ~95% by construction (the
  brand name is in the prompt, so the response nearly always names
  it).
- On non-branded prompts, the own brand scores at its real
  discoverability level (e.g. ~50%).
- The blend is dominated by whichever cohort is larger.

But every tracked competitor is measured on the **same** branded
prompts about the own brand, where competitors score ~0% (the prompt
isn't about them, so they aren't named). This means the blended
figure puts the own brand at a near-95% on the branded slice while
holding every competitor at near-0% on the same slice — the blended
roster comparison is structurally asymmetric and the own brand looks
stronger than the competitive picture warrants.

**Hard rule — competitive comparisons.** Any roster comparison
(Phase B or Phase A) filters to non-branded only. Never report a
no-filter `get_brand_report` blend as a competitive headline.

**Worked example (composition shape, not just numbers).** Initial
deck reported the own brand at "#1 in category visibility, 53%" —
this was the no-filter blend across roughly 70 prompts (a small
branded slice plus a much larger non-branded slice). Recomputed on
non-branded only, the own brand's figure dropped into the high
40s, essentially tied with the next-ranked rival in the low 50s. A
different headline. The 53% was the own brand's branded ~95%
pulling its blend up, while every competitor's blend was held flat
by their ~0% on the same branded prompts. Competitors had no
equivalent inflation pathway.

This is the same family of error as the §14.2 "summing rates across
brands" hard rule — applied to the **time / instrument axis** instead
of the brand axis. Both rules generalise: aggregating across two
distinct measurement instruments doesn't just produce uninterpretable
numbers; when one instrument has structural asymmetries between the
own brand and competitors (as branded prompts do), the aggregate
silently inflates the own brand.

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


## 6a. Section map — where to read what

This skill uses progressive disclosure: this file holds the mental model
and the rules that apply on every invocation; the phase playbooks live in
`references/` and are loaded when their phase runs. Section numbers are
global — a cross-reference like "§13.6" resolves via this map. Every load
trigger below is mandatory, not optional: running a phase without reading
its file first is the same failure shape as skipping an intake ring
(§3.8).

| File | Sections | Load trigger |
|---|---|---|
| `references/data-persistence.md` | §7 | When initialising or resuming the project workspace, and before writing any loop artefact |
| `references/phase-a-intake.md` | §3.8–3.10, §8 | In full, at the start of every Intake step, before asking the user for anything |
| `references/phase-a-strategy.md` | §9–§10 | Before drafting or revising any strategy recommendation, and before sign-off |
| `references/pattern-library.md` | §11 | Skim its contents list during every Strategy and every Analyse step; read any pattern whose symptom matches |
| `references/phase-a-write.md` | §12 | Before executing any write wave against the Peec project |
| `references/phase-a-analyse.md` | §13 | At the start of every Analyse step, before pulling any report |
| `references/phase-b-deliverable.md` | §14 | When Phase B is triggered or offered — plus the §14.5 timing check-in at every loop close |
| `references/quality-gates.md` | §15 | At every phase transition; the relevant gate must pass before Write, Analyse, or Phase B begins |

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
