# Hatch Pet for Claude Code

**A generic, image-generation-agnostic adaptation of the Codex pet hatching skill.**

Create, repair, validate, preview, and package Codex-compatible animated pet spritesheets from character art, screenshots, generated images, or visual references. Works with any coding agent that supports image generation tools.

---

**[Jiaxin Kou 寇佳新](https://www.hypersampling.com)** · **[Neta Art 捏Ta](https://www.neta.art)** · [GitHub @kjx-talesofai](https://github.com/kjx-talesofai)

---

## What Is It

Hatch Pet generates a complete 9-state animated sprite atlas (1536×1872, 8×9 grid of 192×208 cells) plus preview videos, contact sheets, and a `pet.json` manifest — all from a single character concept or reference image.

This adaptation removes the hard dependency on OpenAI Codex CLI's built-in `$imagegen` tool, making the skill work with **any image generator** that supports text prompts + reference image grounding (e.g., GPT Image, Gemini, DALL-E, Stable Diffusion, etc.).

## What's Different from the Original

| | Original (OpenAI) | This Adaptation |
|---|---|---|
| **Image generation** | Hardcoded to Codex CLI `$imagegen` | Image-gen-agnostic; discovers available tools |
| **Reference grounding** | Required Codex native image_gen | Works with any generator supporting image refs |
| **Output location** | `~/.codex/pets/` | Local `./output/pets/` by default |
| **Frame consistency** | Not explicitly enforced | Per-state prompt constraints to lock body size |
| **Provenance** | Required `~/.codex/generated_images/...` | Relaxed for generic environments |

## Installation

```bash
claude skill install https://github.com/kjx-talesofai/claude-skill-codex-pet
```

Or clone manually:

```bash
git clone https://github.com/kjx-talesofai/claude-skill-codex-pet.git
```

## Requirements

- Python 3.10+ with Pillow
- ffmpeg (for preview video generation)
- An image generation tool with:
  1. Text-to-image generation
  2. **Reference image grounding** (required for row strips)
  3. PNG output support
  4. Reasonable layout adherence (exact frame count, flat background)

## Usage

```text
Hatch a Codex pet for [character name] from [reference image or description]
```

The skill will:
1. Prepare a run directory with prompts and layout guides
2. Generate a base character image
3. Generate 9 animation rows (idle, running-right, running-left, waving, jumping, failed, waiting, running, review)
4. Extract frames, compose the atlas, validate, and package

See `SKILL.md` for the full workflow, prompt engineering guide, and subagent delegation patterns.

## Repo Structure

```
.
├── SKILL.md              # Full skill definition and workflow
├── LICENSE.txt           # Apache 2.0 (original work)
├── README.md             # This file
├── agents/
│   └── openai.yaml       # Original Codex agent manifest (unused in Claude)
├── references/
│   ├── animation-rows.md     # Frame counts and durations
│   ├── codex-pet-contract.md # Atlas spec
│   └── qa-rubric.md          # QA checklist
└── scripts/
    ├── prepare_pet_run.py
    ├── record_imagegen_result.py
    ├── finalize_pet_run.py
    ├── compose_atlas.py
    ├── extract_strip_frames.py
    ├── validate_atlas.py
    ├── make_contact_sheet.py
    ├── render_animation_videos.py
    ├── package_custom_pet.py
    ├── pet_job_status.py
    ├── queue_pet_repairs.py
    ├── derive_running_left_from_running_right.py
    ├── inspect_frames.py
    └── generate_pet_images.py   # OpenAI API fallback
```

## Attribution

This is a derivative work of the **hatch-pet** skill from [openai/skills](https://github.com/openai/skills/tree/main/skills/.curated/hatch-pet).

- **Original work**: © OpenAI — [Apache License 2.0](LICENSE.txt)
- **Adaptation**: Modified `SKILL.md` to remove Codex CLI dependencies, add generic image generation support, enforce frame consistency, and default to local output paths. All original scripts, references, and the license remain unchanged.

## License

The original work and this derivative are licensed under the [Apache License, Version 2.0](LICENSE.txt).

---

© 2026 Jiaxin Kou 寇佳新 · Neta Art 捏Ta
