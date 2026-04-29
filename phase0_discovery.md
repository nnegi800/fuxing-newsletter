# Phase 0 — Autonomous News Discovery

**Run Phase 0 at the start of each month, before any input file exists. Skip Phase 0 if the user provides their own `input_data_[month].txt` directly.**

---

## Step 1 — Load Editorial Context

Before searching, read the following to calibrate story selection:

1. `reference_materials/March/Newsletter_March_Data_Structure.md`
2. `reference_materials/April/Newsletter_April_Data_Structure.md`
3. The `## Phase 0 — Editorial Intelligence` section of `CLAUDE.md` — patterns learned from past reasoning about what made stories good fits. Starts empty, grows over time.

From these, derive:
- Which **categories** have appeared and how they are named
- Which **story archetypes** score well (unexpected angle, contrarian investment play, quirky human story with a tech twist, benchmark that reframes an AI narrative)
- Which **editorial patterns** have been reinforced as strong fits in past months

---

## Step 2 — Search for Candidates

Run targeted searches covering the full target month. Search across the newsletter's core category areas:

- AI model releases, capabilities, benchmarks
- AI company strategy, funding, acquisitions, leadership
- AI regulation, government contracts, geopolitics
- AI in creative fields (art, film, music)
- Unusual or quirky tech stories with a digital/AI thread
- Investment plays driven by AI infrastructure
- Space and deep tech
- Tech workforce shifts (layoffs, restructuring, lean teams)

For **niche and quirky stories**, also search trending topics on X (tech Twitter) — unusual high-signal stories surface there earliest, before mainstream coverage catches up. Search queries like `site:twitter.com OR "trending on X" tech niche [month] [year]` or look for stories shared heavily in tech circles that haven't made it to TechCrunch yet.

Use built-in web search. Run 8–12 targeted queries, one per category area above.

Collect raw candidates until you have at least 20 distinct stories. Deduplicate by story (multiple articles covering the same event = one candidate, keep the best source URLs).

---

## Step 3 — Score and Filter

Apply the editorial patterns loaded in Step 1. For each candidate, assess:

- Does it fit an established newsletter category?
- Does it have a non-obvious angle (not just a product launch recap)?
- Would it surprise or inform a technically literate luxury-brand audience?
- Is it from the target month's date range?

**Additional filters (updated April 2026):**
- "AI discovers/finds science thing" stories are too common — skip unless the result is genuinely unprecedented (e.g. first time a robot beats a professional athlete in a live sport, published in a top journal by a major brand). A new benchmark or dataset discovery does not clear the bar
- Multi-thread stories are stronger than single-event stories when three independent developments all point to the same underlying shift — look for convergence across candidates and consider merging them into one entry
- Platform eats its partner is a strong angle: a company launching a product that directly competes with a major investor or partner (the irony is the story)
- Paradigm shift in how AI is used — a new discipline or mental model replacing the previous one — works well as a Word of the Month no-summary card

**Story pairing principle (updated April 2026):**
A standalone industry stat is rarely enough on its own — it needs a paired story that makes the number feel real and personal to the reader. The formula is: **broad industry stat + specific controversial or trending development**. The paired story should be something impactful, a visible change in how things work, and ideally trending on X (tech Twitter) where it is already generating debate. For example: "Q1 AI funding hit a record" is flat on its own, but pairing it with USVC — a fund that for the first time lets anyone invest in OpenAI and Anthropic from $500, no accreditation required — gives the stat a human consequence. When you find a large industry number (funding record, job loss figure, compute spending), actively search for a second story that answers "so what does this mean for a person" and combine them into one candidate entry.

Discard candidates that fail clearly. Keep the top ~20 ranked by fit.

---

## Step 4 — Compile Candidates Doc

Save as `[month]/phase0_candidates.md`.

Each entry uses this format:

