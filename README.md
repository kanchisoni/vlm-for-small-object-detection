# VLM Small-Object Anti-Drone Detection

Internship Report: **Vision-Language Models for Small Object Detection, Classification,
Localisation and Explainability — A Case Study in Anti-Drone Surveillance**

Author: Kanchi Soni | Supervisor: Dr. Akshay Agrawal
IISER Bhopal, Department of Data Science and Engineering

---

## Datasets

- NPS UAV Tracking Dataset: https://www.kaggle.com/datasets/kanchisoni/nps-dataset
- Drone Detection Dataset (DUT Anti-UAV): https://www.kaggle.com/datasets/kanchisoni/drone-detection-dataset
- Drone Tracking Dataset: https://www.kaggle.com/datasets/kanchisoni/drone-tracking

---

## Overview

This project evaluates whether general-purpose **Vision-Language Models (VLMs)** can
reliably detect, localise, and explain small, distant UAVs in anti-drone surveillance
imagery, benchmarked against a fine-tuned **YOLOv11** detector and prior transformer-based
drone-detection work such as **TransVisDrone**.

---

## Pipeline

```
NPS UAV Tracking Dataset
        │
        ▼
Dataset Understanding & Annotation Conversion
        │
        ▼
DUT Anti-UAV Detection Dataset
        │
        ▼
Image Selection & Ground Truth Preparation
        │
        ▼
Zero-Shot Evaluation of Vision-Language Models
        │
        ▼
YOLOv11 Fine-Tuning (P2 Head + SAHI)
        │
        ▼
Performance Evaluation (IoU, Precision, Recall, F1, mAP)
        │
        ▼
Explainability Analysis (ChatGPT, Gemini, Kimi)
        │
        ▼
Comparison and Final Analysis
```

---

## Models Evaluated

- **Open-source VLMs (zero-shot):** Qwen2.5-VL-7B, Qwen3-VL, Gemma3-4B, LLaVA-7B, Moondream
- **Detector:** YOLOv11, fine-tuned with a P2 detection head + SAHI tiled inference
- **Commercial assistants (explainability):** Gemini, ChatGPT, Kimi

---

## Key Results

| System | Precision | Recall | F1 | mAP@0.5 |
|---|---|---|---|---|
| Qwen2.5-VL-7B (zero-shot) | 0.744 | 0.580 | 0.652 | 0.545 |
| Other VLMs (zero-shot) | — | — | — | ~0.00 |
| **YOLOv11 (fine-tuned)** | high | high | — | **0.969** |

- Only Qwen2.5-VL-7B gave usable zero-shot localisation; other VLMs failed to detect UAVs reliably.
- Fine-tuned YOLOv11 far outperformed all VLMs, and is competitive with published
  transformer-based drone detectors such as TransVisDrone (AP@0.5IoU = 0.95 on NPS).
- Gemini gave the most verifiable explanations (structured JSON boxes); ChatGPT and Kimi gave
  useful but less spatially grounded reasoning.
- Detection accuracy drops sharply once a UAV occupies less than ~0.05% of image area —
  object size matters far more than camera motion.

---

## Related Code

- `Qwen25VL_Drone_Detection_Eval.ipynb` — Zero-shot / few-shot / chain-of-thought evaluation
  of Qwen2.5-VL-7B against ground-truth annotations.

---

## References

- **TransVisDrone** — Sangam, T., Dave, I. R., Sultani, W., Shah, M. (2023).
  *TransVisDrone: Spatio-Temporal Transformer for Vision-based Drone-to-Drone Detection in
  Aerial Videos.* ICRA 2023.
  Paper: https://arxiv.org/abs/2210.08423
  Code: https://github.com/tusharsangam/TransVisDrone

- **YOLOv5-TPH** — https://github.com/cv516Buaa/tph-yolov5

- **Ultralytics YOLOv5 / YOLOv11** — https://github.com/ultralytics/ultralytics

- **Swin Transformer** — https://github.com/microsoft/Swin-Transformer

---

## Citation

If referencing this work, please cite:

> Soni, K. (2026). *Vision-Language Models for Small Object Detection, Classification,
> Localisation and Explainability: A Case Study in Anti-Drone Surveillance.* Internship
> Report, Department of Data Science and Engineering, IISER Bhopal. Supervisor: Dr. Akshay
> Agrawal.
