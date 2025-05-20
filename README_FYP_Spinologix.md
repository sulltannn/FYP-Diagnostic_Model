#  Automated Spinal Stenosis Diagnosis Using MRI Scans

This repository contains the full pipeline for a deep learning-based system to detect and classify lumbar spinal stenosis conditions using MRI scans. The solution follows a modular, clinically-aligned architecture, dividing the diagnostic process into three specialized tasks:

1. **Instance Prediction**
2. **Coordinate Prediction**
3. **Severity Classification**

An integrated inference pipeline combines the outputs from each module to generate level-wise, condition-specific diagnostic results.

---

##  Repository Structure

```
.
├── FYP_Instance_Prediction.ipynb       # Module 1: Predicts slice per spinal level
├── FYP_Coordinate_Prediction.ipynb     # Module 2: Predicts (x, y) region coordinates
├── FYP_Severity_Prediction.ipynb       # Module 3: Classifies stenosis severity
└── FYP_Inference.ipynb                 # Complete inference pipeline integrating all modules
```

---

##  Module 1: Instance Prediction

- **Objective**: Predict the most relevant axial/sagittal slice (instance number) for each spinal level where a degenerative condition is present.
- **Input**: 3D MRI volumes.
- **Approach**:
  - Volumes are preprocessed and spatially resampled.
  - A deep regression model is trained to output a normalized slice index per spinal level.
  - One model is used per stenosis condition.
- **Output**: A set of slice indices indicating the central location of pathology per level.

---

##  Module 2: Coordinate Prediction

- **Objective**: Localize the region of stenosis by predicting the (x, y) coordinates of affected areas in 2D slices.
- **Input**: 2D MRI slices centered at predicted instances.
- **Approach**:
  - A shared encoder-decoder architecture processes the image.
  - The decoder connects to multiple regression heads, one per spinal level.
  - Each head outputs (x, y) coordinates.
- **Output**: A set of 2D coordinates for each spinal level indicating localized pathology.

---

##  Module 3: Severity Classification

- **Objective**: Predict the severity of stenosis (Normal, Moderate, Severe) for each spinal level and condition.
- **Input**: 2D MRI slices cropped around predicted coordinates.
- **Approach**:
  - A classification model processes the localized image patches.
  - Separate models are trained per condition and view (e.g., sagittal or axial).
  - Different pretrained models were fine-tuned for this task.
- **Output**: A severity class label for each spinal level and condition.

---

##  Inference Pipeline: `FYP_Inference.ipynb`

- **Objective**: Combine all three modules to perform full spine-level stenosis diagnosis.
- **Flow**:
  1. Load volume and preprocess.
  2. Run instance prediction to locate key slices.
  3. Extract 2D slices and run coordinate prediction.
  4. Crop regions of interest.
  5. Run severity classification on cropped patches.
- **Output**: A complete set of spinal level-wise diagnostic outputs: predicted slice, coordinates, and severity for each condition.

---

##  Notes

- All models were trained and evaluated using labeled DICOM MRI images.
- Preprocessing steps include resampling, intensity normalization, and augmentation.
- This solution is built for interpretability, modularity, and clinical relevance.

Feel free to explore each notebook to understand the training procedures and inference logic.

---


