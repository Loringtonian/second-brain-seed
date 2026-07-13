---
name: nano-banana-pro
description: Generate images using Google Gemini 3 Pro - up to 4K resolution, 14 reference images, advanced reasoning
user_invocable: true
allowed_tools:
  - Bash
---

# Nano Banana Pro

Generate images using Google's Gemini 3 Pro Image model. Professional quality, up to 4K resolution, advanced reasoning.

## ⚡ Launch the result

The image **is** the deliverable — show it the instant it's done instead of leaving the user to hunt for a path. After saving ANY generated image, **immediately open it** (`open <abs_path>` on macOS → Preview; `xdg-open <abs_path>` on Linux) **and** surface the file to the user, **before** writing any explanation. Lead with the image; keep commentary short. Applies to every image path.

## Trigger Conditions

Activate when:
- User says "generate image", "create image", "make me an image" AND wants high quality
- User invokes `/nano-banana-pro` or `/nbp`
- User needs higher resolution
- User needs character consistency across multiple images
- Complex prompts requiring advanced reasoning

## Model Details

| Property | Value |
|----------|-------|
| **Model ID** | `gemini-3-pro-image-preview` |
| **Speed** | Slower (more processing) |
| **Resolution** | Up to 4K |
| **Max Reference Images** | 14 (6 objects, 5 humans max) |

## Prerequisites

1. **API Key:** Set `GEMINI_API_KEY` in `.env` file
2. **Package:** `pip install google-genai Pillow python-dotenv`

Get API key: https://aistudio.google.com/app/apikey

## Usage

### Basic Generation

```python
from google import genai
from google.genai import types
from dotenv import load_dotenv
import os

load_dotenv()
client = genai.Client(api_key=os.environ.get('GEMINI_API_KEY'))

response = client.models.generate_content(
    model='gemini-3-pro-image-preview',
    contents='YOUR PROMPT HERE',
    config=types.GenerateContentConfig(
        response_modalities=['IMAGE'],
    ),
)

# Save the image, then launch it so the user sees it immediately
import subprocess
for part in response.parts:
    if part.inline_data:
        image = part.as_image()
        image.save('output.png')
        subprocess.run(['open', 'output.png'])  # macOS (use 'xdg-open' on Linux)
        print('Image saved + opened: output.png')
```

### With Aspect Ratio and Image Size

```python
response = client.models.generate_content(
    model='gemini-3-pro-image-preview',
    contents='Professional product photography of a luxury watch on marble',
    config=types.GenerateContentConfig(
        response_modalities=['IMAGE'],
        image_config=types.ImageConfig(
            aspect_ratio='4:3',
            image_size='2K',  # Options: 1K, 2K, 4K (uppercase K required)
        ),
    ),
)
```

> **SDK note:** older `google-genai` versions don't expose `types.ImageConfig`. Guard it with `if hasattr(types, 'ImageConfig'):` and only add `image_config` when present (otherwise omit it) so the call still runs across SDK versions.

### With Reference Images

```python
from PIL import Image

ref_image = Image.open('reference.png')

response = client.models.generate_content(
    model='gemini-3-pro-image-preview',
    contents=['Create a variation of this image in watercolor style', ref_image],
    config=types.GenerateContentConfig(
        response_modalities=['IMAGE'],
    ),
)
```

## Image Size Options (Gemini 3 Pro Only)

| image_size | Use Case |
|------------|----------|
| `1K` | Standard web use (default) |
| `2K` | High-quality displays |
| `4K` | Print, professional assets |

**Note:** Use uppercase K (e.g., `'2K'`). Lowercase will be rejected.

## Aspect Ratios

| Ratio | Use Case |
|-------|----------|
| `1:1` | Square (social media, profile pics) |
| `16:9` | Landscape (presentations, thumbnails) |
| `9:16` | Portrait (stories, mobile) |
| `4:3` | Standard photo |
| `3:4` | Portrait photo |
| `21:9` | Ultrawide/cinematic |

## Advanced Capabilities

- **Character consistency:** Same characters across multiple generations
- **World knowledge:** Semantic understanding for realistic scenes
- **Complex prompts:** Better understanding of nuanced instructions
- **Professional quality:** Suitable for marketing, product shots

## When to Use Pro vs Flash

| Use Case | Model |
|----------|-------|
| 4K resolution | **Pro** |
| Character consistency | **Pro** |
| Complex scenes | **Pro** |
| Professional assets | **Pro** |
| Many reference images | **Pro** |
| Quick iterations | Flash |
| Cost-sensitive | Flash |
| Batch generation | Flash |

## Limitations

- Preview status (may change)
- Higher cost ($0.134 vs $0.039)
- Slower generation
- SynthID watermark on all images

## Documentation

Full docs: https://ai.google.dev/gemini-api/docs/image-generation
