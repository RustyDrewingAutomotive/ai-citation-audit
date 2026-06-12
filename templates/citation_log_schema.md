# Citation_Log spreadsheet schema

## Sheet 1: "Log" — one row per query per engine
| Column | Description |
|--------|-------------|
| Date | Run date (YYYY-MM-DD) |
| Engine | chatgpt / claude / gemini / grok / perplexity |
| Q# | Query number in the matrix |
| Category | Group, or the product/service category |
| Location | Geographic framing used |
| Query Text | Exact query submitted |
| Score | YES / PARTIAL / NO |
| Rank | Position of the business in the answer (1, 2, ... or —) |
| Business Named As | Exact name the engine used (store brand vs. group brand) |
| Group Brand Mentioned | TRUE/FALSE — was the parent/group name stated? |
| Competitors Named | Comma-separated list |
| Beaten By | Competitor ranked above (for PARTIAL/NO) |
| Structural? | TRUE if loss is geography/franchise-driven, not reputation |
| Errors/Hallucinations | Factual errors in the answer (verified) |
| Notes | Notable quotes, awards cited, etc. |
| Raw File | Path to saved raw response |

## Sheet 2: "Summary" — per engine
Engine, Total Queries, YES, PARTIAL, NO, Citation Rate (YES %), Strong Rate (YES+PARTIAL %), Top Competitor Mentioned

All percentages must be computed from the Log sheet (formulas or recomputed
programmatically) — never hand-entered.
