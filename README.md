# Prediction of Metastasis in Breast Cancer Using Deep Learning

> Patch-level lymph node metastasis detection on the PatchCamelyon (PCam) dataset using modern convolutional architectures.

![alt text](2_curves_highlighted.png)

## Overview

This project explores automated detection of breast cancer metastasis from histopathology image patches. The work compares three attention-enhanced CNN baselines—**ResNet-50 + CBAM**, **DenseNet-121 + CBAM**, and **EfficientNet-B3 + CBAM**—against a modern convolutional backbone, **ConvNeXt V2**.

The goal is not only to improve classification performance, but also to reduce **false negatives**, which are especially critical in medical screening. According to the paper, the proposed ConvNeXt V2 model achieved the best overall performance on the PCam test split with **AUC = 0.9710**, **F1 = 0.9116**, **accuracy = 0.9127**, and approximately **47% fewer false negatives** compared to the strongest baseline.

## Why this project matters

Breast cancer metastasis detection is a high-impact medical imaging task. Missing metastatic tissue can delay diagnosis and treatment, which makes **sensitivity** and **false-negative reduction** just as important as accuracy. This project demonstrates that modern convolutional design can outperform explicit attention mechanisms for this problem.

## Key Contributions

* Benchmarks strong CNN baselines with **CBAM** attention.
* Uses **ConvNeXt V2** as a modern fully convolutional backbone.
* Applies **Grad-CAM** for interpretability and visual validation.
* Focuses on **AUC, sensitivity, and false-negative reduction** rather than accuracy alone.
* Shows that ConvNeXt V2 better highlights malignant cell clusters with less background noise.

## Dataset

The project uses the **PatchCamelyon (PCam)** dataset:

* **327,680** histopathology patches
* Patch size: **96 × 96** pixels
* Source: H&E-stained lymph node whole-slide images
* Binary labels: metastasis present / not present
* Official train/validation/test split used for fair comparison

## Methodology

ConvNeXt V2 with custom head:

![alt text](<conv2 .drawio.png>)

![alt text](<convnext v2.jpg>)

ResNet-50 + CBAM:

![alt text](<resnet50 with cbam.drawio.png>)

### 1. Preprocessing

* ImageNet mean/std normalization
* Data augmentation to improve generalization:

  * Horizontal and vertical flips
  * Random rotations
  * Affine transforms
  * Color jitter / color variations
  * MixUp
  * CutMix

### 2. Baseline Models

The following models were enhanced with **CBAM**:

* ResNet-50 + CBAM
* DenseNet-121 + CBAM
* EfficientNet-B3 + CBAM

CBAM applies:

* **Channel attention** to emphasize informative feature channels
* **Spatial attention** to localize important regions

### 3. Proposed Model

The proposed approach uses **ConvNeXt V2**:

* Large-kernel depthwise convolutions
* Pointwise channel mixing
* Global Response Normalization (GRN)
* Custom classification head for binary prediction

This architecture helps capture both local nuclear morphology and broader tissue context without requiring explicit attention modules.

### 4. Training Strategy

* ImageNet-pretrained weights
* Fine-tuning on the PCam dataset
* AdamW optimizer
* OneCycle learning rate schedule
* Focal Loss to address hard examples and false negatives
* Mixed-precision training for speed and stability

### 5. Evaluation

Performance was measured using:

* Accuracy
* Sensitivity
* Specificity
* F1-score
* AUC (ROC-AUC)

Grad-CAM was used to inspect what regions the models focused on during prediction.

## Results

### Test Set Performance

| Model                      |   Accuracy | Sensitivity | Specificity |   F1-score |        AUC |
| -------------------------- | ---------: | ----------: | ----------: | ---------: | ---------: |
| ResNet-50 + CBAM           |     0.8871 |      0.8116 |      0.9629 |     0.8779 |     0.9605 |
| DenseNet-121 + CBAM        |     0.9009 |      0.8950 |      0.9074 |     0.9001 |     0.9661 |
| EfficientNet-B3 + CBAM     |     0.8792 |      0.7925 |      0.9662 |     0.8678 |     0.9646 |
| **ConvNeXt V2 (Proposed)** | **0.9127** |  **0.9008** |  **0.9245** | **0.9116** | **0.9710** |

### Main Findings

* **Best AUC:** ConvNeXt V2 (0.9710)
* **Best F1-score:** ConvNeXt V2 (0.9116)
* **Best sensitivity:** ConvNeXt V2 (0.9008)
* **False-negative reduction:** about **47%** compared with ResNet-50 + CBAM

### Interpretation

Grad-CAM visualizations show that ConvNeXt V2 produces more compact, nucleus-centered activations, while attention-based baselines often spread attention into background or stromal areas. This aligns with the quantitative improvement in false-negative reduction.



![alt text](<grad cam.png>)
![alt text](gradcamm.png)
## Repository Structure

```text
.
├── data/                 # Dataset or data-loading scripts
├── models/               # CNN / ConvNeXt V2 model definitions
├── training/             # Training loops and loss functions
├── utils/                # Metrics, augmentation, helper functions
├── notebooks/            # Experiments and analysis
├── figures/              # ROC, PR curves, Grad-CAM images
├── results/              # Saved checkpoints and evaluation outputs
└── README.md
```

## Visualizations

The paper includes:

* ROC curves
* Precision-Recall curves
* Confusion matrix
* Grad-CAM heatmaps

These are useful for validating both performance and interpretability.

## Limitations

* The study is focused on **patch-level** classification, not full whole-slide image analysis.
* Generalization across hospitals, scanners, and staining variations still needs broader validation.
* Further work may benefit from testing on independent datasets.

## Future Work

Possible next steps include:

* Whole-slide image (WSI) evaluation
* Multi-instance learning pipelines
* External clinical dataset validation
* Lightweight hybrid models combining convolution and attention
* Uncertainty-aware prediction for safer clinical deployment

## Citation / Paper Summary

This repository is based on the study **“Prediction of Metastasis in Breast Cancer Using Deep Learning”**. The paper reports that ConvNeXt V2 with Custom head outperformed CBAM-augmented ResNet-50, DenseNet-121, and EfficientNet-B3 on the PCam dataset, achieving the highest AUC and substantially reducing false negatives.

## License

Add the license that matches your repository policy.

