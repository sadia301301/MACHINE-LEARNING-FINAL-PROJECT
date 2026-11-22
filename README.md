# Diabetic Retinopathy Detection Using EfficientNet-B3

## Project Overview

Diabetic Retinopathy (DR) is an eye disease caused by diabetes that can damage the blood vessels in the retina and may lead to vision loss.

This project focuses on developing a deep learning-based system to detect and grade Diabetic Retinopathy from retinal fundus images using **EfficientNet-B3**.

The model predicts the severity of Diabetic Retinopathy across five grades, from No DR to Proliferative DR.

## Project Objectives

- Develop a deep learning model for Diabetic Retinopathy severity grading.
- Preprocess retinal fundus images to improve image quality.
- Apply transfer learning using EfficientNet-B3.
- Predict DR severity using a regression-based approach.
- Implement four-view prediction for more stable outputs.
- Explore screening-oriented threshold optimisation, uncertainty-based referral and explainable AI.

## Dataset

**APTOS 2019 Blindness Detection Dataset**

The project uses 3,662 labelled retinal fundus images.

The dataset contains five DR severity categories:

| Grade | Classification |
|---|---|
| 0 | No DR |
| 1 | Mild |
| 2 | Moderate |
| 3 | Severe |
| 4 | Proliferative DR |

Dataset source: [APTOS 2019 – Kaggle](https://www.kaggle.com/competitions/aptos2019-blindness-detection)

## Technologies Used

- Python
- Google Colab
- TensorFlow
- Keras
- EfficientNet-B3
- OpenCV
- Scikit-learn
- NumPy
- Pandas
- Matplotlib

## Project Workflow

### 1. Dataset Collection
Collected labelled retinal fundus images from the APTOS 2019 dataset.

### 2. Image Preprocessing
Applied the following preprocessing techniques:

- Retinal cropping to remove black borders.
- Square padding to maintain image proportions.
- Resizing images to 300 × 300 pixels.
- Ben Graham local-contrast normalisation.
- Circular masking to reduce unwanted corner regions.

### 3. Data Splitting
The dataset was divided using stratified sampling:

- Training: 70%
- Validation: 10%
- Testing: 20%

### 4. Model Development
Used ImageNet-pretrained EfficientNet-B3 with a custom regression head consisting of:

- Global Average Pooling
- Dropout (0.30)
- One dense output neuron for severity score prediction

### 5. Model Training
Training was performed in two phases:

- Phase 1: Frozen backbone and trained the newly added output layers.
- Phase 2: Unfrozen backbone and fine-tuned the model.

**Loss Function:** Huber Loss  
**Optimiser:** Adam

Data augmentation and early stopping were used during training.

### 6. Prediction Process
The model evaluates each retinal image using four views:

- Original image
- Left-right flipped image
- Up-down flipped image
- Both-direction flipped image

The four predicted scores are averaged. Validation-tuned thresholds convert the averaged score into one of the five DR grades.

## Implemented Extensions

### 1. Screening-Oriented Threshold Optimisation
Implemented an alternative set of grade thresholds to improve the detection of severe DR cases for screening purposes.

### 2. Uncertainty-Based Referral
Used the variation between predictions from four flipped views to estimate uncertainty. Images with higher uncertainty can be referred to a specialist.

### 3. Explainable AI – Class Activation Maps (CAM)
Generated Class Activation Maps to visualise image regions influencing the predicted severity score.

## Evaluation Metrics

The model was evaluated using:

- Accuracy
- Balanced Accuracy
- Sensitivity
- Specificity
- Precision
- Recall
- F1-Score
- Area Under the ROC Curve (AUC-ROC)
- Quadratic Weighted Kappa (QWK)

## Running the Project

1. Open Google Colab.
2. Upload the project notebook.
3. Download or provide access to the APTOS 2019 dataset.
4. Run the notebook cells in sequence.
5. Execute image preprocessing and model training.
6. Generate predictions using the trained EfficientNet-B3 model.
7. Test the model with retinal fundus images.

## Project Outputs

- Preprocessed retinal images.
- Trained EfficientNet-B3 model.
- Training and validation loss visualisations.
- Predicted DR severity grades.
- Four-view prediction results.
- Screening-oriented threshold analysis.
- Uncertainty-based referral analysis.
- CAM visualisations.

## Future Improvements

- Pretraining with the larger EyePACS dataset.
- Improving performance on underrepresented DR grades.
- Exploring additional model architectures.
- Reducing border artefacts in CAM visualisations.
- External validation using datasets such as IDRiD and Messidor.
- Developing a user-friendly web interface.

## Limitations

- The model was trained using the APTOS 2019 dataset.
- The dataset contains an imbalance between DR severity classes.
- External validation using images from other hospitals and cameras is still required.
- The system is an educational prototype and is not intended for clinical diagnosis.

## Team Members

- G. Mukunda Priya
- K. Ushasri Unnathi
- Pathan Sadiya
- K. Abiramasree

**Project Guide:** Dr. Ajay Dilip Kumar  
**Institution:** SRM University–AP

---

*This project is developed for academic and research purposes. It is not a substitute for professional medical diagnosis.*
