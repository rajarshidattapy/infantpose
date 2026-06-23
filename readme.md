# InfantPose: Fine-Tuning ViTPose++ for Infant Keypoint Estimation

## Overview

InfantPose is a domain-adapted pose estimation pipeline built by fine-tuning ViTPose++ on a custom infant pose dataset containing 2,000 annotated images, under IISC, Bangalore.

The project aims to improve keypoint localization accuracy for infant body structures, which differ significantly from standard adult human pose datasets such as COCO. The resulting model can be used for pose estimation, annotation correction, dataset quality assurance, and downstream medical or developmental analysis.

## Dataset

* Total Images: 2,000
* Annotation Format: YOLO Pose
* Keypoints: 14
* Domain: Infant body pose estimation

### Keypoint Schema

1. Arm A Distal Endpoint
2. Arm A Middle Joint
3. Arm A Upper Joint
4. Arm B Upper Joint
5. Arm B Middle Joint
6. Arm B Distal Endpoint
7. Arm B Lower Endpoint
8. Pelvis Center
9. Leg B Upper Joint
10. Leg B Middle Joint
11. Leg B Distal Endpoint
12. Leg A Upper Joint
13. Leg A Middle Joint
14. Leg A Distal Endpoint

## Methodology

### 1. Dataset Conversion

YOLO pose annotations are converted into COCO-style keypoint annotations compatible with the MMPose training framework.

### 2. Transfer Learning

A pretrained ViTPose++ Base model trained on COCO human pose data is used as the initialization checkpoint.

### 3. Head Adaptation

The original 17-keypoint prediction head is replaced with a custom 14-keypoint head corresponding to the infant skeleton definition.

### 4. Progressive Fine-Tuning

#### Stage 1

* Freeze backbone
* Train keypoint head
* Learn infant-specific keypoint semantics

#### Stage 2

* Unfreeze full network
* End-to-end fine-tuning
* Optimize for infant pose distributions

### 5. Data Augmentation

* Horizontal Flip
* Rotation
* Scaling
* Brightness Adjustment
* Contrast Adjustment
* Occlusion Simulation

## Training Pipeline

Dataset
→ COCO Conversion
→ ViTPose++ Base Initialization
→ Head Replacement (17 → 14)
→ Head-Only Training
→ Full Fine-Tuning
→ Validation
→ Checkpoint Selection
→ Evaluation

## Evaluation

Metrics:

* PCK (Percentage of Correct Keypoints)
* AP (Average Precision)
* Validation Loss

Special focus is given to:

* Wrist localization
* Ankle localization
* Occluded joints
* Extreme infant poses


This allows automatic identification of potentially incorrect annotations while minimizing manual review effort.

## Tech Stack

* Python
* PyTorch
* ViTPose++
* COCO Annotation Format
