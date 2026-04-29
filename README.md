# 3D Gaussian Splatting — Monument Reconstruction

A Google Colab pipeline for reconstructing 3D scenes from drone video using **3D Gaussian Splatting (3DGS)**. The notebook takes a YouTube drone video, extracts frames, runs Structure-from-Motion (SfM) via HLOC + COLMAP, trains a 3DGS model, and evaluates quality with PSNR & SSIM metrics.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/YOUR_REPO_NAME/blob/main/3DGS.ipynb)

---

## Pipeline Overview

```
Drone Video (YouTube)
        ↓
  Frame Extraction (ffmpeg @ 2fps)
        ↓
  Feature Extraction (SuperPoint)
        ↓
  Feature Matching (SuperGlue)
        ↓
  Sparse Reconstruction (COLMAP / HLOC)
        ↓
  Image Undistortion (COLMAP)
        ↓
  3DGS Training (30,000 iterations)
        ↓
  Depth Estimation (MiDaS)  +  Quality Metrics (PSNR / SSIM)
```

---

## Notebook Cells

| Cell | Description |
|------|-------------|
| **Cell 1** | Mount Google Drive, download drone video via `yt-dlp`, extract frames with `ffmpeg` |
| **Cell 2** | Install dependencies: COLMAP, HLOC, diff-gaussian-rasterization, simple-knn |
| **Cell 3** | Sparse 3D reconstruction using SuperPoint + SuperGlue + COLMAP |
| **Hotfix** | Fallback cell to locate SfM files if Cell 3 path detection fails |
| **Cell 5** | Undistort images to PINHOLE model, train 3DGS for 30,000 iterations |
| **Cell 6** | MiDaS depth estimation visualization across 5 sample frames |
| **Cell 7** | Render synthetic views, compute PSNR & SSIM metrics |

---

## Requirements

- **Google Colab** with a GPU runtime (T4 or better recommended)
- **Google Drive** — outputs are saved to `MyDrive/New_Monument_Reconstruction/`

All Python packages and system dependencies are installed automatically inside the notebook.

---

## Quick Start

1. Open the notebook in Google Colab (badge above).
2. Switch to a **GPU runtime**: `Runtime → Change runtime type → T4 GPU`.
3. Run cells **in order** (Cell 1 → 2 → 3 → 5 → 6 → 7).
4. If Cell 3 fails to locate SfM files, run the **Hotfix** cell before proceeding to Cell 5.

---

## Outputs

All outputs are automatically saved to your Google Drive under `MyDrive/New_Monument_Reconstruction/`:

| Path | Contents |
|------|----------|
| `point_clouds/sparse_pointcloud.ply` | Sparse point cloud from COLMAP |
| `3dgs_output/3dgs_model_30000.ply` | Trained 3DGS model |
| `depth_maps/midas_visualization.png` | MiDaS depth map grid |
| `renders/metrics_graph.png` | PSNR & SSIM plot over trajectory |

To view the 3DGS model, drag `3dgs_model_30000.ply` into **[SuperSplat](https://supersplat.playcanvas.com/)**.

---

## Key Dependencies

- [3D Gaussian Splatting](https://github.com/graphdeco-inria/gaussian-splatting) — Kerbl et al., SIGGRAPH 2023
- [HLOC](https://github.com/cvg/Hierarchical-Localization) — Hierarchical feature-based localization
- [SuperPoint](https://github.com/magicleap/SuperPointPretrainedNetwork) — Self-supervised keypoint detector
- [SuperGlue](https://github.com/magicleap/SuperGluePretrainedNetwork) — Graph neural network feature matcher
- [COLMAP](https://colmap.github.io/) — Structure-from-Motion & MVS
- [MiDaS](https://github.com/intel-isl/MiDaS) — Monocular depth estimation

---

## Metrics Interpretation

| Metric | Good | Excellent |
|--------|------|-----------|
| PSNR   | > 25 dB | > 30 dB |
| SSIM   | > 0.75 | > 0.90 |

---

## License

This project is for educational and research purposes. Please respect the licenses of the upstream repositories listed above.
