# Phase 2 — Image Prompt Generation

**Do not begin Phase 2 until the user explicitly approves the Phase 1 output or says "let's move to phase 2".**

---

## Trigger

Once Phase 1 is approved, if no art style has been provided, ask:
> "What art style should I use for this month's images?"

---

## Process

For each topic:
1. Pull the headline from the approved Phase 1 output.
2. Determine format:
   - `No summary.` topics → **HORIZONTAL (16:9)**
   - All other topics → **VERTICAL (9:16)**
3. Extract from the headline:
   - **Company logo(s):** any brand, company, or organization named
   - **Main theme:** the core visual concept the image should evoke
4. Assemble the image prompt using the template below.

---

## Prompt Templates

**VERTICAL:**
```
I am creating a newsletter that is a monthly overview of the most important digital news. Help me create a square thumbnail picture evoking the following story.
The title is: [HEADLINE]
Please follow the artistic style of [art style] art. Do not put any text in the final image. The format of the final picture should be VERTICAL (9:16 ratio). Include company logo(s) ([logos]) and the main theme ([theme]) to emphasize the main topic of the article.
```

**HORIZONTAL:**
```
I am creating a newsletter that is a monthly overview of the most important digital news. Help me create a square thumbnail picture evoking the following story.
The title is: [HEADLINE]
Please follow the artistic style of [art style] art. Do not put any text in the final image. The format of the final picture should be HORIZONTAL (16:9 ratio). Include company logo(s) ([logos]) and the main theme ([theme]) to emphasize the main topic of the article.
```

---

## Output

Compile all prompts into a single file: `image_prompts_output.md` inside that month's folder.
Label each entry as `Topic N — VERTICAL` or `Topic N — HORIZONTAL`.

Once the image prompts are approved, create the following directory inside that month's folder:

```
[month]/
└── images/
```

The folder is left empty, ready for the user to drop in generated images after running the prompts externally.
