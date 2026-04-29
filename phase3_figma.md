# Phase 3 — Figma Assembly

**Only begin after the user explicitly approves Phase 2 image prompts and confirms images have been dropped into the `/images/` folder.**

Execute steps in strict sequence. Do not skip or combine steps.

---

## Step 1 — Image Paste (user action)

Do not attempt to read, encode, or process any image files locally. Ask the user:

> "Please paste all 10 images directly into the Figma canvas. Name them however you like — just make sure the numbers 1–10 appear in the names (e.g. `image_1`, `img1`, `1`, `photo_3`, etc.). Let me know when done."

Only proceed once the user confirms.

**Image node matching logic:** Search only among direct children of the page (`figma.currentPage.children`) — never recurse. Exclude any node whose name starts with `topic_` (case-insensitive). For topic N, find the candidate whose name contains N as a standalone number by extracting the first number sequence from the name (`node.name.match(/\d+/)[0]`). If multiple candidates match, prefer the shortest name.

---

## Step 2 — Sort by Category, Then Clone Templates

**Before cloning anything, reorder the topics by category.** Topics must be grouped so that all topics sharing a category appear consecutively. Use the category assignments from `headline_summary_output.md`.

**Sort rule:**
- Determine the first appearance order of each category (i.e. whichever category appears first in the original topic list defines its rank).
- Within each category group, topics maintain their original relative order.
- This sorted order becomes the new processing sequence for all subsequent steps. `Topic_N` names in Figma reflect this new order (Topic_1 = first topic in sorted order, Topic_2 = second, etc.), not the original input numbering.

**Example:** If original order is T1=FOCUS_AI, T2=EMPLOYEE, T3=FOCUS_AI, T4=TECHNOLOGIC — sorted order becomes T1(FOCUS_AI), T3(FOCUS_AI), T2(EMPLOYEE), T4(TECHNOLOGIC), renamed Topic_1 through Topic_4 in Figma.

For each topic in sorted order, clone the correct template group and name it `Topic_N`. Do not set any text or images yet.

**Template selection:**
| Condition | Template |
|---|---|
| First of category + has summary + image left | `Category_Vertical_Image_Left` |
| First of category + has summary + image right | `Category_Vertical_Image_Right` |
| First of category + `No summary.` | `Category_Horizontal` |
| Non-first-of-category + has summary + image left | `no_category_left_image` |
| Non-first-of-category + has summary + image right | `no_category_right_image` |

**Alternating image side — strict global rule:**
- `Category_Horizontal` topics have no left/right slot and are **skipped in the alternation sequence**.
- The alternation tracks vertical topics only. Each vertical topic flips side from the previous vertical topic.
- A new category does NOT reset the alternation — it continues from the last vertical topic.
- Example: if vertical topic 4 was LEFT, the next vertical topic (even if topic 6, skipping horizontal topic 5) must be RIGHT.

**Placement:** Stack all `Topic_N` frames vertically with zero gap (each topic's `y` = previous topic's `y + height`). This ensures the background image in Step 6 is seamless. Exports remain individual per frame.

---

## Step 3 — Transfer Images

For each `Topic_N` frame:
1. Find the matching source image node using the matching logic from Step 1.
2. Read its `imageHash` from its IMAGE fill.
3. Apply that hash to the `image` rectangle inside `Topic_N`.
4. Delete the source image node from the canvas immediately after transfer.

This is a pure Figma-to-Figma operation — no local files, no base64 encoding.

---

## Step 4 — Set Text

Set text in each `Topic_N` frame using placeholder fonts:
- `headline` layer → headline text in **Inter Bold**
- `summary` layer → summary text in **Roboto Regular**
- `category_name` layer → category label in **Inter Bold** (category group frames only)
- Set `textAutoResize = 'HEIGHT'` on all headline and summary nodes so text boxes grow with content.
- Leave `READ_STORY extra_text` untouched — it is static.

**Layer name exceptions:**
- `Category_Horizontal`: the `summary` layer holds the headline text (no actual summary); use **Inter Bold**.
- `no_category_right_image`: the headline layer is named `title`, not `headline`.

**Font sandbox note:** Locally installed fonts (Brilliant Cut Pro, Fancy Cut Pro) are never accessible via `loadFontAsync`. Always use Inter Bold for headlines/categories and Roboto Regular for summaries as placeholders. Do NOT ask the user to swap fonts yet — overlap checks must run first while Inter/Roboto are active so text heights can be measured accurately.

---

## Step 5 — Check Text Overlap

Run a script on each `Topic_N` frame enforcing two rules.

**Rule 1 — Headline must not overlap summary:**
Compare `headlineNode.y + headlineNode.height` against `summaryNode.y`. Result must be ≤ 0. Only shorten the headline if it overlaps — do not adjust it purely for spacing.

Skip Rule 1 for `Category_Horizontal` frames (no separate headline layer).

