# 🍽️ InstaChef — Recipe Generation from Food Image

> Upload a food photo. Get the full recipe. Instantly.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-green?style=for-the-badge)](https://oisheebanerjee08-rgboisheebanerjee.github.io/instachef)
[![Backend](https://img.shields.io/badge/Backend-Hugging%20Face-yellow?style=for-the-badge)](https://huggingface.co/spaces/oisheebanerjee08/instachef-backend)
[![Python](https://img.shields.io/badge/Python-3.10-blue?style=for-the-badge)](https://python.org)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.6-red?style=for-the-badge)](https://pytorch.org)

---

## 📌 About

**InstaChef** is a final year B.Tech project that uses deep learning to generate complete cooking recipes from food photographs. A user uploads any food image, and the system identifies the dish and returns:

- **Recipe title** — the name of the identified dish
- **Ingredients** — a complete list of required ingredients
- **Instructions** — step-by-step cooking directions

---

## 🧠 Model Architecture

The system is based on the **Inverse Cooking** architecture proposed by Facebook AI Research (CVPR 2019).

```
Food Image
    ↓
ResNet-50 CNN Encoder       ← extracts visual features from the image
    ↓
Transformer Decoder         ← multi-head self-attention based sequence decoder
    ↓
Ingredient Prediction       ← predicts ingredient tokens
    ↓
Recipe Generation           ← generates step-by-step instructions
    ↓
Full Recipe Output
```

### Key Details
| Component | Detail |
|-----------|--------|
| CNN Encoder | ResNet-50 pretrained on ImageNet |
| Decoder | Transformer with multi-head self-attention |
| Dataset | Recipe1M (1M+ recipes + food images) |
| Fine-tuning | Kaggle GPU · Adam optimizer · lr=1e-4 · 5–10 epochs |
| Loss Function | Cross-entropy (ingredients + instructions) |
| Top-5 Accuracy | ~85% on Food-101 categories |

---

## 🗂️ Repository Structure

```
instachef/
├── index.html                     # Frontend — 3-page web app
├── README.md                      # This file
├── backend/
│   ├── app.py                     # Gradio app — deployed on Hugging Face
│   ├── requirements.txt           # Python dependencies
│   └── Foodimg2Ing/
│       ├── output.py              # Inference pipeline (patched for PyTorch 2.6)
│       ├── model.py               # Model definition
│       ├── args.py                # Argument parser
│       ├── modules/
│       │   ├── multihead_attention.py   # Fixed bool mask compatibility
│       │   ├── transformer_decoder.py
│       │   ├── encoder.py
│       │   └── utils.py
│       └── utils/
│           └── output_utils.py    # Output formatting
└── presentation/
    └── InstaChef_Presentation.pptx
```

---

## 🚀 Live Demo

🌐 **Frontend:** https://oisheebanerjee08-rgboisheebanerjee.github.io/instachef

☁️ **Backend API:** https://huggingface.co/spaces/oisheebanerjee08/instachef-backend

---

## ⚙️ Running Locally

### Prerequisites
- Python 3.10
- PyTorch 2.6+

### Setup

```bash
# Clone the repo
git clone https://github.com/oisheebanerjee08-rgboisheebanerjee/instachef.git
cd instachef/backend

# Install dependencies
pip install -r requirements.txt

# Download model files
wget -P Foodimg2Ing/data/ https://dl.fbaipublicfiles.com/inversecooking/modelbest.ckpt
wget -P Foodimg2Ing/data/ https://dl.fbaipublicfiles.com/inversecooking/ingr_vocab.pkl
wget -P Foodimg2Ing/data/ https://dl.fbaipublicfiles.com/inversecooking/instr_vocab.pkl

# Run the app
python app.py
```

Open `http://localhost:7860` in your browser.

---

## 🔧 Compatibility Fixes Applied

This project required several patches to run on modern PyTorch 2.6+:

| Issue | Fix |
|-------|-----|
| `torch.load` default changed in PyTorch 2.6 | Added `weights_only=False` flag |
| Byte mask in attention module | Converted to `.bool()` in `multihead_attention.py` |
| TensorFlow dependency for image loading | Replaced with `PIL.Image.open()` |
| Hugging Face OOM on free tier | Moved model loading to startup, not per-request |

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Deep Learning | PyTorch, ResNet-50, Transformer Decoder |
| Model Serving | Gradio |
| Backend Hosting | Hugging Face Spaces |
| Frontend | HTML, CSS, JavaScript |
| Frontend Hosting | GitHub Pages |

---

## 📚 Reference

> Amaia Salvador et al. **"Inverse Cooking: Recipe Generation from Food Images"**
> CVPR 2019 · Facebook AI Research

---

## 👥 Team

Oishee Banerjee · Deepraj Mukherjee · Anant Thakur · Dipanjan Roy

**Sanaka Educational Trust's Group of Institutions**
B.Tech — Computer Science & Engineering · 2025–26
