# Phase 2 - Image Prompt Generation

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
   - `No summary.` topics - **HORIZONTAL (16:9)**
   - All other topics - **VERTICAL (9:16)**
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
Label each entry as `Topic N - VERTICAL` or `Topic N - HORIZONTAL`.

Once the image prompts are approved, create the following directory inside that month's folder:

```
[month]/
└── images/
```

---

## Image Generation

Once prompts are saved and the `/images/` folder is created, ask:
> "Do you have an API key for image generation? If yes, which service: OpenAI (DALL-E 3), Google (Imagen 3), or Stability AI?"

---

### Path A - Automated (API key provided)

**Before running, inform the user:**
> "Generating 10 images will cost approximately $0.80 with DALL-E 3, or similar with other services. Shall I proceed?"

Only proceed after confirmation.

**Execution:**
- Use the prompts from `image_prompts_output.md` exactly as written - do not modify them.
- Generate images sequentially (one at a time) to avoid rate limits.
- Save each image to `[month]/images/N.png` where N matches the topic number.
- If a single image fails, retry once before flagging it to the user for manual generation.
- The API key is used only for this session and never written to disk.

**Per-service API details:**

**OpenAI (DALL-E 3)**
```bash
curl https://api.openai.com/v1/images/generations \
  -H "Authorization: Bearer $API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "dall-e-3",
    "prompt": "[PROMPT]",
    "size": "[SIZE]",
    "response_format": "url"
  }'
```
- VERTICAL size: `1024x1792`
- HORIZONTAL size: `1792x1024`
- Response: download from the returned URL and save as `N.png`

**Google (Imagen 3)**
```bash
curl -X POST \
  "https://generativelanguage.googleapis.com/v1beta/models/imagen-3.0-generate-002:predict?key=$API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "instances": [{"prompt": "[PROMPT]"}],
    "parameters": {"aspectRatio": "[RATIO]", "sampleCount": 1}
  }'
```
- VERTICAL ratio: `"9:16"`
- HORIZONTAL ratio: `"16:9"`
- Response: base64-encoded image - decode and save as `N.png`

**Stability AI**
```bash
curl -X POST \
  "https://api.stability.ai/v2beta/stable-image/generate/core" \
  -H "Authorization: Bearer $API_KEY" \
  -H "Accept: image/*" \
  -F prompt="[PROMPT]" \
  -F aspect_ratio="[RATIO]" \
  -o "[month]/images/N.png"
```
- VERTICAL ratio: `9:16`
- HORIZONTAL ratio: `16:9`
- Response: image written directly to file

Once all 10 images are saved, confirm to the user and proceed to Phase 3.

---

### Path B - Manual (no API key)

Output this instruction:

> "Here are your 10 image prompts in `image_prompts_output.md`. Run each one in your image generation tool of choice (Midjourney, DALL-E, Firefly, etc.), then save the outputs as `1.png` through `10.png` inside `[month]/images/`. Let me know when all 10 are in the folder."

Wait for the user to confirm before proceeding to Phase 3.
