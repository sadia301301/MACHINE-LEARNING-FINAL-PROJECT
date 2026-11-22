# Diabetic Retinopathy Detection Using EfficientNet-B3

## 1. Project Overview

Diabetic Retinopathy (DR) is an eye disease caused by complications of diabetes. It affects the blood vessels in the retina and can lead to vision loss if it is not detected at an early stage.

This project focuses on developing a deep learning-based system to identify and classify Diabetic Retinopathy from retinal fundus images. It uses the **EfficientNetB3** architecture, which is pretrained on ImageNet and fine-tuned using the APTOS 2019 Blindness Detection dataset.

The system predicts the severity of Diabetic Retinopathy across five grades, from No DR to Proliferative DR. It also includes additional features such as screening-oriented threshold optimisation, uncertainty-based referral, and Class Activation Maps (CAM) for visual explanation.

## 2. Project Objectives

- To develop an automated system for detecting Diabetic Retinopathy from retinal fundus images.
- To classify retinal images into five DR severity levels.
- To apply image preprocessing techniques to improve image quality.
- To use transfer learning with the EfficientNetB3 architecture.
- To explore screening-oriented classification and uncertainty-based referral.
- To provide visual explanations using Class Activation Maps.

## 3. Dataset Description

This project uses the **APTOS 2019 Blindness Detection Dataset**.

The dataset contains 3,662 labelled retinal fundus images. Each image is assigned one of five severity grades.

| Grade | Classification |
|---|---|
| 0 | No Diabetic Retinopathy |
| 1 | Mild Diabetic Retinopathy |
| 2 | Moderate Diabetic Retinopathy |
| 3 | Severe Diabetic Retinopathy |
| 4 | Proliferative Diabetic Retinopathy |

The dataset is divided using stratified sampling:

- Training set: 70% (2,563 images)
- Validation set: 10% (366 images)
- Testing set: 20% (733 images)

The stratified split helps maintain the distribution of the five classes across the datasets.

## 4. Technologies Used

- **Programming Language:** Python
- **Deep Learning Framework:** TensorFlow / Keras
- **Model Architecture:** EfficientNetB3
- **Dataset:** APTOS 2019 Blindness Detection
- **Development Environment:** Google Colab
- **Hardware:** NVIDIA T4 GPU
- **Image Processing:** OpenCV
- **Data Handling:** NumPy, Pandas
- **Visualization:** Matplotlib

## 5. Project Workflow and Methodology

### Step 1: Data Collection

The APTOS 2019 dataset is used to train and evaluate the model. It contains retinal fundus images with their corresponding DR severity labels.

### Step 2: Image Preprocessing

Preprocessing is performed to make retinal images more suitable for deep learning.

The following techniques are applied:

1. **Retinal Cropping:** Removes unnecessary black borders around the retinal image.
2. **Square Padding:** Converts images into a square format while maintaining their proportions.
3. **Image Resizing:** Resizes images to 300 × 300 pixels.
4. **Ben Graham Normalisation:** Applies local contrast normalisation to improve retinal details.
5. **Circular Masking:** Helps focus on the circular retinal region.

### Step 3: Data Augmentation

Data augmentation is applied only to the training images to improve the model's ability to handle variations in retinal images.

The techniques include:

- Random horizontal and vertical flipping.
- Random rotation up to ±180 degrees.
- Zoom transformations.
- Brightness adjustments.
- Contrast adjustments.

### Step 4: Model Development

The project uses **EfficientNetB3**, a convolutional neural network pretrained on ImageNet.

A custom output head is added to the pretrained model. It consists of:

- Global Average Pooling layer.
- Dropout layer with a rate of 0.30.
- Dense output layer with one neuron for predicting a continuous severity score.

The predicted score is converted into one of the five DR grades using classification thresholds.

### Step 5: Model Training

The model is trained using a two-phase training approach.

**Phase 1 – Feature Extraction**

- The pretrained EfficientNetB3 backbone is frozen.
- The newly added classification head is trained.
- This allows the model to learn task-specific features.

**Phase 2 – Fine-Tuning**

- Selected layers of the EfficientNetB3 backbone are unfrozen.
- The model is fine-tuned to learn features specific to retinal images.
- Batch Normalization layers remain frozen.

The training configuration includes:

- Optimizer: Adam
- Loss Function: Huber Loss
- Early Stopping
- Learning Rate Reduction

The model is trained in Google Colab using an NVIDIA T4 GPU.

