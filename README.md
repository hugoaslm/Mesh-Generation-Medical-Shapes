# Mesh Generation for Medical Shapes

**IG.2413 — Deep Learning (2024/2025)**
Master's Project (Engineering cycle, ISEP Paris) — Computer Vision.
Goal: paper implementation of "LLaMA-Mesh: Unifying 3D Mesh Generation with Language Models, Zhengyi Wang et al., 2024". Fine-tuned the model to generate medical shapes.

## Summary
This project explores **3D mesh generation for medical objects** with a focus on producing printable meshes (OBJ) with a constrained face budget (≤500 faces). We investigated data preparation, automatic labeling, and lightweight fine‑tuning strategies to make experimentation feasible on modest GPUs. fileciteturn2file0

## Data
- **Source:** MedShapeNet (medical 3D shapes).  
- **Format:** `.obj` meshes; need on the order of **≥500 samples**, with **≤500 faces** per mesh for efficiency. fileciteturn2file0
- **Target objects evaluated:** femur retractors, skulls, balloons, etc., and selection of one object family for training. fileciteturn2file0

## Method
1. **Initial data exploration:** a 2D **ResNet** baseline on representative images derived from meshes (not conclusive). fileciteturn2file0  
2. **Auto‑labeling:** **Gemma 3** inference **quantized to 4‑bit** to tag each item and improve dataset curation. fileciteturn2file0  
3. **Fine‑tuning recipe:**  
   - **4‑bit loading (k‑bit)** to reduce VRAM while computing in float16. fileciteturn2file0  
   - **LoRA** with **r=32, α=16** (~**0.2%** trainable params); recast sensitive layers to float32 for stability. fileciteturn2file0  
   - Inject LoRA on **Q/K/V/O projections** and **MLP (gate/up/down)** only. fileciteturn2file0  
   - **Chat‑template conversion** of prompt/assistant pairs to **Llama‑3.1** format via **unsloth**. fileciteturn2file0  
   - **Training:** **100 steps**, **LR 2e‑4**, **gradient checkpointing**; fits on a **T4**; produces an **adapter ≈320 MB** (mergeable). fileciteturn2file0
