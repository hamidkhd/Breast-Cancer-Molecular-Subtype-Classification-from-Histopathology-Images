# Breast Cancer Molecular Subtype Classification

This repository contains our work for the second Artificial Neural Networks and Deep Learning (AN2DL) challenge, completed in December 2025. The task was to classify low-magnification histopathology images into four breast cancer molecular subtypes: **Luminal A, Luminal B, HER2-positive, and Triple Negative**.

The main difficulty was the image quality. Many samples contained staining artifacts, large background regions, or very little useful tissue. We tried different ways of cleaning the images and selecting the parts that were most relevant for classification. The final approach uses tissue-focused patches rather than training directly on the full images.

## Approach

### Preprocessing

We used the provided tissue masks to identify regions of interest (ROI). The preprocessing notebook includes:

- Flagging unusual images using HSV color information and inspecting potential outliers.
- Reducing strong green and bright artifacts, and replacing background regions with a neutral gray color and a small amount of noise.
- Extracting **224 × 224** patches from tissue-rich areas using the ROI bounding box and candidate tile locations.
- Filtering out patches with too little tissue or very low visual variation.

We also experimented with other preprocessing options, including stain normalization and different masking strategies, but they did not improve the final test results.

### Models

We compared three patch-based models:

- **ResNet50:** a pretrained ResNet backbone with a classification head, trained on RGB patches.
- **ResNet101:** a deeper ResNet variant trained with the same patch-based setup.
- **Dual-Path EfficientNet-B2:** an EfficientNet-B2 encoder for RGB patches and a separate small CNN for the binary tissue masks. Their features are combined before classification.

Each model predicts at patch level. For validation and testing, we average the class probabilities across patches belonging to the same image to obtain one image-level prediction. We used **macro F1-score** to compare the models.

## Results

The following scores are taken from the final challenge report:

| Model | Validation F1 | Test F1 |
| --- | ---: | ---: |
| ResNet50 (without patching) | 39.37% | 35.85% |
| ResNet50 | 43.64% | 40.26% |
| ResNet101 | **48.29%** | 39.47% |
| Dual-Path EfficientNet-B2 | 41.20% | **40.65%** |

ResNet101 had the best validation score, while the dual-path EfficientNet-B2 performed slightly better on the test set. Overall, the results suggest that using tissue-focused patches was more useful than working with entire images, although the noisy data remained difficult to generalize to.

## Repository files

- [`preprocessing_code.ipynb`](preprocessing_code.ipynb) — image cleaning, ROI processing, and patch extraction.
- [`Models_1.ipynb`](Models_1.ipynb) — Dual-Path EfficientNet-B2 experiments.
- [`Models_2.ipynb`](Models_2.ipynb) — ResNet50 and ResNet101 experiments.
- [`AN2DL_2_report.pdf`](AN2DL_2_report.pdf) — full report, including the experimental setup and additional details.

## Running the notebooks

The code was developed in **Google Colab** using **PyTorch**. A GPU runtime is recommended. To run the notebooks, you will need the original challenge dataset, including the RGB images, tissue masks, and training labels.

Start with `preprocessing_code.ipynb` to prepare the patches, then use either model notebook for training or inference. Some cells use Google Drive or Colab-specific paths, so these need to be updated for your own environment. The dataset and trained model weights are not included in the repository.

This is a course project and has not been evaluated for clinical use.
