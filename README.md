# VidFM 3D Probe Project Website

Project website for "How Much 3D Do Video Foundation Models Encode?" - A study quantifying 3D awareness in Video Foundation Models through model-agnostic probing.

🌐 **Live Site:** https://vidfm-3d-probe.github.io

## Overview

This repository contains the source code for the project website, including:
- Interactive radar chart for model benchmarking
- Image carousel for qualitative results  
- **Interactive 3D visualizations** using Viser for exploring reconstructed point clouds

## Repository Structure

```
vidfm-3d-probe.github.io/
├── index.html                    # Main website
├── radar_vidfm.html             # Interactive benchmark radar chart
├── static/                      # Static assets (CSS, JS, images)
│   ├── assets/                  # Paper figures and images
│   ├── css/                     # Stylesheets
│   └── js/                      # JavaScript files
├── viser-client/                # Viser web client (generated)
├── viser-scenes/                # 3D scene data
│   ├── images/                  # Scene thumbnail images
│   ├── exported/                # Exported .viser files
│   └── raw/                     # Raw scene artifacts (not in git)
├── copy_viser_scenes.sh         # Script to copy scenes from cluster
├── batch_export_viser.py        # Script to batch export .viser files
├── viser_view_export.py         # Interactive viewer with export
└── VISER_WORKFLOW.md            # Detailed workflow documentation
```

## Setting Up Interactive 3D Visualizations

The website includes interactive 3D point cloud visualizations powered by [Viser](https://github.com/nerfstudio-project/viser). Follow these steps to set them up:

### Prerequisites

```bash
pip install viser trimesh opencv-python numpy
```

### Step 1: Copy Scene Artifacts from Cluster

Ensure your cluster is mounted via sshfs, then run:

```bash
./copy_viser_scenes.sh
```

This copies scene artifacts for 8 methods × 5 scenes = 40 directories into `viser-scenes/raw/`.

**Scene mapping:**
- Scenes 1-2: CO3D dataset (`batch_15_sample_8_pred`, `batch_67_sample_6_pred`)
- Scenes 3-5: DL3DV dataset (`batch_0_sample_7_pred`, `batch_5_sample_2_pred`, `batch_8_sample_2_pred`)

**Methods:** `aether`, `cogvideox`, `dino`, `f3r`, `opensora`, `vjepa`, `wan`, `wan_14b`

### Step 2: Batch Export to .viser Format

Export all scenes to `.viser` format for web embedding:

```bash
python batch_export_viser.py
```

This processes all 40 scenes and saves them to `viser-scenes/exported/`. Each file is ~1-3 MB.

### Step 3: Build Viser Web Client

Generate the standalone web viewer (one-time setup):

```bash
viser-build-client --output-dir viser-client/
```

### Step 4: Add Scene Thumbnails

Place 5 scene thumbnail images in `viser-scenes/images/`:
```
viser-scenes/images/scene1.png  # 150x150px recommended
viser-scenes/images/scene2.png
viser-scenes/images/scene3.png
viser-scenes/images/scene4.png
viser-scenes/images/scene5.png
```

### Step 5: Test Locally

```bash
python -m http.server 8000
```

Visit http://localhost:8000/index.html and scroll to "Interactive 3D Visualizations"

### Step 6: Deploy to GitHub Pages

```bash
git add viser-client/ viser-scenes/exported/ viser-scenes/images/
git commit -m "Add interactive 3D visualizations"
git push origin main
```

## Interactive Visualization Features

Users can:
- **Select scenes:** Click thumbnail images to switch between 5 scenes
- **Select methods:** Click method buttons to view different model reconstructions
- **Interact with 3D:** Rotate, zoom, pan the point clouds
- **Toggle cameras:** Show/hide camera frustums with GUI controls
- **Adjust confidence:** Filter points by confidence percentile

## Development

### Local Testing

```bash
# Start local server
python -m http.server 8000

# View specific .viser file directly
open http://localhost:8000/viser-client/?playbackPath=http://localhost:8000/viser-scenes/exported/wan_14b_co3d_scene1.viser

# Enable camera logging (for setting initial view)
open http://localhost:8000/viser-client/?playbackPath=...&logCamera
```

### Customizing Initial Camera Positions

1. Open a scene with `&logCamera` parameter
2. Navigate to desired viewpoint
3. Copy camera position string from browser console
4. Add to iframe URL: `&initialCameraPosition=x,y,z&initialCameraLookAt=x,y,z&initialCameraUp=x,y,z`

## Citation

```bibtex
@article{huang2025vidfm3d,
  title   = {How Much 3D Do Video Foundation Models Encode?},
  author  = {Huang, Zixuan and Li, Xiang and Lv, Zhaoyang and Rehg, James M.},
  booktitle = {arXiv preprint arXiv:2512.19949},
  year    = {2025}
}
```

## Credits

- Website template adapted from [Nerfies](https://github.com/nerfies/nerfies.github.io)
- 3D visualization powered by [Viser](https://github.com/nerfstudio-project/viser)
- Interactive charts using [ECharts](https://echarts.apache.org)

## License

This project is open source and available under the MIT License.