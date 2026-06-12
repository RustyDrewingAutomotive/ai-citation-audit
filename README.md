# AI Citation Audit

A Claude skill that measures how often AI assistants (ChatGPT, Claude, Gemini, Grok, Perplexity) recommend **your business** when real customers ask buying questions — the AI-search equivalent of a rank tracker.

Built for local/regional businesses competing on "best X in [city]" style queries. Originally developed for a multi-location auto dealer group; fully config-driven so it works for any business with locations, service categories, and named competitors.

## What it does

1. Builds a query matrix from your config: `(group query + one query per category) x each location` — e.g. 7 categories x 3 locations = 21 queries per engine.
2. Runs each query **live** in your browser against each AI engine, in a fresh conversation, phrased like a real customer ("What's the best [category] in [location]? Name specific businesses if you can.").
3. Scores every response:
   - **YES** — your business is the #1 / primary recommendation
   - **PARTIAL** — mentioned, but behind a competitor
   - **NO** — not mentioned at all
4. Verifies ground truth before scoring (which locations/brands you actually own) so hallucinations are flagged correctly — in both directions.
5. Saves raw responses, a citation log spreadsheet, and a weekly summary report with trends vs. prior runs and prioritized action items.

## Requirements

- [Claude desktop app](https://claude.ai/download) with Cowork mode, or Claude Code
- [Claude in Chrome extension](https://www.anthropic.com/chrome) (used to drive the AI engines in your browser)
- Signed-in accounts for engines that require login (Grok needs one; ChatGPT/Gemini work better signed in)

## Setup

1. Copy this folder into your Claude skills directory (or install via Settings > Capabilities > Skills).
2. Copy `config.example.yaml` to `config.yaml` and fill in your business, locations, categories, and competitors.
3. Tell Claude: **"Run the AI citation audit."**

## Repo contents

| File | Purpose |
|------|---------|
| `SKILL.md` | The skill — full instructions Claude follows to run the audit |
| `config.example.yaml` | Template config: business, locations, categories, competitors, engines |
| `prompts/paste_in_audit_prompt.md` | No-automation fallback: a self-contained prompt you paste into any AI engine manually, then paste the answer back to Claude for scoring |
| `templates/weekly_summary_template.md` | Structure of the weekly report |
| `templates/citation_log_schema.md` | Column definitions for the citation log spreadsheet |

## Tips from real-world use

- **Verify ground truth every run.** Our first audit flagged a correct AI answer as a hallucination because the rubric itself had a store's city wrong. The skill now web-verifies ownership/locations before scoring.
- **Expect sign-in walls and rate limits.** Grok requires login; engines rate-limit bursts. The skill waits and retries, and falls back to the paste-in prompt when a domain can't be automated.
- **Structural losses are normal.** AI engines favor the in-town option. A PARTIAL behind a closer competitor is geography, not reputation — the report separates these from true gaps.
- **Run weekly, same day.** Trends across runs matter more than any single week's numbers.

## License

MIT — use it, fork it, share it.
