# MOSS Image ComfyUI Workflows
 
🌐 [日本語](README.md) · **English**

**Ready-to-load ComfyUI workflows for MOSS-Image (INT8 ConvRot + 8-step DMD distilled LoRA). Practical workflows covering Japanese manga pages, authentic animation storyboard sheets (e-konte), cinematic multi-panel imageboards, and loose sketch grids in a single 8-step inference pass.**

> **Author**: Kamimoto (かみもと) | Quick benchmarks & updates: **[X (@sep_is_heim)](https://x.com/sep_is_heim)** | In-depth write-ups (Japanese): **[note](https://note.com/sepiablue)**

---

## Overview

This repository is a central collection of production-ready ComfyUI workflows built around **MOSS-Image**, an open-source multimodal high-fidelity image foundation model. 

It covers practical creative use cases spanning Japanese manga page production (dynamic paneling, character banter, speech balloons, Japanese dialogue, and sound effects), commercial animation storyboard sheets, and multi-panel cinematic imageboards. Workflows leverage MOSS-Image's outstanding native text rendering, precise coordinate-based JSON layout conditioning, and blazing-fast 8-step generation.

- **8-Step High-Speed Generation**: Powered by `moss_image_base_8step_dmd.safetensors`, delivering sharp, artifact-free compositions and high-resolution visuals in just 8 sampling steps.
- **Native MOSS JSON Layout Control**: Employs bounding box coordinates (`abs_centralized` yxyx format) to render slanted manga gutters, speech balloons, onomatopoeia, ruled storyboard sheets, printed typography, and sketches simultaneously onto a single canvas.
- **Panel-by-Panel Conditioning (via ComfyUI-MOSS-Storyboard)**: In 2×3 (6-panel) grids, describe each frame independently (`[PANEL 1]` through `[PANEL 6]`), with guaranteed clean separation rules and pixel-aligned per-panel cropping.
- **Custom Style LoRA Integration**: Incorporates `SepiaBlue_Luminous_MOSS-Image_v1` (trigger: `qsv2style`) for luminous lighting, delicate colors, and cinematic anime aesthetics.

---

## Workflows Included

| Workflow File | Category | Resolution / Layout | Key Highlights & Use Case |
|---|---|---|---|
| [`page_kakeai.json`](page_kakeai.json) | Monochrome Manga Page / Banter | 1024×1536 (7-Panel Slanted Layout) | Manga artist vs. editor on deadline day. Slanted gutters, vertical/horizontal Japanese speech, SFX, and strict character bbox conditioning in one pass |
| [`ekonte_p1.json`](ekonte_p1.json) | Japanese Animation E-Konte | 1216×1440 (Portrait Sheet) | *Sky Post* (Part 1/2). Ruled table, printed gothic text & pencil sketches in a single pass |
| [`ekonte_p2.json`](ekonte_p2.json) | Japanese Animation E-Konte | 1216×1440 (Portrait Sheet) | *Sky Post* (Part 2/2). Cuts 4 through 6 concluding the storyboard sequence |
| [`imageboard_cafe.json`](imageboard_cafe.json) | Polished Imageboard Grid | 6 Panels (2×3, 4:3 each) | Everyday café atmosphere. Japanese Mincho serif captions & luminous anime style |
| [`imageboard_rainy_cat.json`](imageboard_rainy_cat.json) | Polished Imageboard Grid | 6 Panels (2×3, 4:3 each) | A girl and a stray cat on a rainy day. Emotional lighting and color storytelling |
| [`imageboard_sky_post.json`](imageboard_sky_post.json) | Polished Imageboard Grid | 6 Panels (2×3, 4:3 each) | Seaplane mail delivery across floating island outposts. Adventurous cinematic sequence |
| [`sketch_grid.json`](sketch_grid.json) | Loose Sketch Grid | 6 Panels (2×3, 16:9 each) | Monochrome sepia graphite pencil sketches. Pure composition & staging, no text |
| [`moss_storyboard_horizontal_4panels_dmd8.api.json`](moss_storyboard_horizontal_4panels_dmd8.api.json) | Horizontal 4-Panel Directive Storyboard | 4 Panels (4×1, 1:1 each, horizontal row) | MOSS-Image 8-step. Top 75% anime illustration, bottom 25% action directive note band. Tailored for Ref2VA video pipelines |
| [`h3_storyboard_ref2va_14s_bgm.api.json`](h3_storyboard_ref2va_14s_bgm.api.json) | Storyboard-to-Video 4-Shot Montage + BGM | 1024×1792 (9:16) / 14s (345F) | MiniMax H3 (FastH3 8-step). Converts the MOSS 4-panel storyboard into a seamless 4-shot video sequence with synchronized SFX and non-diegetic BGM |

---

## Detailed Breakdown

### 1. Monochrome Manga Page & Dialogue Layout

Generates a complete, publication-style seinen manga manuscript page featuring dense hatching, screentones, dramatic angles, speed lines, slanted diagonal gutters, and crisp Japanese dialogue and onomatopoeia.

- **`page_kakeai.json`**: Deadline day face-off between manga artist Mio and her editor Kurosawa (7 panels).
- **Resolution**: 1024 × 1536 (Portrait manuscript).
- **Layout Architecture**: Dynamic modern panel layout with 7 panels of varying dimensions separated by thick slanted white gutters (not a generic grid or horizontal stack).
- **Native JSON Conditioning**: Utilizes MOSS-Image's native JSON prompt format (`summary` + `items` with `abs_centralized` bounding boxes):
  - **Identity Consistency**: Strictly binds two recurring characters—Mio (silver-white bob, oversized gray hoodie) and Kurosawa (long black ponytail, rectangular glasses, tailored suit)—to individual panel coordinates and actions.
  - **Typography & Sound Effects**: Explicitly renders narration boxes (Panel 1: "締切当日、" "午前4時。"), giant bold sound effects (Panel 2: "バンッ"), vertical dialogue balloons (Panel 2: "原稿は？", Panel 4: "居留守は無駄。" "白紙も芸術です", Panel 5: "三時間で描け。", Panel 7: "コーヒーは" "淹れてあげる。"), and comedic shouts (Panel 6: "鬼ーっ！") with exact character string precision.
- **Sampler Config**: Euler / `sgm_uniform` / 8 steps / CFG 1.0

### 2. Classic Japanese Animation Storyboard Sheets (E-Konte)

Generates full-page, printed-style Japanese animation storyboard sheets on plain white paper. The layout features a 5-column ruled table containing Cut Number, Picture Frame, Shot Description, Dialogue, and Seconds.

- **`ekonte_p1.json`**: *Sky Post* Storyboard — Page 1 of 2 (Cuts 1–3)
- **`ekonte_p2.json`**: *Sky Post* Storyboard — Page 2 of 2 (Cuts 4–6)
- **Resolution**: 1216 × 1440
- **Under the Hood**: Uses MOSS-Image's native JSON prompt structure with exact coordinate-based bounding boxes (`summary` + `items` array). Printed Japanese gothic characters and rough sepia pencil sketch frames are synthesized simultaneously inside their allocated cells.
- **Sampler Config**: Euler / `sgm_uniform` / 8 steps / CFG 1.0

### 3. Polished Anime Imageboard Grids

Ideal for visual development, art direction, and visual pitch decks where full-color cinematic anime frames are presented alongside quiet, restrained captions.

- **`imageboard_cafe.json`** (Cozy café moments)
- **`imageboard_rainy_cat.json`** (Rainy shelter with a cat)
- **`imageboard_sky_post.json`** (Island flight & mail drop)
- **Layout**: 2 columns × 3 rows (6 panels total), 4:3 aspect ratio per frame.
- **Style LoRA**: `SepiaBlue_Luminous_MOSS-Image_v1.safetensors` (Weight 1.0, Trigger: `qsv2style`)
- **Custom Nodes**: Powered by `ComfyUI-MOSS-Storyboard` (`MossStoryboardLayout`, `MossStoryboardSampler`).
- **Typography & Aesthetics**: Each illustration sits above a white caption strip with a fine Japanese Mincho serif body text and shot index, mimicking printed film boards.

### 4. Loose Sketch Storyboard Grid

Focuses strictly on dynamic cinematography, character gesture, staging, and lighting contrast without typography or text distractions.

- **`sketch_grid.json`**
- **Layout**: 2 columns × 3 rows (6 panels total), cinematic 16:9 aspect ratio per frame.
- **Visual Style**: Rough graphite pencil sketches with warm sepia monochrome washes on cream paper.
- **Prompting Format**: Employs the `[STYLE] ... [PANEL 1] ... [PANEL 6]` tag format in `MossStoryboardSampler`, depicting an aviator girl flying to a remote cliff lighthouse.

### 5. Horizontal 4-Panel Directive Storyboard (Tailored for Ref2VA)

A horizontal 4-panel (1 row × 4 columns) storyboard workflow designed specifically as a structured multimodal visual reference for downstream video generation models (MiniMax H3 Ref2VA, S2V, etc.).

- **`moss_storyboard_horizontal_4panels_dmd8.api.json`**
- **Grid Layout**: 1 row × 4 columns (4 panels, 1:1 square aspect ratio each, panel_edge: 640px).
- **75:25 Split Architecture**:
  - **Upper 75%**: Luminous cinematic anime key-pose illustration.
  - **Bottom 25%**: Clean white production-note band rendering explicit action directives in bold legible typography.
- **Rendered Directive Typography**:
  - Panel 1: `1 Wave your right hand twice.`
  - Panel 2: `2 Hop once to the left.`
  - Panel 3: `3 Spin once on her palm.`
  - Panel 4: `4 Close your eyes and smile.`
  Leverages MOSS-Image's high-precision text rendering to embed exact acting directives directly onto the storyboard sheet.
- **Pipeline Role**: Serving this single image into video diffusion models locks both character visual identity and per-shot chronological action sequences in one unified conditioning input.
- **Sampler Config**: Euler / `simple` / 8 steps / CFG 1.0 (with 8-step DMD distilled LoRA).

### 6. Storyboard-to-Video 4-Shot Sequence with Audio & BGM (MiniMax H3 Ref2VA)

Loads the single image generated by `moss_storyboard_horizontal_4panels_dmd8` as a multimodal reference (`<Picture 1>`) to synthesize a **14-second, 4-shot anime sequence with synchronized sound effects and a continuous fantasy background score** in a single pass.

- **`h3_storyboard_ref2va_14s_bgm.api.json`**
- **Output Specs**: 1024 × 1792 (9:16 vertical cinematic) / 345 frames (24fps, ~14 seconds) / 4 full shots (~3.5 seconds each).
- **Storyboard Multi-Shot Unfolding**:
  - Maps the 4 panels of `<Picture 1>` directly to `[Shot 1]` through `[Shot 4]`.
  - Automatically strips sheet dividers, frames, and note bands, outputting full-screen scenes only.
  - Uses clean cinematic hard cuts between shots (no morphing artifacts) and faithfully executes the embedded acting directives (wave hand → hop left → spin on palm → close eyes and smile).
- **Integrated Multimodal Audio & Non-Diegetic BGM**:
  - **Soundscape (SFX)**: Perfectly timed sound cues per shot (Shot 1 coat rustle and forest ambience, Shot 2 soft moss landing, Shot 3 magical shimmer, Shot 4 gentle scarf rustle).
  - **Non-Diegetic Score**: An emotive 76 BPM fantasy instrumental track featuring celesta melody, delicate harp arpeggios, and soft string pads playing continuously across all four shots, resolving warmly at the end.
- **Acceleration Stack & Streaming NVENC**:
  - FastH3 8-step DiT (`fasth3_omniref_8step_pruned_int8_convrot.safetensors`) + Qwen3-VL 32B NVFP4.
  - FastH3 acceleration stack (Prefetch, ChunkFFN, ProducerReuse, BlockSparseAttention, MiniMaxH3SigmaShift).
  - 2-stage LBH (Latent Bilinear Handoff) sampling (5 low-res steps + 3 high-res steps).
  - Real-ESRGAN (`realesr-animevideov3.pth`) quality upscaling.
  - `FastH3AccelStreamSave` async NVENC hardware video encoding.

---

## Required Models & File Placement

Please ensure the required checkpoints and LoRAs are placed in their respective ComfyUI directories before loading the workflows:

| Model Type | File Name | Target Directory | Description / Role |
|---|---|---|---|
| **UNet / Diffusion (MOSS)** | `moss_image_base_int8_convrot.safetensors` / `moss_image_sft_int8_convrot.safetensors` | `ComfyUI/models/unet/` (or `diffusion_models/`) | MOSS-Image INT8 ConvRot base/SFT checkpoint |
| **CLIP / Text Encoder (MOSS)** | `qwen3.5_9b_int8_convrot.safetensors` | `ComfyUI/models/clip/` | Type: `moss_image` |
| **VAE (MOSS)** | `flux2-vae.safetensors` | `ComfyUI/models/vae/` | FLUX.2 standard VAE |
| **Acceleration LoRA (MOSS)** | `moss_image_base_8step_dmd.safetensors` (or `lora.safetensors`) | `ComfyUI/models/loras/` | 8-step DMD distilled LoRA (Weight: 1.0) |
| **Style LoRA (MOSS)** | `SepiaBlue_Luminous_MOSS-Image_v1.safetensors` | `ComfyUI/models/loras/moss/` | Used in `imageboard_*.json` (Trigger: `qsv2style`) |
| **DiT / UNet (FastH3)** | `fasth3_omniref_8step_pruned_int8_convrot.safetensors` | `ComfyUI/models/unet/` | FastH3 OmniRef 8-step DiT (`h3_storyboard_*.json`) |
| **CLIP / Text Encoder (FastH3)** | `qwen3vl_32b_minimax_h3_nvfp4_awq.safetensors` | `ComfyUI/models/clip/` | Type: `minimax` (FastH3 NVFP4/AWQ) |
| **Video VAE (FastH3)** | `minimax_h3_video_vae_int8_convrot.safetensors` | `ComfyUI/models/vae/` | MiniMax H3 Video VAE |
| **Audio VAE (FastH3)** | `minimax_h3_audio_vae_fp32.safetensors` | `ComfyUI/models/vae/` | MiniMax H3 Audio VAE (Soundscape & BGM decoding) |
| **Latent Upscaler (FastH3)** | `minimax_h3_latent_upscaler_3d_conv_v1_fp16.safetensors` | `ComfyUI/models/upscale_models/` | LBH Latent Upscaler |
| **Video Upscaler** | `realesr-animevideov3.pth` | `ComfyUI/models/upscale_models/` | Real-ESRGAN anime video upscaler |

---

## Required Custom Nodes

- **[ComfyUI-MOSS-Storyboard](https://github.com/sepiablue-ai/ComfyUI-MOSS-Storyboard)**
  - Nodes: `MossStoryboardLayout`, `MossStoryboardSampler`, `MossStoryboardCrop`
  - Installation: Clone into `ComfyUI/custom_nodes/` or install via ComfyUI Manager.
- **[ComfyUI-H3-X2-Stream](https://github.com/sepiablue-ai/ComfyUI-H3-X2-Stream)**
  - Nodes: `FastH3AccelStreamSave`, `FastH3AccelLBHHandoff` and other FastH3 optimization nodes.
- **ComfyUI**: Must include native MOSS-Image support (PR #17095 or later, recommended ComfyUI ≥ 0.42.0).

---

## Author & Community

**Kamimoto (かみもと)**  
Exploring local AI acceleration, VRAM reduction, and practical creative workflows on consumer hardware (RTX 4070 12GB).

- **X (Twitter)**: [@sep_is_heim](https://x.com/sep_is_heim) — Benchmarks, speed metrics & visual updates
- **note**: [かみもと｜ローカルAI生成](https://note.com/sepiablue) — Detailed tutorials & setup recipes
- **GitHub**: [@sepiablue-ai](https://github.com/sepiablue-ai)

### Related Repositories
- [ComfyUI-MOSS-Storyboard](https://github.com/sepiablue-ai/ComfyUI-MOSS-Storyboard): MOSS-Image storyboard & multi-panel custom nodes
- [minimax_h3_workflows](https://github.com/sepiablue-ai/minimax_h3_workflows): MiniMax H3 video workflows on 12GB VRAM
- [ComfyUI-H3-X2-Stream](https://github.com/sepiablue-ai/ComfyUI-H3-X2-Stream): INT8 X2 VAE & async NVENC video saver

---

## License

The workflow files in this repository are released under the [MIT License](LICENSE). They may be freely used, modified, redistributed, and integrated into personal or commercial projects.

*Note: Models, LoRAs, and third-party custom nodes referenced by these workflows are subject to their respective creators' and distributors' licenses.*
