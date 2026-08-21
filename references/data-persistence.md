# Data persistence (§7)

Part of the **peec-ai-tracking-strategy-builder** skill (CC BY 4.0 — Eoghan Henn / [rebelytics.com](https://www.rebelytics.com)). Section numbers are global across `SKILL.md` and `references/` — the section map in `SKILL.md` says where each § lives.

**Load trigger:** Read when initialising or resuming the project workspace, and before writing any loop artefact.

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
