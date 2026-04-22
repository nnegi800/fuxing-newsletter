# Cartier Digital Innovation Newsletter Generator

An AI-assisted workflow for producing the monthly Cartier Digital Innovation newsletter, from raw article links to finished Figma cards — ready to export.

---

## What it does

Each month, you drop in a list of article links. The tool generates headlines, summaries, image prompts, and assembles everything into Figma-ready cards. It runs across three phases, each building on the last.

---

## Workflow Overview

```
Input file (links)
      ↓
Phase 1 — Newsletter copy       → Newsletter_[Month]_Data_Structure.md
      ↓
Phase 2 — Image prompts         → image-prompts-[month].md
      ↓
Phase 3 — Figma assembly        → 10 exported PNG cards
```

---

## Phase 1 — Newsletter Generation

The AI reads the input file, fetches all articles, and produces a structured `.md` file with headlines, summaries, and categories for all 10 topics.

**🙋 Human required:**
1. **Prepare the input file** — list article links in the required format (see `CLAUDE.md`) and upload it to start the session.
2. **Review and approve the output** — read through the generated headlines and summaries and confirm before moving on. You can request edits to any entry at this stage.

---

## Phase 2 — Image Prompt Generation

The AI generates one image prompt per topic based on the approved headlines. Vertical format for standard topics, horizontal for no-summary topics.

**🙋 Human required:**
1. **Specify the art style** — tell the AI what visual style to use (e.g. "oil painting", "risograph", "ink illustration"). This sets the tone for all 10 images.
2. **Run the prompts externally** — copy the prompts from `image-prompts-[month].md` and generate images in your tool of choice (Midjourney, DALL-E, etc.).
3. **Drop images into the `/images/` folder** — place all 10 generated images inside `[month]/images/`.
4. **Confirm to proceed** — tell the AI the images are ready.

---

## Phase 3 — Figma Assembly

The AI uses the Figma Plugin API to clone templates, transfer images, set text, check overlaps, apply the background, and configure exports.

**🙋 Human required:**
1. **Paste images onto the Figma canvas** — open the Figma file and manually paste all 10 images directly onto the canvas. Names just need to include the numbers 1–10 somewhere (e.g. `image_1`, `img3`, `4`).
2. **Confirm images are pasted** — tell the AI once done so it can begin assembly.
3. **Swap fonts** — after the AI confirms text and spacing are correct, open the Font Switcher plugin in Figma and replace:
   - Inter Bold → Brilliant Cut Pro Medium
   - Roboto Regular → Fancy Cut Pro Regular
4. **Confirm font swap is done** — the AI will then apply export settings.
5. **Export the cards** — select all 10 `Topic_N` frames in Figma, scroll to Export in the right panel, and click the download button.

---

## File Structure

```
CLAUDE.md                        ← AI instructions (role, input format, workflow)
phase1_newsletter.md             ← Phase 1 detailed rules
phase2_image_prompts.md          ← Phase 2 detailed rules
phase3_figma.md                  ← Phase 3 detailed rules
README.md                        ← This file

[Month]/
  Newsletter_[Month]_Data_Structure.md   ← Phase 1 output
  image-prompts-[month].md              ← Phase 2 output
  images/                               ← Drop generated images here

reference_materials/
  March/                         ← Published March newsletter + card PNGs
  April/                         ← Published April newsletter + card PNGs
  Newsletter_Backgound_Image.png ← Shared background pattern used in Figma
```

---

## Input File Format

```
Topic 1:
- https://...
- https://...
Narrative: Focus on the competitive dynamics between OpenAI and Google.

Topic 2:
- https://...
No summary.
```

- `Topic N:` marks each story
- Multiple links per topic are synthesized into one entry
- `Narrative:` optionally steers the angle
- `No summary.` produces a headline-only card (horizontal format)

---

## Human Involvement Summary

| Step | What you do |
|---|---|
| Start Phase 1 | Upload the input file |
| End Phase 1 | Review and approve newsletter copy |
| Start Phase 2 | Specify the art style |
| During Phase 2 | Generate images externally and drop into `/images/` |
| Start Phase 3 | Paste images onto Figma canvas |
| During Phase 3 | Swap fonts using Font Switcher plugin |
| End Phase 3 | Export the 10 cards from Figma |
