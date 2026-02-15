# EBA-QR: Entropy-Based Adaptive Quantum Image Representation

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1tUTHOa5iyWGzKdAlK3WqlHbP5oezR9OP?usp=sharing)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Qiskit](https://img.shields.io/badge/Qiskit-0.44-purple.svg)](https://qiskit.org/)
[![arXiv](https://img.shields.io/badge/arXiv-2501.XXXXX-b31b1b.svg)](https://arxiv.org/)

**Official implementation** of:

> **“EBA-QR: An Entropy-Based Adaptive Quantum Image Representation Framework for Efficient Multi-Modal Image Processing”**  
> *Tanisha Debnath* – Institute of Engineering & Management, Kolkata, India.

---

##  Overview

Quantum Image Processing (QIP) promises powerful speedups but is heavily constrained by the cost of encoding classical images into quantum states on NISQ devices. Classical QIR models such as FRQI and NEQR are **content-agnostic**, spending the same gate budget on background and Regions of Interest (ROIs), which is wasteful for sparse scenes like SAR ship detection or tumor-focused MRI scans.

**EBA-QR** (Entropy-Based Adaptive Quantum Representation) is a **content-aware** quantum image representation that:

- Computes **local Shannon entropy** over image blocks.
- Uses a tunable threshold to classify blocks as ROI vs. background.
- Allocates **high-precision NEQR-style gates** only to high-entropy blocks.
- Applies **gate-skipping** to background blocks so their cost approaches **O(1)** per block.

This yields large gate-count reductions while preserving NEQR-level fidelity in ROIs across SSDD SAR, brain tumor MRI and ICEYE SAR imagery.

---

##  One-Click Reproducibility (Colab)

All experiments, figures and tables from the paper are implemented in a **single Colab notebook**:

👉 **[Open the EBA-QR Colab Notebook](https://colab.research.google.com/drive/1tUTHOa5iyWGzKdAlK3WqlHbP5oezR9OP?usp=sharing)**

The notebook includes:

- Dataset preparation and loading for:
  - **Brain Tumor MRI** (Figshare).
  - **SSDD** SAR ship detection.
  - **ICEYE** SAR (e.g., Doha Airport).
- EBA-QR circuit construction and the **NEQR+Mask** control baseline.
- Gate-cost, depth and ROI-fidelity evaluation.
- Ablation studies (entropy threshold, block size).
- Visualization of entropy maps, ROI masks, geometric transforms and Sobel edge detection.

No local setup is required: just run the notebook cells top to bottom.

---

##  Datasets

The notebook expects you to download three **public** datasets. This repository **does not** redistribute raw data; please download from the official sources below and follow the folder structure.

### 1. Brain Tumor MRI (Figshare)

- **Source:** Figshare – “brain tumor dataset” (3,064 T1-weighted CE MRI images with meningioma, glioma and pituitary tumors).
- **Download:**  
  [https://figshare.com/articles/dataset/brain_tumor_dataset/1512427](https://figshare.com/articles/dataset/brain_tumor_dataset/1512427)
- **Destination folder:**  
  `data/data_MRI/`

### 2. SSDD – SAR Ship Detection

- **Source:** SAR Ship Detection Dataset (SSDD), SAR imagery for maritime ship detection.
- **Download:**  
  [https://drive.google.com/file/d/1glNJUGotrbEyk43twwB9556AdngJsynZ/view?usp=sharing](https://drive.google.com/file/d/1glNJUGotrbEyk43twwB9556AdngJsynZ/view?usp=sharing)
- **Destination folder:**  
  `data/data_SSDD/`

### 3. ICEYE SAR

- **Source:** ICEYE open SAR datasets (e.g., Doha Airport imagery).
- **Download:**  
  [https://www.iceye.com/downloads/datasets](https://www.iceye.com/downloads/datasets)
- **Destination folder:**  
  `data/data_ICEYE/`

**Directory Structure (after download):**

```text
data/
├── data_SSDD/   # SSDD SAR ship detection images
├── data_MRI/    # Brain tumor MRI images
└── data_ICEYE/  # ICEYE SAR crops (e.g., Doha Airport)
```

The Colab notebook handles resizing to 32 X 32, grayscale conversion and entropy-block partitioning once the files are placed correctly.

---

##  Key Ideas and Contributions

- EBA-QR uses entropy-driven gate allocation, where local Shannon entropy highlights structurally important regions so gates are concentrated on meaningful content instead of being spent uniformly on all pixels.  

- This shifts the design focus from “qubits per pixel” toward “gates per unit of useful structure,” reducing the overall number of operations needed for sparse or moderately complex images.  

- A formal gate-cost model is used conceptually to show that the contribution of low-entropy background can be pushed very close to zero, while the cost in regions of interest mainly depends on how many high-entropy blocks there are rather than on the full image size.  

- A NEQR+Mask baseline is introduced that uses exactly the same entropy-based ROI mask as EBA-QR but keeps a standard NEQR circuit, which makes it possible to isolate how much of the improvement comes from the adaptive representation itself instead of from classical masking.  

- The method is implemented with realistic NISQ constraints in mind, using Qiskit-based simulations and small IBM Quantum backends to evaluate gate counts, depth and noise behavior.  

- Because the representation preserves NEQR-style structure in informative regions, it directly supports quantum geometric transformations such as horizontal and vertical flips and hybrid quantum–classical Sobel edge detection, making it a practical front-end for quantum CNNs and other quantum machine learning models.
---

##  Main Results (High-Level)

- Up to **78.1% gate-cost reduction** on sparse SSDD SAR scenes while preserving ROI fidelity.
- Up to **64.7% reduction** on Brain Tumor MRI with high diagnostic safety.
- Additional structural gains over **NEQR+Mask**, confirming that improvements come from the representation itself, not only from classical masking.
- Demonstrated execution of flips and Sobel edges on EBA-QR states within NISQ-friendly circuit depths.

All numeric values and tables (including ablations) are recomputed in the Colab notebook.

---

##  Local Use (Optional)

If you want to clone and run the notebook locally:

```bash
git clone [https://github.com/TanishaDebnath/EBA-QR.git](https://github.com/TanishaDebnath/EBA-QR.git)
cd EBA-QR

# (Optional) create a virtual environment
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate

pip install -r requirements.txt
```

Then open the `.ipynb` notebook in Jupyter or VS Code and run all cells.

---

##  Citation

If you use this repository or build upon EBA-QR, please cite:

```bibtex
@article{debnath2025eba,
  title   = {EBA-QR: An Entropy-Based Adaptive Quantum Image Representation Framework for Efficient Multi-Modal Image Processing},
  author  = {Debnath, Tanisha},
  journal = {arXiv preprint},
  year    = {2025},
  note    = {Under review}
}
```

---

##  License

This project is licensed under the **MIT License**. See `LICENSE` for details.

---

##  Contact

For questions, feedback or collaborations:

- **Author:** Tanisha Debnath  
- **Affiliation:** Institute of Engineering & Management, Kolkata  
- **Email:** [tanishabdebnath@gmail.com](mailto:tanishabdebnath@gmail.com)  
- **Project:** [https://github.com/TanishaDebnath/EBA-QR](https://github.com/TanishaDebnath/EBA-QR)

