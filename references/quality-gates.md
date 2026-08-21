# Quality gates (§15)

Part of the **peec-ai-tracking-strategy-builder** skill (CC BY 4.0 — Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)). Section numbers are global across `SKILL.md` and `references/` — the section map in `SKILL.md` says where each § lives.

**Load trigger:** Read at every phase transition. The relevant gate must pass before §12 Write, §13 Analyse, or §14 Phase B begins — a failed gate item blocks the transition.

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
- [ ] No visibility, SoV, retrieval-share, citation-share, or any
      chat-share metric is computed by adding per-brand percentages.
      Any "group" / "family" / "combined" figure was produced by a
      single Peec query with `brand_id IN (…)` (or by manual chat-ID
      set union) — and is labelled as such with the composition
      method named in the caption. No "combined" / "group" bar
      drawn by arithmetic addition appears on any chart (§14.2,
      §14.3 group-as-rows rule)
- [ ] Every named page or content piece on every slide has its URL
      as a clickable hyperlink on the same slide (§14.3 every-named-
      piece-clickable-URL rule). Hyperlink markers use ASCII arrows
      (`→ `), not the unicode link emoji
- [ ] No time-series chart appears in a window during which the
      tracked prompt set changed (`create_prompt`, prompt-text
      `update_prompt`, or `delete_prompt` operations). For any
      surviving time-series chart, the caption names the prompt-set
      stability window (§14.2 trend-with-changing-cohort, §14.3
      time-series caption rule, §13.7 stable-cohort gate)
- [ ] For every named gap URL on any slide, the URL's host has been
      classified against the tracked brand roster. Competitor
      homepages, category pages, and product pages have been
      excluded from editorial-gap slides. Competitor-owned
      multi-brand listicles that appear are explicitly flagged as
      competitor-owned in the slide content (§13.9, §14.3 gap-list
      classification rule)
- [ ] Every named factual claim on every slide (especially
      comparison claims of the form "X includes Y, Z doesn't") has
      been verified in this session or in the most recent loop's
      findings. Claims older than the most recent loop are flagged
      for re-verification before inclusion or removed from the slide
      (§13.6 carry-forward claim re-verification)
- [ ] The cover slide carries a single message; multi-stat hero
      compositions sit on body slides only. The §14.8 combined-
      headline pattern is not used on the literal cover slide
      (§14.3 cover-single-message rule)
- [ ] **Editorial pitch targets are browser-verified.** Every
      editorial pitch target named on any slide has been opened in
      a browser and verified to satisfy all of: (a) the own brand
      is genuinely absent from the page (cases (a)/(b)/(c) of the
      §13.9 brand-detection verification have been disambiguated);
      (b) the page is open-access — paywalled sources are dropped
      or framed as visible-snippet targets only; (c) the page URL
      is the right one for the brand's commercial positioning
      (e.g. mid-market vs upper mid-market — the listicle that
      ranks the wrong tier is not a pitch target); (d) the page's
      editorial quality justifies a pitch (vs an AI-generated SEO
      farm). Tool-surfaced action candidates from URL gap reports
      are signals, not actions; promotion to action requires
      browser verification.
- [ ] **No internal artifact references on stakeholder slides.**
      Stakeholder methodology slides, footers, and captions must
      not carry: Peec project IDs (`or_…`), internal findings file
      paths (`findings-YYYY-MM-DD.md`), prompt IDs (`pr_…`), brand
      IDs (`kw_…`), tag IDs (`tg_…`), session IDs, workspace paths,
      or any other internal artifact identifier. Provenance for
      audit purposes belongs in the working findings artefact, not
      on the deck. Methodology slides describe the data and method
      in stakeholder-register language ("Source: daily tracking of
      N questions across M engines, P-day window"), not in
      internal-tooling language. The §3.2 provenance discipline
      governs the findings file; the stakeholder methodology slide
      describes the methodology, not the audit trail (§14.3
      audience-separation rule).
- [ ] **Terminology consistency check across the full deck.**
      Identify the 4–6 key concept terms the deck depends on
      (engines / AI tools / AI assistants; branded / non-branded /
      "by name" / category-level; expertise pages / sub-expertise
      / practice-area pages; specific deal numbers / deal volume /
      deal counts; etc.) and grep the build script for each
      variant. Pick one preferred term per concept and replace
      every alternate. The decision on which term to use is less
      important than using one consistently — terminology drift
      across slides reads as inattention to the stakeholder
      audience even when each individual choice is defensible
      (§14.3 register-consistency rule). Ship a small terminology-
      check helper alongside the build script when feasible:
      a `{key concept: preferred term, forbidden alternates}`
      map with grep-and-report output, run as the last
      pre-render check.

---
