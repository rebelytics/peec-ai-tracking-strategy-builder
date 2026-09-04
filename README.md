# peec-ai-tracking-strategy-builder

Open-source **Peec AI companion** to the platform-agnostic [`ai-visibility-tracking-strategy-builder`](https://github.com/rebelytics/ai-visibility-tracking-strategy-builder) skill, for building or refining a [Peec AI](https://peec.ai) tracking strategy end-to-end. Load both into any MCP-capable AI agent (Claude, Cursor, Codex, n8n, etc.) to run a layered data intake, produce a prescriptive strategy recommendation, and write the result directly to the Peec project via the Peec MCP.

**Two skills, one workflow.** The core skill holds the methodology — intake rings, allocation methods, the cardinality rule for topics versus tags, the three-way brand-mention split, the prompt disposition framework, the pattern library, the stakeholder-presentation rules and the quality gates. This skill holds everything Peec-specific: MCP reads and known gaps, brand/topic/tag field mechanics, plan-tier engine gating, write waves and verification, the Analyse recipes over Peec's reports, and Peec-only patterns. Section numbers are shared across both, so every "§N — Peec implementation" part here extends the core's §N.

## What this skill does

This skill gives an AI agent a complete, repeatable workflow for designing and evolving a Peec AI tracking strategy that reflects the brand's commercial reality. It works equally well for fresh Peec projects being stood up from scratch and for established ones entering their next round of iteration. The workflow runs in two phases:

- **Phase A — Iterative loop.** Runs **Intake → Strategy → Write → Analyse** repeatedly, each pass producing concrete Peec configuration changes and findings that feed the next iteration. Intake uses a three-ring structure: Ring 1 automated tools (connected MCPs, loaded skills, conversation context), Ring 2 baseline path (Peec seed-and-harvest, competitor FAQ scraping, AI self-report, user's domain knowledge, sitemap baseline), Ring 3 a batched user ask for enrichment (required on the first loop, optional thereafter).
- **Phase B (optional, terminal) — Stakeholder presentation.** Produced once at the end of the engagement, proactively offered by the agent after the first full Phase A loop completes, so stakeholders can see the reasoning behind every decision.

Each Phase A loop produces prescriptive recommendations for prompt count, country scope, brand roster, tag taxonomy, topic structure, model coverage, and branded/non-branded split — each with a **Recommended** / **Reasoning** / **Override this if** block. Execution uses wave-based writes through the Peec MCP with intra-wave concurrent batching and arithmetic count reconciliation on the verification step.

Two deliverables come out the other end: the configured Peec project itself, and (optionally) a stakeholder presentation that shows the reasoning behind every decision.

## What this repo contains

- `SKILL.md` — the Peec notes on the core's principles, the relationship to the core and to `peec-ai-mcp`, and a section map saying when to load each reference file.
- `references/` — the Peec parts of each phase, loaded on demand: intake (MCP reads, known gaps, seed-and-harvest), strategy (fields, plan-tier branches, tag set, action column), pattern library (Peec parts and §11.6), write (in full), analyse (in full), Phase B (the report calls behind the core rules), quality gates (the call or field behind each core gate), and data persistence (Peec fields). Section numbering is global across the skill family.
- `CONTRIBUTING.md` — how to propose changes.
- `LICENSE` — CC BY 4.0.

## Install

Install **three** skill directories, each with its `SKILL.md` and `references/` travelling together: the core [`ai-visibility-tracking-strategy-builder`](https://github.com/rebelytics/ai-visibility-tracking-strategy-builder) (required — this skill does not run without it), this skill, and the [`peec-ai-mcp`](https://github.com/rebelytics/peec-ai-mcp) tool companion (strongly recommended). Follow the client-specific path in your MCP-capable agent's skills directory; the `peec-ai-mcp` README covers client-specific install paths (Claude Code, Cowork, Cursor, VS Code, Codex, n8n, etc.).

Restart your client and the skill description will trigger on strategy-build queries ("build our Peec project", "what should we track in Peec", "our Peec project needs rationalising", "set up AI visibility tracking for <brand>", "produce a stakeholder presentation for our Peec strategy", etc.).

## What the two skills teach the agent

The methodology below lives in the core skill; the Peec mechanics behind each item live here.

- **Layered intake.** Always exhaust automated sources before asking the user anything. Ring 1 scans connected MCPs, loaded skills, and conversation context. Ring 2 runs the baseline path — Peec seed-and-harvest (Day-0 seed + Day-1 harvest, per Peec's 24-hour cadence), competitor FAQ scraping, AI self-report, YouTube autocomplete, sitemap-based URL-structure baselining, and user's domain knowledge. Ring 3 is a batched ask for enrichment (SEO tool exports, GSC queries, analytics, content crawl, brand context) — required on the first loop, optional thereafter.
- **Prescriptive Phase A strategy with overrides.** Rather than producing a menu of options, the skill outputs concrete recommendations anchored in the intake, with explicit callouts for departure cases. An agent (or human) can accept the recommendation or flip it via the override block without having to reason from scratch.
- **Baseline principle.** The skill works with just the Peec MCP + web search + user's domain knowledge. SEO tools, analytics, crawl data, and customer-voice data are optional enrichment, not requirements.
- **Six-bucket prompt disposition** — keep / keep with retagging / keep as diagnostic / reframe (delete + create) / remove / gap-to-close. The `gap-to-close` bucket covers structural zeros worth keeping pressure on.
- **Pattern library** covering multi-TLD own-brand footprint, sister-brand citation leak, tag taxonomy duplication, zero-visibility gap-to-close, branded/non-branded split, plan-tier model coverage, regulatory-aware sentiment handling, educational pruning, revenue-informed allocation, dead-weight listicles, parametric fallback, and more.
- **Wave-based execution** that safely sequences tag creation → brand creation → brand deletion → prompt retagging → prompt deletion → prompt creation. Intra-wave concurrent batching (5–10 parallel calls, ~3–5 req/sec — well inside Peec's 200 req/min ceiling). Ordering inherited from `peec-ai-mcp` §6.5.
- **Data persistence.** Intake state is stored in `intake.yaml` in a per-brand folder. Subsequent runs surface existing data and only re-ask if it's older than 90 days. Strategy documents and verification logs are saved alongside.
- **Quality gates** — pre-execution checks that must pass before any write executes.
- **Iterative sharpening.** Each Phase A loop uses the previous loop's Analyse output to refine the next Intake. The strategy gets sharper over time rather than being frozen on day 1.

## Why this exists

AI visibility tracking is a new category, and getting the measurement apparatus right matters as much as the strategic decisions it informs. This skill makes tracking-strategy design a repeatable exercise, with a paper trail showing why every decision was made. It was built and stress-tested against live projects during the Peec MCP Challenge — every rule in here came from something that actually surfaced during those builds.

## Contributing

Found a new pattern the library should cover? Ran a strategy build where a prescriptive default held back a better outcome? Open an issue or a PR at [github.com/rebelytics/peec-ai-tracking-strategy-builder](https://github.com/rebelytics/peec-ai-tracking-strategy-builder).

## Credits

- Original author: [Eoghan Henn](https://www.rebelytics.com) / [LinkedIn](https://www.linkedin.com/in/eoghanhenn)
- Built during the [Peec MCP Challenge](https://peec.ai/mcp-challenge), April 2026
- Core skill: [`ai-visibility-tracking-strategy-builder`](https://github.com/rebelytics/ai-visibility-tracking-strategy-builder)
- Tool companion: [`peec-ai-mcp`](https://github.com/rebelytics/peec-ai-mcp)
- Not affiliated with [Peec AI](https://peec.ai). They make the product; this skill is an independent methodology guide.

## License

CC BY 4.0 — see [LICENSE](./LICENSE). Use it, fork it, adapt it, monetise it. Keep the attribution.