**Rule 2 — Summary must not overlap READ_STORY:**
Compare `summaryNode.y + summaryNode.height` against `readStoryNode.y`. Result must be ≤ 0. Only shorten the summary if it overflows — do not adjust it purely for spacing.

For `Category_Horizontal` frames, apply Rule 2 to the `summary` layer (which holds headline text).

**Layer name exceptions:**
- `no_category_right_image`: headline layer is named `title`.
- `READ_STORY extra text` (space) vs `READ_STORY extra_text` (underscore) — check both.

**Fixing violations:**
- R1 > 0: shorten headline slightly, re-apply, re-check.
- R2 — the bottom of the summary should reach the bottom edge of the READ_STORY box. This is the target length, not just an allowance. If the summary falls short, extend it with additional context. If it exceeds the READ_STORY box bottom, shorten it. The placeholder fonts (Inter/Roboto) run larger than the final fonts (Brilliant Cut Pro / Fancy Cut Pro), so a summary that just reaches the READ_STORY box bottom at this stage will sit above it after the font swap in Step 7 — which is the correct final state.
- After fixing any topic, update `headline_summary_output.md` to stay in sync with Figma.

---

## Step 6 — Background Image

Apply a continuous background image across all 10 topic cards using the `Newsletter_Backgound_Image_Whole` rectangle on the Figma canvas.

**Process:**
1. Read the `imageHash` from the `Newsletter_Backgound_Image_Whole` node's IMAGE fill. Also read its pixel dimensions (`imgWidth`, `imgHeight`).
2. Compute `TOTAL_SPAN` = sum of all Topic_N heights (no gaps since topics are stacked flush).
3. For each topic, find its `background` rectangle (search recursively — may be nested in a group).
4. Apply an IMAGE fill in CROP mode using this transform:

```
a = (cardWidth × imgHeight) / (imgWidth × TOTAL_SPAN)
c = (1 - a) / 2
d = topicHeight / TOTAL_SPAN
f = topicY / TOTAL_SPAN       ← topicY = sum of heights of all preceding topics

imageTransform = [[a, 0, c], [0, d, f]]
opacity = 0.42
```

This uniform-scale center-crop approach keeps the pattern undistorted and continuous across all cards.

---

## Step 7 — Link Annotations

After the background image is applied, create a text annotation to the right of each `Topic_N` frame containing the first "Read more" URL for that topic (from `headline_summary_output.md`, in sorted Figma order).

**Rules:**
- Raw URL only — no publication name, no label.
- Position: `x = topic.absoluteBoundingBox.x + topic.width + 30`, `y = topic.absoluteBoundingBox.y`
- Font: Inter Regular, 14px, colour #0000EE (standard link blue)
- Set a hyperlink on the full text: `textNode.setRangeHyperlink(0, url.length, { type: 'URL', value: url })`
- Name each annotation node `link_Topic_N` so they are easy to find and never confused with card content.
- These nodes sit outside the card bounds and are never exported — they are canvas-only references.

---

## Step 8 — Header & Footer

After all 10 `Topic_N` frames are stacked and their background image is applied, clone the `Header` and `Footer` template nodes from the canvas and position them as follows:

- **Header:** clone `Header`, place it directly above `Topic_1` — `header.y = Topic_1.y - header.height` (zero gap).
- **Footer:** clone `Footer`, place it directly below the last topic — `footer.y = Topic_10.y + Topic_10.height` (zero gap).

Name the clones `Header_Final` and `Footer_Final`. Do not set any text inside them — they are static.

---

## Step 8 — Font Swap

Once all 10 topics pass both overlap rules, output this instruction verbatim:

---
✅ **Text and spacing confirmed. Now fix the fonts in Figma:**

1. Open the **Font Switcher** plugin (or go to **Edit → Find/Replace Fonts**)
2. Replace **Inter Bold → Brilliant Cut Pro Medium** (headlines + category names)
3. Replace **Roboto Regular → Fancy Cut Pro Regular** (summaries)

This takes ~30 seconds. Let me know once done.

---

## Step 8 — Export Setup

**Before applying export settings**, the Header and Footer must be grouped into their respective topic frames:
- Group the `Header` node together with `Topic_1` (select both → Cmd+G) and rename the group `Topic_1`.
- Group the `Footer` node together with the last topic frame (select both → Cmd+G) and rename the group `Topic_10` (or whatever the last topic number is).

This ensures each exported PNG includes the header/footer as part of the card, not as a separate element.

Ask the user to confirm this grouping is done before proceeding.

Once confirmed, run a script that sets PNG 2x export settings on each `Topic_N` frame. Then output this instruction verbatim:

---
✅ **Export settings applied. To download all 10 cards:**

1. Select all 10 `Topic_N` frames on the canvas
2. In the right panel, scroll to **Export** and click the **↓** button

Each card will download as an individual PNG file.
