# PCB Defect VLM — Custom Vision-Language Model for Quality Inspection

> 🚧 **Upcoming Project** — Design document complete, implementation planned

## Overview

A custom Vision-Language Model (VLM) for PCB defect detection that allows natural language queries like *"Where's the short circuit?"* and returns precise coordinates with confidence scores.

## Key Concepts

- **Base Model**: Qwen3-VL-8B with spatial attention modifications
- **Target Inference**: <2 seconds (offline deployment)
- **Hallucination Mitigation**: Visual Contrastive Decoding (VCD) + DPO training
- **Localization Accuracy**: ±15-20 pixel error
- **Training Data**: 50K PCB images with defect bounding boxes → synthetic QA pairs

## Proposed Architecture

```
┌─────────────────────────────────────────────────────┐
│                   Qwen3-VL-8B                      │
│  ┌─────────────┐    ┌─────────────────────────┐    │
│  │   Vision    │───▶│   Spatial Attention     │    │
│  │   Encoder   │    │   + Coordinate Branch   │    │
│  └─────────────┘    └─────────────────────────┘    │
│         │                      │                    │
│         ▼                      ▼                    │
│  ┌─────────────┐    ┌─────────────────────────┐    │
│  │  Language   │    │   Calibrated Confidence │    │
│  │   Decoder   │    │   + Grammar Constraints │    │
│  └─────────────┘    └─────────────────────────┘    │
└─────────────────────────────────────────────────────┘
```

## Expected Output Format

```json
{
  "defect_type": "Short_Circuit",
  "center": [450, 200],
  "confidence": 0.92,
  "severity": "High"
}
```

## Optimization Strategy

| Technique | Time Saved | Accuracy Impact |
|-----------|------------|-----------------|
| INT8 Quantization | -0.3s | <0.5% loss |
| Token Pruning (67%) | -0.25s | ~1% loss |
| FP16 KV Cache | -0.1s | None |

**Target: 2.0s → 1.3s**

## Training Pipeline

1. **Synthetic QA Generation** — 2 hours
2. **Supervised Fine-tuning (SFT)** — 24 hours
3. **Preference Optimization (DPO)** — 12 hours
4. **Instruction Tuning** — 4 hours

**Total: ~42 hours on single L40S GPU**

## Full Design Document

📄 **[proposal.pdf](proposal.pdf)** — Complete technical proposal with architecture details, training strategy, and validation metrics.

## Status

- [x] Design document complete
- [ ] Implementation (coming soon)
- [ ] Training
- [ ] Evaluation
- [ ] Deployment

## License

MIT
