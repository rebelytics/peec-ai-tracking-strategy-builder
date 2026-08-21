# Pattern library (§11)

Part of the **peec-ai-tracking-strategy-builder** skill (CC BY 4.0 — Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)). Section numbers are global across `SKILL.md` and `references/` — the section map in `SKILL.md` says where each § lives.

**Load trigger:** Skim the contents list during every Strategy and every Analyse step; read any pattern whose symptom matches the project. Do not skip the skim — patterns fire on recognition.

**Contents:**

- 11.1 Multi-TLD footprint underspecification
- 11.2 Sister-brand misclassification
- 11.3 Tag taxonomy dimension duplication
- 11.4 Zero-visibility = gap-to-close, not dead weight
- 11.5 Branded / non-branded reporting split
- 11.6 Plan-tier model coverage gating
- 11.7 Regulatory-aware sentiment
- 11.8 Fanout exposure without platform presence
- 11.9 Dead-weight listicle
- 11.10 Structural-zero category
- 11.11 Per-engine retrieval personality profiling
- 11.12 Narrative ossification risk
- 11.13 ChatGPT parametric behaviour in regulated verticals
- 11.14 Roster candidates surfacing from gap reports
- 11.15 Sale / discount intent cluster (revenue-informed)
- 11.16 Sub-brand revenue defence (revenue-informed)
- 11.17 High SoV / low visibility divergence (narrow-but-deep presence)
- 11.18 Product-knowledge vs retailer-discovery intent mismatch
- 11.19 Category-ranking mirror (ranking-dominated verticals)
- 11.20 Different page types earn citations via different recipes

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

**Symptom:** Landing-page revenue analysis shows 10-25 % (or more) of
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

**Confirming diagnostic — platform-mention mining (§13.10).** The
decomposition above infers ranking-dominance from outcome metrics;
fanout query text can prove it one level earlier in the causal chain.
Grep the fanout queries for the vertical's candidate ranking bodies by
name — a high named-source share (e.g. ~20%+ of all fanout queries
naming the same one or two directories, with tier vocabulary and year
qualifiers) confirms the engine literally searches the ranking bodies
before answering, and identifies exactly which bodies gate visibility.
That narrows step 4's submission/PR targets to the named bodies rather
than the long tail of directories. See §13.10 for the procedure.

**Reporting implication:** Phase B attribution should disclose that
a single-firm (or single-brand) Peec project in a ranking-dominated
vertical is inherently biased toward the own brand's focus sub-areas
(§14.14). The #1 position this project reports describes "most
coverage within our chosen sub-areas"; a firm measured against its
own sub-areas would show a different picture.

### 11.20 Different page types earn citations via different recipes

**Symptom:** A "what makes a high-citing page" pattern surfaced from
analysis on one page genre (e.g. expertise / practice-area pages)
gets applied as a generic improvement recipe across the entire site,
including page genres where the recipe doesn't fit. The most common
failure shape is to derive a recipe from a high-citing **expertise
page** and then evaluate every page through that same recipe — at
which point a high-citing **insights hub** page looks like it's
"missing" the recipe ingredients even though it's outperforming the
expertise pages on citation rate.

**Diagnosis:** AI engines treat different page genres as different
classes of evidence. The features that make an expertise page citable
are not the features that make a hub page citable, and not the
features that make a comparison-style listicle citable. Each genre
has its own citation recipe; conflating them produces wrong actions.

Three observed recipes (illustrative — not exhaustive):

- **Expertise / practice-area pages.** Specific quantitative claims
  with named sources: deal counts with attribution, named awards
  with years, named directory tiers, attributed client / partner
  quotes, practice-specific named expert counts. The mechanism is
  "the page contains hard evidence the engine can lift verbatim."
- **Insights hub pages.** Strong positioning at the top, structured
  sub-topic framing (named pillars, value-chain breakdown, taxonomy
  diagram), and large content-cluster depth (50+ regularly-updated
  publications, organised by named sub-topic). The mechanism is
  "the page reads as the canonical entry point to a topic the brand
  owns at depth."
- **Comparison / listicle / "best of" pages** (typically
  third-party). Multi-brand coverage with consistent attribute
  structure across entries (price, feature, rating, year, named
  reviewer). The mechanism is "the page reads as a structured
  reference table the engine can sample from."

**Action:**

1. **Classify the page first, then apply the recipe.** Before
   attributing a page's high or low citation rate to any specific
   content element, classify the page genre. Use the recipe for
   that genre as the comparison baseline, not the recipe from a
   different genre.
2. **Don't recommend recipe ingredients from one genre on a page of
   a different genre.** "Add specific deal counts to the AI Insights
   hub" is the wrong recommendation; the hub doesn't fail by lacking
   deal counts, and adding them won't move citation rate. The right
   recommendation is "deepen the content cluster" or "tighten the
   pillar framing", per the hub recipe.
3. **In Phase B side-by-side comparisons across page genres, frame
   the contrast as different recipes, not different scores.** A
   slide that compares an expertise page (1.04× citation rate) and a
   hub page (1.80× citation rate) should explain *why each is
   citable* (different mechanism) rather than treating one as the
   baseline and the other as the gap.

**Anti-pattern.** Surfacing a content-element pattern from one
high-citing expertise page (e.g. "this practice-area page has named
directory tiers and attributed quotes; that's the recipe") and then
listing the same elements as "missing" on a high-citing hub page on
a comparison slide. The hub page isn't missing them — it's earning
citations through a different mechanism. The comparison slide reads
as a critique of a page that is in fact outperforming the page being
held up as the recipe source.

**Reporting implication:** When a deck includes a side-by-side
comparison of two pages with different citation rates, classify both
pages by genre before attributing the rate difference to specific
content elements. If the genres differ, the rate difference may not
be a content-recipe gap at all — it may be a different recipe
working at a different intensity, which is a different strategic
implication.

---
