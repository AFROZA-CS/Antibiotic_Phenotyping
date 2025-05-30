# Antibiotic_Phenotyping

##  Part 1: Segmentation Accuracy Evaluation of Cellpose on Antibiotic-Treated Images

In the first stage of this project, I systematically evaluated the performance of the Cellpose segmentation model on phase-contrast microscopy images of bacterial cells subjected to different antibiotics. Ground truth masks were manually annotated using FIJI, while model-generated masks were obtained via Cellpose. To quantify performance, I computed key pixel-level segmentation metrics—F1 score (Dice coefficient), Intersection over Union (IoU), precision, recall, and accuracy—across a curated set of 20 images. Additionally, I removed small artifacts to reduce false positives and produced per-image overlays visualising true positives (green), false positives (red), and false negatives (blue). A cumulative confusion matrix was generated to summarise performance across all images. Mean F1 and IoU scores exceeded 0.90 and 0.83, respectively, with recall consistently high, indicating the model captured nearly all true cell regions.

This pipeline ensures reproducible, quantitative, and visual validation of Cellpose in biologically relevant conditions and lays the groundwork for the next phases of this study.


<p align="center">
  <img src="part1.png" width="400"/>
</p>

Segmentation Mask Validation: Pixel-wise Metrics

To verify the quality of Cellpose-generated segmentation masks, we performed pixel-level comparison against manually annotated ground truth. Metrics such as F1 Score, IoU, Precision, Recall, and Accuracy were computed for each image.

<div align="center"> <img src="Screenshot%202025-05-27%20at%2019.01.50.png" width="720"/> <p style="text-align: center;"><em>Per-image segmentation metrics (F1, IoU, Precision, etc.)</em></p> </div>
The results demonstrate high agreement between predicted and ground truth masks, with:

Mean F1 Score ≈ 0.91

Mean IoU ≈ 0.83

Mean Recall ≈ 0.97

In addition, the overall confusion matrix shows strong accuracy across all pixels:

<div align="center"> <img src="confusion_matrix_percent.png" width="300"/> <p style="text-align: center;"><em>Normalized confusion matrix across all images</em></p> </div>
✔These results confirm that the predicted masks are sufficiently accurate and can be reliably used for further morphological or phenotypic analysis.

##  Part 2 — Single-Cell Extraction & Classification
Segmented single-cell patches (membrane + DNA) were used to train a ResNet-18 model to classify antibiotic treatments based on morphology and fluorescence patterns.

Output:
Objective:
Extract single bacterial cells from microscopy images using segmentation masks, then classify them based on morphological features visible in membrane (red) and DNA (blue) channels.

# Process:
Used validated masks to extract individual cells

Resized cells to 64×64 patches

Labeled each patch by antibiotic condition

Trained a lightweight CNN classifier to predict the antibiotic class based on visual features

# Dataset Summary:
Class	Cell Count
Control	75
Antibiotic_A	78
Antibiotic_B	82
Antibiotic_C	174
Antibiotic_D	38
Total	447

# Classifier Used:
Model: CNN with 2 convolutional layers + dense layers

Input: RGB patches (64×64)

Output: Antibiotic class (5-way classification)

Split: 80% train / 20% test (random across all classes)

# Classification Results:
<p align="center"> <img src="your_repo_path/confusion_matrix_percent.png" alt="Confusion Matrix" width="500"/> </p>
Training Accuracy: ~99%

Test Accuracy: High accuracy observed for dominant classes

Morphological traits helped the model learn drug-induced differences
