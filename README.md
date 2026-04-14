<!-- Banner -->
<div align="center">

# 🔩 Text-to-3D-CAD
### *Natural Language → Parametric 3D Models for Mechanical Parts*

<img src="https://img.shields.io/badge/LLaMA-3--8B-blueviolet?style=for-the-badge&logo=meta&logoColor=white"/>
<img src="https://img.shields.io/badge/QLoRA-Fine--Tuned-orange?style=for-the-badge"/>
<img src="https://img.shields.io/badge/CadQuery-3D%20CAD-blue?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Python-3.12-green?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/HuggingFace-Transformers-yellow?style=for-the-badge&logo=huggingface&logoColor=black"/>

<br/>

> **Type a sentence. Get a 3D model.**  
> No CAD expertise. No manual modeling. Just plain English.

---

</div>

## 🧠 What is this?

**Text-to-3D-CAD** is a BTP (Bachelor's Thesis Project) from the **Department of Mechanical & Industrial Engineering** that bridges the gap between *natural language* and *parametric CAD design*.

A fine-tuned **LLaMA-3-8B** model takes engineering requirements as plain English prompts, extracts structured design parameters, validates them against **ISO standards**, and auto-generates a **downloadable 3D STL model** — all in seconds.

```
"Design a bolt M10 grade 10.9 length 50mm"
                    ↓
         LLM (Fine-tuned LLaMA-3)
                    ↓
     { "size": "M10", "pitch_mm": 1.5, ... }
                    ↓
        ISO Standard Validation
                    ↓
        CadQuery → bolt.stl  🔩
```

---

## ⚙️ Supported Parts

| Part | Input Example | Output |
|------|--------------|--------|
| 🔩 **Bolt** | `"Design a bolt M12 grade 10.9 length 60mm"` | Hex-head bolt STL |
| 🔧 **Nut** | `"Design a nut for M10 bolt"` | Hex nut with bore STL |
| ⚙️ **Spur Gear** | `"Design a gear with 24 teeth, module 2, thickness 12mm, bore 10mm"` | Full gear STL |

---

## 🗄️ Dataset

Synthetically generated **(Engineering Requirements → CAD Parameters)** input-label pairs for training the LLM.

**Input parameters include:**
- Power / Load (P)
- Speed N (100–3000 rpm)
- Torque T (10–5000 Nm)
- Safety Factor (1.0–3.0)
- Material (steel, alloy steel, cast iron)
- Geometric constraints & desired fatigue life

**Output labels include:**
- ISO bolt size, thread pitch, stress area
- Gear module, number of teeth, pitch diameter
- Nut thickness, width across flats

---

## 🚀 Pipeline Architecture

```
┌─────────────────────────────────────────────────────┐
│                   USER PROMPT                        │
│     "Design a bolt M10 grade 10.9 length 50mm"      │
└──────────────────────┬──────────────────────────────┘
                       │
              ┌────────▼────────┐
              │  Part Detector  │  bolt / nut / gear
              └────────┬────────┘
                       │
              ┌────────▼────────┐
              │  LLM Inference  │  Fine-tuned LLaMA-3-8B (QLoRA)
              └────────┬────────┘
                       │
            ┌──────────▼──────────┐
            │  JSON Extraction    │  Multi-strategy parser
            │  + ISO Validation   │  Fallback: regex parser
            └──────────┬──────────┘
                       │
              ┌────────▼────────┐
              │ CadQuery Engine │  Parametric 3D modeling
              └────────┬────────┘
                       │
              ┌────────▼────────┐
              │   STL Output    │  + Plotly 3D viewer
              └─────────────────┘
```

---

## 🏋️ Model Details

| Parameter | Value |
|-----------|-------|
| Base Model | `meta-llama/Meta-Llama-3-8B-Instruct` |
| Adapter | `Black4cosmos/Llama-3-Engineering-Phi-Finetune-QLoRA` |
| Fine-tuning Method | QLoRA (4-bit quantization) |
| Framework | Unsloth + HuggingFace PEFT |
| Hardware | NVIDIA Tesla T4 (14.5 GB) |
| Training Steps | 300 |
| Batch Size | 10 |
| Learning Rate | 2e-4 (cosine decay) |
| Optimizer | AdamW 8-bit |

---

## 📁 Repository Structure

```
text-to-3D-CAD/
│
├── 📓 BTP_ETE.ipynb          # Main pipeline notebook
├── 🗄️ dataset/               # Engineering design dataset
│   ├── bolts_dataset.csv
│   ├── nuts_dataset.csv
│   └── gears_dataset.csv
└── 📄 README.md
```

---

## 🛠️ How to Run

### 1. Install dependencies
```bash
pip install unsloth transformers accelerate bitsandbytes peft cadquery
pip install numpy-stl plotly huggingface_hub
```

### 2. Login to HuggingFace
```python
from huggingface_hub import login
login("your_hf_token")
```

### 3. Run the notebook
Open `BTP_ETE.ipynb` in Google Colab (T4 GPU recommended) and run all cells.

### 4. Generate your first part
```python
generate_part("Design a bolt M10 grade 10.9 length 50mm")
generate_part("Design a nut for M12 bolt")
generate_part("Design a spur gear with 24 teeth, module 2, thickness 12mm, bore 10mm")
```

---

## 📐 Key Formulae

| Formula | Purpose |
|---------|---------|
| `Pitch Ø = d - 0.6495 × pitch` | Thread geometry |
| `Minor Ø = d - 1.2269 × pitch` | Thread root diameter |
| `Thread Height = 0.866 × pitch` | Thread profile |
| `Pitch Radius = (module × teeth) / 2` | Gear geometry |
| `Addendum = module` | Gear tooth tip |
| `Dedendum = 1.25 × module` | Gear tooth root |

---

## 👨‍💻 Team

| Name | Roll No |
|------|---------|
| Kaustubh Dwivedi | 22117066 |
| Aman Kumar | 22117019 |
| Akshay Kumar | 22117015 |
| Samay Jain | 22117124 |
| Priyanshu | 22117110 |

**Supervised by:** Prof. Anuj Bisht  
**Department of Mechanical & Industrial Engineering**

---

## 📜 License

This project is academic research — feel free to use, cite, and build upon it.

---

<div align="center">

**⭐ Star this repo if you found it useful!**

*Made with 🔩 and a lot of debugging*

</div>
