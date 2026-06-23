# InfantPose: Fine-Tuning ViTPose++ Base for Infant Keypoint Estimation

## Overview

This project focuses on adapting ViTPose++ Base for infant pose estimation through transfer learning on a custom dataset of 2,000 annotated infant images.

While ViTPose++ is pretrained on the COCO human pose dataset, infant body proportions, pose distributions, and occlusion patterns differ significantly from adults. This work fine-tunes the model to accurately localize infant-specific anatomical keypoints, under IISC Bangalore.

## Dataset

* Images: 2,000
* Annotation Format: YOLO Pose
* Target Keypoints: 14
* Domain: Infant Pose Estimation

The dataset is converted into COCO keypoint format for compatibility with the MMPose training framework.

## Model

### Backbone

* ViTPose++ Base
* Pretrained on COCO Human Pose Dataset

### Adaptation

The original COCO prediction head is replaced to predict 14 infant keypoints instead of the standard 17 COCO keypoints.

## Training Strategy

### Stage 1: Head Training

The ViTPose++ backbone is frozen while training only the newly initialized keypoint prediction head.

Objective:

* Learn infant-specific keypoint definitions
* Stabilize training

### Stage 2: Full Fine-Tuning

The entire network is unfrozen and trained end-to-end.

Objective:

* Adapt feature representations to infant body structure
* Improve localization accuracy on challenging poses and occlusions

## Data Augmentation

The following augmentations are applied during training:

* Random Horizontal Flip
* Random Rotation
* Random Scaling
* Brightness Adjustment
* Contrast Adjustment
* Synthetic Occlusion

These augmentations improve generalization across diverse infant poses and imaging conditions.

## Training Pipeline

Dataset
→ COCO Conversion
→ ViTPose++ Base Checkpoint
→ Replace Prediction Head (17 → 14)
→ Head-Only Training
→ Full Fine-Tuning
→ Validation
→ Best Checkpoint Selection

## Evaluation Metrics

Model performance is evaluated using:

* Average Precision (AP)
* Percentage of Correct Keypoints (PCK)
* Validation Loss

Special attention is given to:

* Wrist localization
* Ankle localization
* Occluded joints
* Extreme infant poses

## Expected Outcome

The fine-tuned ViTPose++ model learns infant-specific body geometry while leveraging the strong human pose priors learned from COCO pretraining.

The resulting model serves as a high-accuracy infant pose estimator and can be further used for annotation correction, dataset refinement, and downstream developmental analysis tasks.

## Tech Stack

* Python
* PyTorch
* ViTPose++