```md
## CANDIDATE [N]
**Status:** KEEP
**Category suggestion:** [CATEGORY_NAME]
**Format:** Standard | No summary
**Source(s):** [Publication](url) · [Publication](url)
**Published:** [date]

**Why this belongs:**
One short paragraph. Explain what makes this story fit — which newsletter archetype
it matches, what angle is non-obvious, why the Cartier DI audience would find it
relevant. Reference a past newsletter story if there is a close parallel.

**Rejection reason:** *(leave blank, or fill in if rejecting)*

---
```

- `Format: No summary` = headline-only horizontal card. Use for quirky one-liner stories only.
- Do not write draft headlines or summaries — those are generated in Phase 1.

At the end of the doc, include a **Your Additional Topics** section (see Step 5).

Show the user a summary once the doc is ready:
> "Found [N] candidates for [Month] in `[month]/phase0_candidates.md`. Review the entries, update any statuses, add topics to the Additional Topics section if needed, then save the file and send: **'Phase 0 ready — please process `[month]/phase0_candidates.md`'**"

---

## Step 5 — Additional Topics (User-Directed Search)

The `Your Additional Topics` section at the bottom of `phase0_candidates.md` is where the user can write topic descriptions or headlines they want covered — without needing to find articles themselves.

Format the section as:

```md
---

## Your Additional Topics

*Write a topic description or headline below and I will find the exact articles and add a full candidate entry above.*

- 
```

When the user adds entries here, treat each line as a search prompt: find the best 1–3 source URLs for that story, write a full candidate entry (same format as Step 4), and append it to the candidates section. Then confirm:
> "Found [N] additional topics. Entries added to `phase0_candidates.md`."

---

## Step 6 — Feedback Loop

Once the user confirms they are done with the doc, run both feedback loops:

**Modification loop:**
For each entry where `Status: MODIFY`, read the modification note and apply it before generating `input_data_[month].txt`:
- **Combine:** merge the two (or more) flagged candidates into a single entry, synthesizing their URLs and narrative angles
- **Add narrative:** append the user's additional angle to the existing entry's narrative

**Rejection loop (negative signal):**
For each entry where `Status: REJECT` and a rejection reason is provided, synthesize patterns across all rejections and append to `CLAUDE.md` under `## Phase 0 — Editorial Intelligence`:

```md
### [Month] [Year] — Topics to deprioritise
- [Synthesized rule] (e.g. "Skip pure US politics stories unless there is a direct AI company or policy impact angle")

### Rejection Log
| Month | Story description | Reason |
|---|---|---|
| [Month Year] | [brief story description] | [rejection reason] |
```

**Positive reinforcement loop (user-added topics only):**
For each entry that came from the `Your Additional Topics` section, read its **Why this belongs** reasoning. Synthesize what these user-chosen stories reveal about editorial priorities the automated search should weight more heavily, and append to `CLAUDE.md`:

```md
### [Month] [Year] — What user additions reveal
- [Synthesized pattern] (e.g. "User added two stories about AI in physical/industrial contexts — weight this angle higher in future searches")
```

This builds a two-sided signal in `CLAUDE.md`: rejections teach what to avoid, user additions teach what to prioritise.

---

## Step 7 — Generate Phase 1 Input Template

Once all modifications, additional topics, and feedback loops are complete, generate a blank `[month]/input_data_[month].txt` with 10 empty topic slots:

```
Topic 1:
- 
Narrative: 

Topic 2:
- 
Narrative: 

Topic 3:
- 
Narrative: 

Topic 4:
- 
Narrative: 

Topic 5:
- 
Narrative: 

Topic 6:
- 
Narrative: 

Topic 7:
- 
Narrative: 

Topic 8:
- 
Narrative: 

Topic 9:
- 
Narrative: 

Topic 10:
- 
Narrative: 
```

Then output this message verbatim:

---
✅ **Phase 0 complete.**

A blank input template has been created at `[month]/input_data_[month].txt`.

Fill in your 10 topics — paste links under each `Topic N:` and add a `Narrative:` angle if you have one. Add `No summary.` instead of a narrative for headline-only topics. Use `phase0_candidates.md` as a reference if helpful.

When ready, come back and say **"Ready for Phase 1"** and I'll generate the headlines, summaries, and categories.

---
