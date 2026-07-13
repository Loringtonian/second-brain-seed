---
name: nano-banana-flash
description: Generate images using Google Gemini 2.5 Flash - fast, 1024x1024, up to 3 reference images
user_invocable: true
allowed_tools:
  - Bash
---

# Nano Banana Flash

Generate images using Google's Gemini 2.5 Flash Image model. Fast, optimized for high-volume generation.

## ⚡ Launch the result

The image **is** the deliverable — show it the instant it's done instead of leaving the user to hunt for a path. After saving ANY generated image, **immediately open it** (`open <abs_path>` on macOS → Preview; `xdg-open <abs_path>` on Linux) **and** surface the file to the user, **before** writing any explanation. Lead with the image; keep commentary short. Applies to every image path.

## Trigger Conditions

Activate when:
- User says "generate image", "create image", "make me an image"
- User invokes `/nano-banana-flash` or `/nbf`
- User wants quick image generation
- High-volume batch image generation needed

## Model Details

| Property | Value |
|----------|-------|
| **Model ID** | `gemini-2.5-flash-image` |
| **Speed** | Fast (optimized for latency) |
| **Resolution** | 1024x1024 default |
| **Max Reference Images** | 3 |

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
    model='gemini-2.5-flash-image',
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

### With Aspect Ratio

```python
response = client.models.generate_content(
    model='gemini-2.5-flash-image',
    contents='YOUR PROMPT HERE',
    config=types.GenerateContentConfig(
        response_modalities=['IMAGE'],
        image_config=types.ImageConfig(
            aspect_ratio='16:9',  # Options: 1:1, 2:3, 3:2, 3:4, 4:3, 4:5, 5:4, 9:16, 16:9, 21:9
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
    model='gemini-2.5-flash-image',
    contents=['Combine these in a surreal art style', ref_image],
    config=types.GenerateContentConfig(
        response_modalities=['IMAGE'],
    ),
)
```

## Aspect Ratios

| Ratio | Use Case |
|-------|----------|
| `1:1` | Square (social media, profile pics) |
| `16:9` | Landscape (presentations, thumbnails) |
| `9:16` | Portrait (stories, mobile) |
| `4:3` | Standard photo |
| `3:4` | Portrait photo |
| `21:9` | Ultrawide/cinematic |

## When to Use Flash vs Pro

| Use Case | Model |
|----------|-------|
| Quick iterations | **Flash** |
| Batch generation | **Flash** |
| Cost-sensitive | **Flash** |
| 4K resolution needed | Pro |
| Complex prompts | Pro |
| Character consistency | Pro |

## Limitations

- Max 3 reference images
- 1024x1024 max resolution (varies by aspect ratio)
- SynthID watermark on all images
- Some geographic restrictions (EU, MEA)

## Documentation

Full docs: https://ai.google.dev/gemini-api/docs/image-generation
