# Fuxing Newsletter Generator

An AI-assisted workflow for producing the monthly Fuxing newsletter, from raw article links to finished Figma cards - ready to export.

---

## Workflow

| Phase | Input | Output |
|---|---|---|
| **Phase 1** - Newsletter copy | `.txt` input file of article links | `headline_summary_output.md` - headlines, summaries, categories |
| **Phase 2** - Image prompts | Approved Phase 1 `.md` + art style | `image_prompts_output.md` - one image prompt per topic |
| **Phase 3** - Figma assembly | Approved prompts + 10 generated images | 10 exported `.png` cards |

Each phase only begins once you explicitly approve the previous output.

---

## Input File

The input file is a plain `.txt` file listing article links grouped by topic. Name it `input_data_[month].txt` (e.g. `input_data_may.txt`). Full format spec in `CLAUDE.md`.

```
Topic 1:
- https://...
- https://...
Narrative: optional angle to steer the story.

Topic 2:
- https://...
No summary.
```

---

## Phase 1 - Newsletter Copy

**🙋 Human required:**
1. **Upload the input file** to start the session.
2. **Review and approve** the generated headlines and summaries. Request edits before moving on.

---

## Phase 2 - Image Prompts

**🙋 Human required:**
1. **Specify the art style** (e.g. "oil painting", "risograph", "ink illustration").
2. **Provide an API key** (optional) - if you have a key for OpenAI (DALL-E 3), Google (Imagen 3), or Stability AI, images are generated and saved automatically.
3. **If no API key** - run the prompts in your tool of choice and save all 10 images into `[month]/images/` as `1.png` through `10.png`, then confirm.

---

## Phase 3 - Figma Assembly

**🙋 Human required:**
1. **Paste all 10 images** onto the Figma canvas. Names just need to include the numbers 1–10 (e.g. `image_1`, `img3`, `4`).
2. **Confirm** images are pasted.
3. **Swap fonts** using the Font Switcher plugin in Figma:
   - Inter Bold → Brilliant Cut Pro Medium
   - Roboto Regular → Fancy Cut Pro Regular
4. **Confirm** font swap is done.
5. **Export** - select all 10 `Topic_N` frames, click Export in the right panel.

---

## Human Involvement Summary

| Step | What you do |
|---|---|
| Start Phase 1 | Upload the input file |
| End Phase 1 | Review and approve newsletter copy |
| Start Phase 2 | Specify the art style |
| During Phase 2 | Provide API key for auto-generation, or generate manually and drop into `/images/` |
| Start Phase 3 | Paste images onto Figma canvas |
| During Phase 3 | Swap fonts using Font Switcher plugin |
| End Phase 3 | Export the 10 cards from Figma |

---

## File Structure

```
CLAUDE.md                         ← AI instructions
phase1_newsletter.md              ← Phase 1 rules
phase2_image_prompts.md           ← Phase 2 rules
phase3_figma.md                   ← Phase 3 rules

[Month]/
  headline_summary_output.md      ← Phase 1 output
  image_prompts_output.md         ← Phase 2 output
  images/                         ← Drop generated images here

reference_materials/
  March/                          ← Published March newsletter + card PNGs
  April/                          ← Published April newsletter + card PNGs
  Newsletter_Backgound_Image.png  ← Background pattern used in Figma
```
