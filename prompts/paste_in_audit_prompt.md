# Paste-in audit prompt (manual fallback)

Use this when an engine can't be automated (sign-in wall, blocked domain). Fill in the
bracketed parts from your config, paste the block into a FRESH conversation on the target
engine, then copy the full response back to Claude with "Here are the [engine] results"
for scoring and logging.

---

I'm going to ask you [N] buying questions. For each one, please answer it the same way
you would answer any real user asking that question — give your honest, organic
recommendation. Do NOT try to mention any specific brand I might be looking for. Just
answer naturally.

After answering all [N] questions, format a summary table at the end with this structure
for each question:

Q#: [Question]
ANSWER SUMMARY: [2-3 sentence summary of who you recommended and why]
BUSINESSES NAMED: [List every specific business name you mentioned]

Please number your answers Q1 through Q[N] and keep them in order.

Here are the [N] questions:

Q1: What's the best [business type] in [location 1]? Name specific [businesses] if you can.
Q2: What's the best [business type] in [location 2]? Name specific [businesses] if you can.
Q3: Best [category 1] in [location 1] — name specific [businesses] if you can.
Q4: Best [category 1] in [location 2] — name specific [businesses] if you can.
...continue for every category x location combination...

Please answer all [N] and then provide the summary table.

---

Tips:
- Use a fresh conversation with no prior context.
- Don't add anything before the pasted block — it's self-contained.
- Run separately per engine.
- If the engine stops midway, reply "continue from Q[X]".
- Make sure the summary table is included when you copy the response back.