### Step 6: Model Prediction

For a new retinal image, the same preprocessing pipeline is applied.

The model evaluates the image using four different views:

1. Original image.
2. Left-right flipped image.
3. Up-down flipped image.
4. Both left-right and up-down flipped image.

The four prediction scores are averaged to obtain the final prediction score. Validation-tuned thresholds are then used to assign the predicted DR grade.

## 6. Novel Contributions and Implemented Extensions

This project includes three additional implemented extensions to improve the screening and interpretation process.

### 1. Screening-Oriented Threshold Optimisation

Instead of using only standard grade thresholds, an alternative set of thresholds is tuned using validation data.

The purpose is to improve the detection of severe Diabetic Retinopathy cases for screening purposes.

This approach focuses on identifying potentially serious cases that may require further medical attention. However, improving severe-grade detection can involve a trade-off with overall classification accuracy.

### 2. Uncertainty-Based Referral

The model generates four prediction scores using the original image and its three flipped versions.

The variation among these prediction scores is measured using standard deviation.

- Lower variation indicates more consistent predictions.
- Higher variation indicates greater uncertainty in the prediction.

When the prediction uncertainty is high, the system can flag the case for further examination by a specialist.

This feature helps identify cases where the model's prediction may be less reliable.

### 3. Explainable AI Using Class Activation Maps (CAM)

Class Activation Maps (CAM) are used to visualise the image regions that contribute to the model's predicted severity score.

The CAM heatmap is generated using the final feature maps and the weights of the output neuron.

The heatmap is displayed over the retinal image to provide a visual representation of the regions influencing the prediction.

This feature helps us inspect the model's behaviour. However, the heatmap does not guarantee that the model is focusing on medically correct regions.

**Note:** These three extensions have been implemented and evaluated as part of the project. They are additional contributions built around the main EfficientNetB3 model.

## 7. Model Evaluation

The model is evaluated using the test dataset.

The following evaluation metrics are considered:

- Accuracy
- Balanced Accuracy
- Sensitivity
- Specificity
- Precision
- Recall
- F1-Score
- AUC-ROC
- Quadratic Weighted Kappa (QWK)
- Within ±1 Grade Accuracy
- Confusion Matrix

These metrics help assess the model's classification performance and its ability to distinguish between different DR severity levels.

## 8. How to Run the Project

The project is developed using Google Colab.

Follow these steps:

1. Open Google Colab.
2. Upload or open the project notebook.
3. Make sure the APTOS 2019 dataset is available.
4. Configure the runtime to use a GPU.
5. Run the notebook cells in the correct order.
6. Complete the preprocessing and model training steps.
7. Evaluate the trained model using the test dataset.
8. Use the prediction section to upload a new retinal fundus image.
9. View the predicted DR grade and related outputs.

## 9. Expected Output

After uploading a retinal fundus image, the system:

- Preprocesses the uploaded image.
- Generates predictions using four image views.
- Averages the prediction scores.
- Assigns a DR severity grade using the selected thresholds.
- Displays the predicted severity level.
- Provides an uncertainty estimate based on prediction variation.
- Generates a CAM heatmap for visual explanation.

The system also provides a screening-oriented prediction option using the optimised thresholds.

## 10. Limitations

- The system depends on the quality and diversity of the training dataset.
- Variations in image brightness, focus, and camera quality may affect predictions.
- The model may produce uncertain predictions for some retinal images.
- CAM heatmaps may highlight irrelevant regions or image artefacts.
- The system has not been established as a clinically validated diagnostic tool.

## 11. Future Improvements

Possible future improvements include:

- Training with larger and more diverse retinal image datasets.
- Exploring advanced deep learning architectures.
- Improving image quality assessment and preprocessing.
- Enhancing the reliability of uncertainty estimation.
- Improving the visual explanation methods.
- Further evaluating the model on independent datasets.
- Working towards clinical validation with medical experts.

## 12. Team Members

**Project Team:**

1. G. Mukunda Priya
2. K. Ushasri Unnathi
3. Pathan Sadiya
4. K. Abiramasree

**Project Guide:**

Dr. Ajay Dilip Kumar  
Assistant Professor

**Institution:**  
SRM University–AP, Andhra Pradesh, India

## 13. Disclaimer

This project is developed for academic and research purposes. It is an educational prototype and should not be used as a replacement for professional medical diagnosis or consultation. All medical decisions must be made by qualified healthcare professionals.
