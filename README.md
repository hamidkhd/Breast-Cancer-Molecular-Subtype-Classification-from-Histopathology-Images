# Breast Cancer Molecular Subtype Classification

Code and report for the second AN2DL challenge (December 2025).

We worked on classifying low-magnification breast histopathology images into four molecular subtypes: Luminal A, Luminal B, HER2-positive, and Triple Negative.

The images had quite a bit of background noise and staining artifacts, so we tried several preprocessing approaches before training. Our final pipeline uses tissue masks to select informative 224 × 224 patches. We trained ResNet50, ResNet101, and a dual-path EfficientNet-B2 that takes both the RGB patches and their masks. Image-level predictions are obtained by averaging the probabilities across patches.

## Results

F1 scores reported in our challenge report:

| Model | Validation F1 | Test F1 |
| --- | ---: | ---: |
| ResNet50 (no patching) | 39.37% | 35.85% |
| ResNet50 | 43.64% | 40.26% |
| ResNet101 | 48.29% | 39.47% |
| Dual-Path EfficientNet-B2 | 41.20% | **40.65%** |

## Files

- [`preprocessing_code.ipynb`](preprocessing_code.ipynb) — image cleaning and patch extraction
- [`Models_1.ipynb`](Models_1.ipynb) — Dual-Path EfficientNet-B2
- [`Models_2.ipynb`](Models_2.ipynb) — ResNet50 and ResNet101 experiments
- [`AN2DL_2_report.pdf`](AN2DL_2_report.pdf) — full project report

The notebooks were developed in Google Colab. The challenge dataset isn't included, and the data paths need to be adjusted before running them.

## Team

As listed in the project report submitted on December 16, 2025:

- Amirali Askari
- Hamidreza Khodadadi
- Mohammad Rashid Khan Mofrad
- Zahra Nazar Zadeh Attar
