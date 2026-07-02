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

**If Phase 0 was run in this session (Stream A or Stream B):** use the facts, figures, and angles already gathered during Phase 0 research. Do not re-fetch or re-read the URLs — the Phase 0 output is the authoritative source for this step.

**If Phase 0 was not run (user supplied input file directly):** fetch and read all provided URLs to extract key information.

- If a URL is not scrapable (403, paywall, redirect failure), extract keywords from the URL itself and use web search on those keywords to find equivalent coverage of the same story.
- If a topic has multiple links, treat them as a single story — synthesize the key facts, figures, and angle across all links into one unified set of information for headline and summary generation.

---

## Step 3 — Generate Headline and Summary

Generate both pieces together for each topic before moving to the next. Apply the self-check before writing anything to the output file.

---

### Headline

Target length: **90–110 characters**. For `No summary.` topics: **200–230 characters**.

**Rules:**
- ALL CAPS with a dot at the end.
- **One sentence only.** No period mid-headline. If a compound structure is needed, use a comma or colon — never a full stop until the end.
- **No em dashes.** Use a comma or colon instead.
- **One statistic at most.** If the statistic is not the angle itself, cut it and save it for the summary. A headline with no statistic is fine. A headline with two or more is not.
- **Lead with the angle, not the data.** The story's consequence or irony comes first; facts and figures support it.
- **Multi-link topics:** Identify the overarching shift across all articles and lead with it. Format: `THEME: SPECIFIC DETAIL.`
- `No summary.` headlines run up to 240 characters and must be fully self-contained.

**Self-check before writing:**
- Is it one sentence? (no mid-headline period)
- Does it contain at most one statistic?
- Does it contain no em dashes?
- Does it lead with angle or consequence — not a statistic or proper noun?
- Is the character count between 90 and 110? Count before writing. For `No summary.` topics: between 200 and 230?

---

### Summary

**Skip entirely for `No summary.` topics.**

Target length: **530–590 characters**. If the written summary falls outside this range, do not trim — rewrite it entirely from scratch, targeting 560 characters.

**Rules:**
- **No em dashes. No semicolons.**
- **No sentence fragments or colon-as-a-break** ("The goal: build X"). Every sentence must be grammatically complete.
- **Find the narrative first.** Before writing, identify the dominant editorial angle in quality coverage of this story. Write from that perspective — not as a neutral list of facts, but as a coherent point of view.
- **Build cohesively.** Each sentence must follow logically from the previous. No abrupt topic jumps. The summary should read as a single flowing argument.
- Open with just enough context for a cold reader to understand what's happening, then move into the specific event. Don't lead with jargon or proper nouns without grounding them first.
- Statistics and specific figures belong here, not in the headline.
- Don't repeat what's already in the headline — extend the story.
- End on the implication or consequence, not a neutral restatement.

**Self-check before writing:**
- Does it tell a story with a clear POV, not just a list of facts?
- Does each sentence follow from the previous?
- Does it contain no em dashes or semicolons?
- Does it avoid repeating the headline?
- Does it end on a consequence or implication?
- Is the character count between 530 and 590? Count before writing. If not, rewrite from scratch targeting 560 characters.

---

## Step 5 — Categorize

Read the reference `.md` files in `reference_materials/` and assign each topic a category based on how past stories have been grouped. Do not use a fixed list — derive the category from the patterns you find in the reference data.

`No summary.` topics are typically niche, one-of-a-kind stories and will usually warrant their own specific category. Let the content and reference data guide the classification.

---

## Step 6 — Sort by Category

Before compiling output, reorder all topics so that entries sharing the same category appear consecutively. Topics within a category should remain in their original relative order. The category grouping order is not fixed — arrange category blocks in the order that makes the best editorial sense given that month's content (e.g. leading with the most news-dense category, ending with EMPLOYEE_OF_THE_MONTH if present).

This reordering must be applied to `headline_summary_output.md` — the file should reflect category-grouped order, not the order topics appeared in the input file.

---

## Step 7 — Compile & Output

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

---

## Step 8 — Output Summary Table

After saving the file, output a summary table directly in the conversation so the user can review all topics at a glance without opening the file. Format:

```
| # | Category | Format | Story |
|---|---|---|---|
| 1 | FOCUS_AI | Standard | Musk lawsuit vs OpenAI dismissed |
| 2 | FOCUS_AI | No summary | Shift AI trains robots on cleaners |
...
```

- Number entries in their final sorted order (after category grouping).
- The Story column is 2–5 words identifying the main topic at a glance — not the headline, not a sentence. Write it like a label. E.g. "Musk OpenAI lawsuit", "Ferrari EV backlash", "Anthropic SpaceX compute deal".
- Format column should read `Standard` or `No summary`.
- Follow the table with this prompt: **"Review the order and categories above. Reply with any changes — reorder, recategorise, swap, or drop — and I will update the file before we move to Phase 2."**
