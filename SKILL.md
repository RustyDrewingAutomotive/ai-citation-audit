---
name: ai-citation-audit
description: Run a recurring AI-search citation audit — query ChatGPT, Claude, Gemini, Grok, and Perplexity live in the browser with real customer-style buying questions, score whether the user's business is cited (YES/PARTIAL/NO), log results to a spreadsheet, and produce a weekly summary report with trends and action items. Use when the user asks to run a citation audit, AI visibility audit, LLM SEO check, or asks how often AI recommends their business.
---

# AI Citation Audit

You are running an organic-citation audit: measuring whether AI engines recommend the user's business when asked realistic customer questions. Treat it like a rank tracker for AI search.

## Step 0 — Load config

Read `config.yaml` in this skill's folder (fall back to `config.example.yaml` and tell the user to create their own). It defines:

- `business`: canonical name, aliases, and every owned location/store (name, city, category)
- `locations`: the geographic framings to test (e.g. city A, city B, region)
- `categories`: product/service categories to test (plus one "group" query about the business type overall)
- `competitors`: known competitors to track mentions of
- `engines`: which AI engines to run (chatgpt, claude, gemini, grok, perplexity)
- `output`: where to save results (local folder and/or cloud-drive folder name)

## Step 1 — Verify ground truth (do not skip)

Before scoring anything, verify via web search/fetch which locations and categories the business ACTUALLY operates, and where competitors actually are. Update your scoring notes accordingly.

> Lesson learned: an early run flagged a correct AI answer as a hallucination because the audit's own assumptions about which city a store was in were wrong. An AI answer that contradicts the config may be right — check before scoring.

Scoring rule: any store/location owned by the business counts as a citation, even if branded differently (e.g. "BMW of Springfield" owned by "Smith Auto Group").

## Step 2 — Build the query matrix

For each location × (group + each category):

- Group query: "What's the best [business type] in [location]? Name specific [businesses] if you can."
- Category query: "Best [category] in [location] — name specific [businesses] if you can."

Number them Q1..Qn. Keep wording organic — never mention the user's business in the prompt. Do not bias the engine.

## Step 3 — Run queries live

Use the Claude in Chrome extension. For each engine:

1. Open the engine in a fresh conversation (new chat per query, or per batch if the engine supports the numbered-list format in `prompts/paste_in_audit_prompt.md`).
2. Submit the query, wait for the full response to render (scroll to confirm completion).
3. Capture the full response text and save it under `Raw_Responses/[engine]/[date]_Q[n].md`.

Practical handling, learned from real runs:
- **Sign-in walls** (Grok, sometimes others): never enter credentials. Ask the user to sign in once in Chrome, then continue. If they can't, skip the engine and note it in the report.
- **Rate limits / "high demand"**: wait 60s and retry; after 3 failures, move on and note it.
- **Cookie/consent/age overlays**: decline non-essential cookies; never answer personal questions (e.g. birth year) on the user's behalf — ask them.
- **Blocked domains**: if the browser extension can't automate an engine, give the user the paste-in prompt (`prompts/paste_in_audit_prompt.md`, filled with the query list) and score whatever they paste back.

## Step 4 — Score each response

- **YES** — the business (any owned store/alias) is the #1 or primary recommendation
- **PARTIAL** — mentioned, but behind a competitor
- **NO** — not mentioned

Also record: every competitor named, rank position of the business, notable quotes, factual errors/hallucinations (with evidence from Step 1), and whether the parent/group brand name was mentioned alongside a store brand.

## Step 5 — Log and report

1. **Citation log spreadsheet** (`Citation_Log_[date].xlsx`) — one row per query per engine; schema in `templates/citation_log_schema.md`. Include a Summary sheet with per-engine totals, Citation Rate (YES %) and Strong Rate (YES+PARTIAL %).
2. **Weekly summary** (`Weekly_Summary_[date].md`) — follow `templates/weekly_summary_template.md`: executive summary, results table by engine, results by category × location, key findings, hallucinations/corrections, competitor mention counts, prioritized action items, trend vs. prior runs (read previous logs from the output folder if present).
3. Save both plus raw responses to the configured output location, and present the files to the user.

## Step 6 — Verify before finishing

Recompute all percentages programmatically (don't hand-add), confirm row counts match the query matrix, and confirm every NO/PARTIAL has the competitor that beat the business recorded.

## Suggested cadence

Offer to schedule this weekly (same weekday) if the user hasn't already. Trends matter more than single runs.
