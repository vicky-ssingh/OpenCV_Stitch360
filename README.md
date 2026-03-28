# 🌐 OpenCV 360° Panorama Stitcher

A Jupyter-friendly image stitching pipeline that combines multiple overlapping
photos into a seamless 360° panorama using OpenCV's powerful `cv2.Stitcher` engine,
wrapped inside a clean object-oriented `AdvancedStitcher` class.

---

## 📌 Project Overview

| Item | Detail |
|---|---|
| **Algorithm** | OpenCV `cv2.Stitcher` (SIFT + Bundle Adjustment + Spherical Warp + Blending) |
| **Modes** | `PANORAMA` (360° rotating-camera) and `SCANS` (flat document stitching) |
| **Language** | Python 3 |
| **Environment** | Jupyter Notebook (local) |
| **Output** | `stitched_output.jpg` saved in the project folder |

---

## 📁 Folder Structure

```
your_project/
│
├── OpenCv_stitch360_enhanced.ipynb   ← Main notebook
├── stitched_output.jpg               ← Generated panorama (after running)
├── README.md                         ← This file
│
└── data/                             ← Put ALL your input images here
    ├── 1.jpeg
    ├── 2.jpeg
    ├── 3.jpeg
    └── ...
```

---

## 🖼️ Input Image Requirements

| Requirement | Details |
|---|---|
| **Formats** | `.jpg`, `.jpeg`, `.png` |
| **Minimum count** | At least **2** images (10–30 recommended for 360°) |
| **Overlap** | Each consecutive image should share **30–50%** of its area with the next |
| **Camera motion** | Rotate the camera **horizontally in place** — do NOT walk sideways |
| **Lighting** | Keep exposure consistent across all shots for smooth blending |
| **Naming** | Name files numerically: `01.jpg`, `02.jpg` ... so they sort correctly |

---

## ⚙️ Setup & Installation

### 1. Clone or download this repository

```bash
git clone https://github.com/your-username/opencv-stitch360.git
cd opencv-stitch360
```

### 2. Install required libraries

```bash
pip install opencv-contrib-python matplotlib numpy
```

> ⚠️ Use `opencv-contrib-python` (NOT plain `opencv-python`) —
> the contrib package includes advanced modules like SIFT used by the Stitcher.

### 3. Add your images

Create a `data/` folder in the project directory and place your `.jpg` / `.png`
images inside it.

### 4. Launch Jupyter Notebook

```bash
jupyter notebook OpenCv_stitch360_enhanced.ipynb
```

---

## 🚀 How to Run

Run the notebook cells **top to bottom** in order:

| Cell | What It Does |
|---|---|
| **Cell 0** | Install `opencv-contrib-python` (run once) |
| **Cell 1** | Import libraries |
| **Cell 2** | Define `show_image()` display helper |
| **Cell 3** | Define the `AdvancedStitcher` class |
| **Cell 4** | Set `FOLDER_PATH`, `SCALE`, and `MODE` — check folder exists |
| **Cell 5** | Create stitcher object and load images |
| **Cell 6** | Preview first 4 loaded images (optional verification) |
| **Cell 7** | Run stitching → display and save `stitched_output.jpg` |
| **Cell 8** | Optional: retry with `SCANS` mode if `PANORAMA` fails |

---

## 🔧 Configuration Options

Open **Cell 4** to adjust these settings:

```python
FOLDER_PATH = 'data'      # Path to your images folder
SCALE = 0.5               # Image resize factor (0.2 = fast/low quality, 1.0 = full quality)
MODE  = 'PANORAMA'        # 'PANORAMA' for 360° shots, 'SCANS' for flat document pages
```

### Scale Guide

| SCALE | Use Case | Speed |
|---|---|---|
| `0.2` | Quick testing | ⚡⚡⚡ Fastest |
| `0.5` | General use | ⚡⚡ Balanced |
| `1.0` | Final output | ⚡ Slowest, best quality |

---

## 🧠 How It Works (Pipeline)

```
Input Images (data/)
       │
       ▼
load_and_preprocess()
  └─ glob finds all .jpg/.jpeg/.png files
  └─ cv2.imread() loads each as a BGR NumPy array
  └─ cv2.resize() scales images (if scale != 1.0)
       │
       ▼
stitch_advanced()
  └─ cv2.Stitcher_create(PANORAMA or SCANS)
  └─ Feature Detection  → SIFT key-points in every image
  └─ Feature Matching   → match key-points across image pairs
  └─ Bundle Adjustment  → globally optimise all camera angles
  └─ Warping            → spherical or affine projection
  └─ Blending           → smooth seam merging
       │
       ▼
stitched_output.jpg  +  inline display in Jupyter
```

---

## 🛠️ Troubleshooting

| Problem | Likely Cause | Fix |
|---|---|---|
| `ERR_NEED_MORE_IMGS` | Too few / non-overlapping images | Add more; aim for 30–50% overlap |
| `ERR_HOMOGRAPHY_EST_FAIL` | Dark, blurry, or featureless photos | Use sharp, well-lit, textured scenes |
| `ERR_CAMERA_PARAMS_ADJUST_FAIL` | Mixed cameras or sideways movement | Rotate in place; use same camera |
| Seam lines visible | Exposure differences | Normalise brightness before shooting |
| Very slow | Images too large | Lower `SCALE` to `0.3` or `0.2` |
| Black borders in output | Normal after spherical warp | Crop manually or ignore |
| `cv2` import error | Wrong OpenCV package | Run `pip install opencv-contrib-python` |
| Images in wrong order | Non-numeric filenames | Rename: `01.jpg`, `02.jpg`, ... |

---

## 💡 Key Concepts

| Term | Plain English |
|---|---|
| **SIFT** | Detects distinctive corners/blobs in images for matching |
| **Key-point** | A distinctive pixel location used to align images |
| **Bundle Adjustment** | Simultaneously optimises ALL camera angles for best global fit |
| **Spherical Warping** | Maps flat images onto a sphere — enables true 360° panoramas |
| **Affine Warping** | Flat parallel projection — best for side-by-side document scans |
| **BGR vs RGB** | OpenCV = Blue-Green-Red; Matplotlib = Red-Green-Blue (must convert!) |
| **glob** | Python module to find files matching wildcard patterns (`*.jpg`) |

---

## 📦 Dependencies

```
opencv-contrib-python >= 4.5.0
numpy                 >= 1.21.0
matplotlib            >= 3.4.0
```

---

## 📝 Notes

- This notebook is **Jupyter-friendly** — no Google Colab patches needed.
- All image display uses `matplotlib` (not `cv2.imshow`) for inline rendering.
- The `AdvancedStitcher` class is reusable — instantiate it with any folder path.

---

## 👤 Author

Built as part of a Computer Vision internship project.  
Focuses on 360° panorama generation using OpenCV's automatic stitching engine.
