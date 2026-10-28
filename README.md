# Awesome Image Layer Decomposition

A curated list of models, APIs, tools, plugins, datasets and papers for **image layer decomposition**: turning one flat image (a poster, a photo, an AI render) into a stack of separate RGBA layers, usually with the background repainted behind each element.

Scope: image → layers. Text-to-layered-image generation is listed only where the same model also decomposes an existing image. Everything here links to a primary source; facts are as the source states them on the date checked. Corrections and additions welcome, see [Contributing](#contributing).

Last checked: 2026-09-29.

## Contents

- [What layer decomposition is](#what-layer-decomposition-is)
- [Models and APIs](#models-and-apis)
- [Online tools](#online-tools)
- [Design-app plugins](#design-app-plugins)
- [Datasets](#datasets)
- [Papers](#papers)
- [Related lists](#related-lists)
- [Contributing](#contributing)

## What layer decomposition is

Given a single raster image, a layer decomposition model returns several images that stack back into the original: one layer per element (text, product, person, decoration) with a transparent background, plus a base image with those elements removed and the hidden areas filled in. Good results also **complete the hidden parts of each element** (amodal completion), so a cup that was behind a pumpkin comes back as a whole cup, not a cup with a hole.

It differs from background removal (one subject, background discarded), from segmentation (masks, no fill), and from vectorisation (paths, not pixels).

## Models and APIs

| Model | Access | What it returns | Notes | Source |
|---|---|---|---|---|
| **Qwen-Image-Layered** (Qwen team, Alibaba) | Open weights, Apache 2.0, 20B params | A chosen number of RGBA layers; recursive decomposition; PPTX export | Prompt describes the whole image and is *not* meant to control what lands on each layer. Needs a CUDA GPU. Weights Dec 19 2025, demo on HF Spaces and ModelScope. | [Model card](https://huggingface.co/Qwen/Qwen-Image-Layered) · [GitHub](https://github.com/QwenLM/Qwen-Image-Layered) · [Paper](https://huggingface.co/papers/2512.15603) |
| **Seedream 5.0 Flash / Pro layer decomposition** (ByteDance) | Hosted API (Volcengine / BytePlus ModelArk and resellers) | Base image + up to 16 transparent PNG layers, each with `z_index`, bounding box, name and description | Exactly one input image; prompt optional (with no prompt it splits every major element). Async task. | [BytePlus ModelArk image API](https://docs.byteplus.com/en/docs/ModelArk/1541523) · [Segmind (Pro)](https://www.segmind.com/models/seedream-5-pro-layer-decomposition) · [MindStudio (Flash)](https://www.mindstudio.ai/models/seedream-v5-0-flash-layer-decomposition) |
| **OmniPSD** (Show Lab) | Open code | Image-to-PSD decomposition and text-to-PSD generation, Flux-based, RGBA-VAE | Iterative in-context editing that extracts and erases text and foreground components. | [GitHub](https://github.com/showlab/OmniPSD) · [arXiv 2512.09247](https://arxiv.org/abs/2512.09247) |
| **LayerD** (CyberAgent AI Lab, ICCV 2025) | Open code | Layers of a raster graphic design, extracted iteratively (unoccluded foreground first) | Aimed at graphic designs rather than photos. | [GitHub](https://github.com/CyberAgentAILab/LayerD) · [arXiv 2509.25134](https://arxiv.org/abs/2509.25134) |
| **LayerDiffuse** (lllyasviel) | Open code | Transparent image *generation* with latent transparency; layer-aware variants for Forge, ComfyUI, Diffusers, Flux | Generation-side counterpart; listed because most decomposition work builds on its transparency ideas. | [GitHub](https://github.com/lllyasviel/LayerDiffuse) · [Project page](https://lllyasviel.github.io/pages/layerdiffuse/) |

## Online tools

Hosted products that take an upload and return layers. Prices and limits change; the source column is where to check.

| Tool | Runs on | Output | Notes | Source |
|---|---|---|---|---|
| **LayerGrab** | Web app; same account in Photoshop, Figma, Sketch and Chrome | Up to 16 named transparent layers over a filled background; PNG, ZIP, layered PSD; copy a layer straight into Figma/Photoshop/Canva | Name one object ("only the price tag") to get just that; credits per layer returned; free sample splits without an account | [layergrab.com](https://layergrab.com) · [How it compares with Canva Magic Layers](https://layergrab.com/canva-magic-layers-alternative) |
| **Canva Magic Layers** | Inside the Canva editor | Separate, movable elements in the Canva design | Public beta March 2026; PNG/JPG up to 5,000 px; help page does not mention a layered-file export | [Canva help](https://www.canva.com/help/editable-magic-layers/) · [PetaPixel](https://petapixel.com/2026/03/11/canvas-new-ai-tool-turns-flat-images-into-editable-layers/) |
| **Codia Magic Layers** | Web, opens in Codia Studio | Editable canvas | Free to try per its page; PNG, JPG, WebP, screenshots, posters, mockups | [codia.ai/magic-layers](https://codia.ai/magic-layers) |
| **Krea Image Breakdown** | Web (Krea nodes) | Transparent layers or a layered file; masks can be refined | Part of the Krea suite | [krea.ai](https://www.krea.ai/nodes/app/versatileprestigiouschimpanzee/image-breakdown) |
| **LlamaGen AI Layer Splitter** | Web | Separate layers keeping layout and hierarchy | | [llamagen.ai](https://llamagen.ai/features/ai-layer-splitter) |
| **RoboNeo Split Layer** | Web | Foreground and background layers | | [roboneo.com](https://www.roboneo.com/split-layer) |
| **magiclayers.net** | Web | Named transparent PNG layers | 2 credits per output image incl. base; plans from $29/month for up to 200 layers (as listed 2026-09-28) | [magiclayers.net](https://magiclayers.net/) |
| **Image2Layer** | Web | Auto or 2–10 transparent layers per run over a rebuilt background; layered PSD, transparent PNG layers or ZIP | Targeted Layer extracts one named object; any layer can be split again; paid, 8 credits per layer, packs from $3.99, free trial | [image2layer.com](https://image2layer.com/) |
| **imagetolayers.com** | Web | PNG, ZIP or layered PSD; reconstructs the background behind the subject | States 60–120 s per image | [imagetolayers.com](https://www.imagetolayers.com/) |
| **ImageLayered** | Web | Transparent, editable layers | States that it builds on Qwen-Image-Layered | [imagelayered.com](https://imagelayered.com/) |
| **PixMind image layers** | Web | Split images, export layered | | [pixmind.io](https://www.pixmind.io/ai-tools/image-tools/image-layers) |

## Design-app plugins

| Plugin | App | Notes | Source |
|---|---|---|---|
| **LayerGrab for Figma** | Figma | Splits a selected image, frame or group and groups the named layers in place; same credits as the web app | [Figma plugin page](https://layergrab.com/figma-plugin) |
| **LayerGrab for Photoshop** | Photoshop (UXP) | Adds the split as a named layer group above the image | [Photoshop plugin page](https://layergrab.com/photoshop-plugin) |
| **LayerGrab for Sketch** | Sketch | Plugins › LayerGrab; group placed over the selection | [Sketch plugin page](https://layergrab.com/sketch-plugin) |
| **LayerGrab for Chrome** | Chrome side panel | Right-click any image on the web to split it; copy or download layers | [Chrome extension page](https://layergrab.com/chrome-extension) |
| **AI Image Separator** | Figma | Detects and separates objects into layers with transparent backgrounds | [Figma Community](https://www.figma.com/community/plugin/1587846742391619054/ai-image-seperator) |
| **Layerize: Image to Figma AI** | Figma | Image to Figma layers | [Figma Community](https://www.figma.com/community/plugin/1600233534990464379/layerize-image-to-figma-ai) |

## Datasets

| Dataset | Size | What it is | Source |
|---|---|---|---|
| **MuLAn** (CVPR 2024) | 44K+ multi-layer annotations, 100K+ instance images | RGB images decomposed into instance-wise RGBA layers with occluded areas completed, built with a training-free pipeline | [Project](https://mulan-dataset.github.io/) · [arXiv 2404.02790](https://arxiv.org/abs/2404.02790) |
| **PrismLayers / PrismLayersPro** | 200K / 20K multi-layer transparent images | Open data with accurate alpha mattes, plus the ART+ generation model | [Project](https://prism-layers.github.io/) · [arXiv 2505.22523](https://arxiv.org/html/2505.22523v1) |
| **Crello** | 20K+ | Vector graphic design documents with layer structure (used as a layered-design benchmark) | via [LayerD](https://arxiv.org/abs/2509.25134) |
| **LayerGrab samples** | 6 real splits | Original, filled background, every layer and a JSON manifest with names and boxes, CC BY 4.0 | [github.com/layergrab/layergrab-samples](https://github.com/layergrab/layergrab-samples) |

## Papers

Newest first. Decomposition (image → layers) unless noted.

- **PixelART: Image-to-Layer Decomposition without Latents or Text-to-Image Pretraining** (2026) — [arXiv 2609.22789](https://arxiv.org/abs/2609.22789)
- **Does Synthetic Layered Design Data Benefit Layered Design Decomposition?** (2026) — [arXiv 2605.15167](https://arxiv.org/abs/2605.15167)
- **LiWi: Layering in the Wild** (2026) — [arXiv 2605.14552](https://arxiv.org/abs/2605.14552)
- **RevealLayer: Disentangling Hidden and Visible Layers via Occlusion-Aware Image Decomposition** (2026) — decomposition into RGBA layers from user-given bounding boxes — [arXiv 2605.11818](https://arxiv.org/abs/2605.11818)
- **Referring Layer Decomposition** (2026) — pick the layer by describing it — [arXiv 2602.19358](https://arxiv.org/abs/2602.19358)
- **Qwen-Image-Layered: Towards Inherent Editability via Layer Decomposition** (Dec 2025) — RGBA-VAE, variable-layer MMDiT, multi-stage training — [arXiv 2512.15603](https://arxiv.org/abs/2512.15603)
- **OmniPSD: Layered PSD Generation with Diffusion Transformer** (Dec 2025) — text-to-PSD and image-to-PSD — [arXiv 2512.09247](https://arxiv.org/abs/2512.09247)
- **From Inpainting to Layer Decomposition: Repurposing Generative Inpainting Models for Image Layer Decomposition** (Nov 2025) — [arXiv 2511.20996](https://arxiv.org/abs/2511.20996)
- **Controllable Layer Decomposition for Reversible Multi-Layer Image Generation** (Nov 2025) — [arXiv 2511.16249](https://arxiv.org/abs/2511.16249)
- **LayerD: Decomposing Raster Graphic Designs into Layers** (ICCV 2025) — [arXiv 2509.25134](https://arxiv.org/abs/2509.25134)
- **PrismLayers: Open Data for High-Quality Multi-Layer Transparent Image Generative Models** (May 2025, generation + data) — [arXiv 2505.22523](https://arxiv.org/abs/2505.22523)
- **PSDiffusion: Harmonized Multi-Layer Image Generation via Layout and Appearance Alignment** (May 2025, generation; background inpainting and foreground completion for occluded areas) — [arXiv 2505.11468](https://arxiv.org/abs/2505.11468)
- **MULAN: A Multi Layer Annotated Dataset for Controllable Text-to-Image Generation** (CVPR 2024) — [arXiv 2404.02790](https://arxiv.org/abs/2404.02790)
- **Transparent Image Layer Diffusion using Latent Transparency** (LayerDiffuse, 2024, generation) — [Project](https://lllyasviel.github.io/pages/layerdiffuse/)

## Related lists

- [awesome-transparent-generation](https://github.com/wendashi/awesome-transparent-generation) — transparent (layer-wise) *generation* methods
- [awesome-image-to-layer](https://github.com/s07811005141-lang/awesome-image-to-layer) — tools, models, PSD libraries and tutorials

## Contributing

Open a pull request. One line per entry, a primary source (the model card, the product's own page, the paper), and no claims the source does not make. Prices and limits go in the Notes column with the date they were read. Entries about a product from the person who makes it are fine as long as they follow the same rules; this list is maintained by [LayerGrab](https://layergrab.com), which is one of the entries.

## License

[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/). Do what you like with the list; the linked works keep their own licences.
