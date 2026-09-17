# 🌿 Plant Disease Detection from Leaf Photos

A lightweight AI model that identifies plant diseases from a single leaf photo.
This is the first prototype of my agritech idea: helping farmers in Uzbekistan
detect crop diseases early.

## Results

| Metric | Value |
|---|---|
| Classes | 38 (14 crops, healthy + diseased) |
| Test images | 5,440 (never seen during training) |
| **Test accuracy** | **94.7%** |
| Model size for phones (TFLite) | **2.6 MB** |

## How it works

- **Data:** PlantVillage dataset (54,000+ leaf images), GitHub copy by spMohanty
- **Model:** MobileNetV2 pre-trained on ImageNet, adapted with **transfer learning**
- **Training:** 5 epochs with a frozen base, then 3 epochs of fine-tuning the last 30 layers
- **Regularisation:** data augmentation (flip, rotation, zoom) and dropout
- **Export:** converted to TensorFlow Lite for mobile use

## What I learned

1. **Dangerous errors matter more than overall accuracy.** For `Tomato___healthy`,
   recall was 1.00 but precision was 0.84 — some diseased leaves were labelled
   healthy. For a farmer, a missed disease is the most costly mistake.
2. **Lab data does not equal field data.** On a real outdoor photo of a walnut leaf
   (a crop not in the dataset), the model answered "Apple scab" with only 44.7%
   confidence. It cannot say "I don't know".
3. **Training layers are not deployment layers.** The augmentation layers blocked
   TFLite conversion, so I rebuilt an inference-only model and verified that its
   outputs matched the original.

## Limitations & next steps

- PlantVillage images are taken on plain backgrounds; accuracy drops in real fields
- No cotton or wheat — key crops for Uzbekistan
- Next: collect local field photos with agronomists, add a confidence threshold
  ("unsure — consult an agronomist"), and connect the model to a Telegram bot

## Files

- `plant_disease_detection.ipynb` — full training notebook (Google Colab)
- `plant_disease_model.tflite` — mobile-ready model
- `class_names.txt` — the 38 class labels, in model output order

---
*Author: Islomjon Agzamov*
