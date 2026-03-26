# CIFAR-10 Image Classification: From MLP to ResNet18

This project demonstrates the process of building and improving an image classification model using the **CIFAR-10** dataset. I started with a simple architecture and gradually applied advanced models and optimization techniques to achieve high performance.

---

## Key Highlights
* **Data Pipeline**: Automated data loading using `torchvision.datasets`.
* **Model Evolution**: Improved accuracy by moving from MLP to CNN and finally to **Pre-trained ResNet18**.
* **Experimental Approach**: Analyzed data characteristics through RGB channel tests (Grayscale, Red-only).
* **Optimization**: Solved overfitting using Data Augmentation, Dropout, and Schedulers.

---

## Evolutionary Path

### 1. Model Structure: MLP → CNN → ResNet
* **MLP (1st Trial)**: Flattening images into 1D vectors caused a loss of **Spatial Information**, leading to a performance limit of **51% accuracy**.
* **CNN (2nd Trial)**: Used kernels for feature extraction, which preserved spatial data and significantly boosted performance.
* **ResNet18 (3rd~7th Trial)**: Used Residual Connections and pre-trained weights to break the 80% accuracy barrier, reaching a **final accuracy of 95.8%**.

### 2. Strategy for Overfitting
* **Problem**: During the 3rd trial, Train Loss decreased but Test Loss increased (Overfitting).
* **Solution**: 
    * **Data Augmentation**: Used random flips and rotations to help the model generalize better.
    * **Regularization**: Applied Dropout and Learning Rate Schedulers to encourage learning 'universal features' instead of just memorizing data.

### 3. Value of Color Information
* **Grayscale/Red Channel (5~6th Trial)**: Achieved ~88% accuracy using only shapes and textures. This shows the model's feature extractor is very powerful even without full color.
* **Full Color (7th Trial)**: Reached the highest performance (**95.8%**) by allowing all RGB channels to interact.

---

## Analysis & Troubleshooting

### Q1. Why does the model show "Over-confidence"?
* **Analysis**: The model often gives a 99% probability for a specific class. This is a common issue in deep learning, especially when the model (ResNet18) is complex but the dataset is relatively small.
* **Solution**: In the future, I can use **Label Smoothing** to create a more "humble" model with a softer probability distribution.

### Q2. Why are the output images' colors distorted?
* **Cause**: This happened because of **Normalization**. Pre-trained models expect data normalized with ImageNet stats. When visualizing, I missed the **Unnormalization** step.
* **Fix**: I added a denormalization process to bring the values back to the `[0, 1]` range for correct display.

---

## Lessons Learned (Expert Feedback)
* **Channel Limitation**: Restricting color (Grayscale/Red-only) is generally not recommended because all RGB channels provide valuable data for classification.
* **Normalization Stats**: For CIFAR-10, it is better to use specific stats: `mean=(0.4914, 0.4822, 0.4465)` and `std=(0.2023, 0.1994, 0.2010)`.
* **Architecture vs. Resizing**: Instead of resizing images (32x32 to 128x128) to fit a large model, modifying the model layers to fit the original image size can be more efficient.
* **Unnormalization**: Always reverse the normalization formula and use `clip(0, 1)` before plotting to ensure correct colors.

---

## Future Work & Goals
1. **Deepen Knowledge**: Study CNN and ResNet architectures in more detail.
2. **Fine-tuning Practice**: Select a Kaggle dataset and practice fine-tuning a pre-trained model on real-world data.
3. **Data Quality**: Work with unrefined datasets (high resolution, low noise) and focus on preprocessing without excessive resizing.

---
*Note: This code was developed with the assistance of an LLM to learn and improve coding structures through 7 iterations of trial and error.*
