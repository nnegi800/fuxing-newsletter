# Phase 1 — Newsletter Generation

Execute steps in order for each topic. Do not skip or reorder.

---

## Step 1 — Parse Input File

Read the uploaded file and extract for each topic:
- All links
- Narrative angle (if present: `Narrative: [text]`)
- Whether it is marked `No summary.`

Topics with neither flag are treated as standard (headline + summary).

---

## Step 2 — Fetch & Read

For each topic, fetch and read all provided URLs to extract key information.

- If a URL is not scrapable (403, paywall, redirect failure), extract keywords from the URL itself and use web search on those keywords to find equivalent coverage of the same story.
- If a topic has multiple links, treat them as a single story — synthesize the key facts, figures, and angle across all links into one unified set of information for headline and summary generation.

---

## Step 3 — Generate Headline

Target length: **~160 characters**. For `No summary.` topics: **up to 240 characters**.

**Style rules:**
- ALL CAPS with a dot at the end.
- No em dashes (—). Use a colon or comma instead if a pause is needed.
- One key statistic at most per headline. Save the rest for the summary. A headline with no statistic is fine; a headline with two or more is not.
- Capture the story angle — do not front-load data.
- **Multi-link topics:** Identify the shared industry trend, transformation, or shift across all articles and lead with it as a hook. Format: `OVERARCHING THEME: SPECIFIC DETAIL.` (e.g. "LOYALTY IS DEAD IN TECH: AS MICROSOFT, AMAZON AND GOOGLE...")

**`No summary.` topics** have no summary text, so the headline must be self-contained. It can run up to 240 characters and should convey the full story on its own.

Calibrate against past Cartier DI newsletter headline examples in `reference_materials/`.

---

## Step 4 — Generate Summary

**Skip this step entirely for `No summary.` topics.**

Target length: **~415 characters**. Never exceed **600 characters** — if the content demands it, cut the least important detail rather than expanding further.

**Style rules:**
- No em dashes (—).
- No semicolons.
- No sentence fragments or colon-as-a-break constructions (e.g. "The goal: build X" or "A key limitation: Y"). Every sentence must be complete.
- Explain the story from scratch — assume the reader knows nothing. Open with a sentence that gives the reader just enough context to understand what's happening, then move into the specific event or detail. Do not lead with jargon, proper nouns, or insider references without first grounding them. For example: if the summary references a named test or concept, briefly define it in one clause before describing what happened with it.
- Statistics and figures belong here, not in the headline. Use specific numbers (e.g. "2.7%") rather than vague qualifiers ("nearly closed").
- Connect facts to meaning using transition phrases ("this underscores", "the result", "the broader implication"). Do not leave the reader to infer why a fact matters — state it.
- Do not over-explain technical concepts. One brief defining clause is enough (e.g. "a process that does X"). Move on.
- Do not contextualize what the reader already knows from the headline. The summary extends the story — it does not repeat it.
- End on the implication or consequence, not on a neutral restatement of the event.

Calibrate against past Cartier DI newsletter summary examples in `reference_materials/`.

---

## Step 5 — Categorize

Read the reference `.md` files in `reference_materials/` and assign each topic a category based on how past stories have been grouped. Do not use a fixed list — derive the category from the patterns you find in the reference data.

`No summary.` topics are typically niche, one-of-a-kind stories and will usually warrant their own specific category. Let the content and reference data guide the classification.

---

## Step 6 — Compile & Output

**Standard topic format:**
```md
## [Headline]

[Summary]

**Read more:** [link1](url) · [link2](url)

*Category: [category]*
```

**`No summary.` topic format:**
```md
## [Headline]

**Read more:** [link1](url)

*Category: [category]*
```

Save the output as `headline_summary_output.md` inside that month's folder. Separate each entry with `---`.
