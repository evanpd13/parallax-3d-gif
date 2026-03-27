# Parallax 3D GIF Generator

Turn any single image into a 3D parallax GIF. Uses monocular depth estimation, AI segmentation, and neural inpainting to separate the scene into multiple depth layers, then renders smooth sub-pixel orbital camera motion with realistic parallax.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/evanpd13/parallax-3d-gif/blob/main/parallax_3d_gif.ipynb)

## Pipeline

| Stage | Model / Method | What it does |
|-------|---------------|--------------|
| 1. Depth | [Depth Anything V2](https://huggingface.co/depth-anything/Depth-Anything-V2-Small-hf) | Monocular depth estimation |
| 2. Segmentation | [RMBG-2.0](https://huggingface.co/briaai/RMBG-2.0) | Foreground/background separation |
| 3. Inpainting | [LaMa](https://github.com/enesmsahin/simple-lama-inpainting) | Multi-layer backing plates with aggressive mask dilation |
| 4. Rendering | `cv2.remap` sub-pixel warper | Back-to-front layer compositing with bilinear interpolation |
| 5. Assembly | Pillow | Ping-pong GIF output |

## Quick Start (Colab)

1. Click the **Open in Colab** badge above
2. Run all cells
3. Upload an image when prompted
4. Download the generated 3D GIF

## Configuration

Edit the config cell to tune the effect:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `FRAMES` | 9 | Number of viewpoints to render |
| `ARC_DEG` | 7.0 | Total camera arc in degrees |
| `FPS` | 12 | GIF frame rate |
| `PING_PONG` | True | Bounce animation back and forth |
| `MAX_DIM` | 2048 | Max image dimension |
| `BG_PARALLAX` | 2.5 | Background shift multiplier |
| `BG_BLUR` | 3 | Depth-of-field blur on background |
| `NUM_LAYERS` | 4 | Depth layers (more = smoother parallax, slower inpainting) |
| `MASK_DILATE_K` | 21 | Inpaint mask dilation kernel size |
| `MASK_DILATE_I` | 5 | Inpaint mask dilation iterations |

## Requirements

- Python 3.8+
- CUDA GPU (recommended, runs on CPU but slowly)
- Hugging Face account (for model access)

Install locally:

```bash
pip install -r requirements.txt
```

## Outputs

- `*_3d.gif` — the parallax GIF
- `depth.png` — depth map visualization
- `mask.png` — foreground segmentation mask
- `bg_inpainted.png` — inpainted background
- `frames/` — individual PNG frames

## License

MIT
