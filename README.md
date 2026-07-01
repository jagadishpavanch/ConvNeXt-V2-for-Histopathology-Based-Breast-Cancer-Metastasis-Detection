# Breast Cancer Metastasis Detection using ConvNeXt V2 and Custom MLP Head

> Patch-level lymph node metastasis detection on the PatchCamelyon (PCam) dataset using modern convolutional architectures.

<img width="5400" height="2400" alt="2_curves_highlighted" src="https://github.com/user-attachments/assets/9cd00818-f725-4682-881e-76eb560c5fbf" />


## Overview

Breast cancer is one of the leading causes of cancer-related mortality worldwide, making early and accurate detection of lymph node metastasis critical for effective treatment planning. This project presents a deep learning framework for automated metastasis detection from histopathology image patches using the PatchCamelyon (PCam) dataset.

The proposed approach employs a **ConvNeXt V2 backbone** combined with a **custom multi-layer classification head** designed for binary metastasis classification. To evaluate its effectiveness, the model was benchmarked against three strong attention-enhanced CNN baselines: **ResNet-50 + CBAM**, **DenseNet-121 + CBAM**, and **EfficientNet-B3 + CBAM**.

The primary objective was to maximize diagnostic performance while minimizing **false-negative predictions**, a critical requirement in medical screening systems where missed cancer cases can have serious clinical consequences.


Additionally, Grad-CAM visualizations demonstrated that the model focuses more effectively on malignant cellular regions while suppressing irrelevant background tissue, improving both performance and interpretability.

This work demonstrates that modern convolutional architectures with care

## Why this project matters

Breast cancer metastasis detection is a high-impact medical imaging task. Missing metastatic tissue can delay diagnosis and treatment, which makes **sensitivity** and **false-negative reduction** just as important as accuracy. This project demonstrates that modern convolutional design can outperform explicit attention mechanisms for this problem.

## Key Contributions

## Proposed Architecture

The proposed framework uses ConvNeXt V2 as a feature extractor and a custom-designed classification head for binary lymph node metastasis detection.

### ConvNeXt V2 Backbone

The backbone leverages:

- Large-kernel depthwise convolutions
- Global Response Normalization (GRN)
- Modern convolutional design principles
- Strong feature representation learning

### Custom Classification Head

Instead of using the default ConvNeXt V2 classifier, a custom Multi-Layer Perceptron (MLP) head was designed:

Input Features (1024)
        ↓
Global Average Pooling
        ↓
Linear (1024 → 512)
        ↓
LayerNorm
        ↓
Dropout (p=0.3)
        ↓
Linear (512 → 256)
        ↓
GELU Activation
        ↓
Dropout (p=0.5)
        ↓
Linear (256 → 1)
        ↓
Sigmoid


## Key Contributions

- Developed a ConvNeXt V2 based metastasis detection framework.
- Designed a custom MLP classification head for binary classification.
- Benchmarked against ResNet-50 + CBAM, DenseNet-121 + CBAM, and EfficientNet-B3 + CBAM.
- Achieved 0.9710 ROC-AUC on the PCam dataset.
- Reduced false negatives by approximately 47%.
- Applied Grad-CAM for model interpretability.

## Dataset

The project uses the **PatchCamelyon (PCam)** dataset:

* **327,680** histopathology patches
* Patch size: **96 × 96** pixels
* Source: H&E-stained lymph node whole-slide images
* Binary labels: metastasis present / not present
* Official train/validation/test split used for fair comparison

## Methodology

ConvNeXt V2 with custom head:

<img width="759" height="481" alt="conv2  drawio" src="https://github.com/user-attachments/assets/f2909a80-c30b-4184-959f-b698febb973b" />

<img width="283" height="620" alt="convnext v2" src="https://github.com/user-attachments/assets/c7a8e81a-65d3-4a18-9d40-25cd2ea02e01" />


ResNet-50 + CBAM:

<img width="729" height="491" alt="resnet50 with cbam drawio" src="https://github.com/user-attachments/assets/3b35fc9e-dbc4-4b8e-81a1-1f295ea8d3de" />


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
<img width="1800" height="1500" alt="CM_ConvNeXt-V2" src="https://github.com/user-attachments/assets/55a4dad7-b7f2-4369-bd12-e3156464fd98" />


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



<img width="859" height="392" alt="image" src="https://github.com/user-attachments/assets/17ed4d92-0493-4c02-a143-eaa218daef0d" />



### Interpretation

Grad-CAM visualizations show that ConvNeXt V2 produces more compact, nucleus-centered activations, while attention-based baselines often spread attention into background or stromal areas. This aligns with the quantitative improvement in false-negative reduction.


<img width="1638" height="323" alt="gradcamm" src="https://github.com/user-attachments/assets/1a5751c5-609e-4a0f-b7e4-e3656373938b" />





## Repository Structure

```text
.
├── README.md
├── requirements.txt
├── .gitignore
│
├── comparison/
│   ├── 2_curves_highlighted (1).png
│   └── comparision alll.ipynb
│
├── convnext_v2_baseline/
│   ├── convnextv2.ipynb
│   └── results/
│       ├── confusion_matrix.png
│       ├── pr_curve.png
│       ├── roc_curve.png
│       └── training_loss.png
│
├── convnext_v2_proposed/
│   ├── convnextv2.ipynb
│   └── results/
│
├── densenet121_attention/
│   ├── densenet121.ipynb
│   ├── optimsethresold.ipynb
│   └── results/
│
├── efficientnet_b3_attention/
│   ├── efficientnetb3.ipynb
│   └── results/
│
└── resnet50_attention/
    ├── resnet50 with cbam.ipynb
    ├── test_predictions.csv
    └── results/ '''
  
```


## Visualizations

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

## Citation 

This repository is based on the study **“Prediction of Metastasis in Breast Cancer Using Deep Learning”**. The paper reports that ConvNeXt V2 with Custom head outperformed CBAM-augmented ResNet-50, DenseNet-121, and EfficientNet-B3 on the PCam dataset, achieving the highest AUC and substantially reducing false negatives.

📚 Bibliography & References
For a complete review of the methodology and cited literature, please refer to the full study documentation.

J. Kim et al., "Global patterns and trends in breast cancer incidence and mortality across 185 countries," Nature Medicine, 2025.

M. S. Reza and J. Ma, "Imbalanced histopathological breast cancer image classification with convolutional neural network," Proc. 14th IEEE Int. Conf. Signal Processing, 2018.

B. S. Veeling et al., "Rotation equivariant CNNs for digital pathology," Proc. MICCAI, 2018.

G. Huang et al., "Densely connected convolutional networks," Proc. CVPR, 2017.

Y. Celik et al., "Automated invasive ductal carcinoma detection based on deep transfer learning with whole-slide images," Pattern Recognition Letters, 2020.

S. Woo et al., "ConvNeXt V2: Co-designing and scaling convnets with masked autoencoders," Proc. CVPR, 2023.

S. Woo et al., "CBAM: Convolutional block attention module," Proc. ECCV, 2018.

I. Kandel and M. Castelli, "A novel architecture to classify histopathology images using convolutional neural networks," Applied Sciences, 2020.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
