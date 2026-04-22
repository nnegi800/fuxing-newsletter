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
- One key statistic at most. Save detailed numbers for the summary.
- Capture the story angle — do not front-load data.
- **Multi-link topics:** Identify the shared industry trend, transformation, or shift across all articles and lead with it as a hook. Format: `OVERARCHING THEME: SPECIFIC DETAIL.` (e.g. "LOYALTY IS DEAD IN TECH: AS MICROSOFT, AMAZON AND GOOGLE...")

**`No summary.` topics** have no summary text, so the headline must be self-contained. It can run up to 240 characters and should convey the full story on its own.

Calibrate against past Cartier DI newsletter headline examples in `reference_materials/`.

---

## Step 4 — Generate Summary

**Skip this step entirely for `No summary.` topics.**

Target length: **~415 characters**.

**Style rules:**
- No em dashes (—).
- No semicolons.
- No sentence fragments or colon-as-a-break constructions (e.g. "The goal: build X" or "A key limitation: Y"). Every sentence must be complete.
- Explain the story from scratch — assume the reader knows nothing. Start with basic context before getting into details.
- Statistics and figures belong here, not in the headline.

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
