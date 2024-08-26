# ISIC2024_Skin_Cancer_Detection
Identify cancers among skin lesions from 3D photographs and metadata (Erdos Institute 2024 Deep Learning Project)

### Team Members

- **Madelyn Esther Cruz**: Lead contributor and primary developer.
- **Maksim Kosmakov**: Lead contributor and primary developer.
- **Sarasij Maitra**: Contributing member who helped with model training.
- **Jinjin Zhang**: Contributing member who helped with EDA.

### Overview
The goal of this project is to develop image-based algorithms to identify histologically confirmed skin cancer cases with single-lesion crops from 3D total body photos (TBP).  This project is inspired by the [ISIC2024_Skin_Cancer_Detection Kaggle Competition](https://www.kaggle.com/competitions/isic-2024-challenge). Skin cancer can be fatal if not diagnosed early. Dermoscopy-based AI algorithms should help clinicians in diagnosing melanoma, basal cell, and squamous cell carcinoma by aiding early diagnosis and disease prognosis. Also, determining which individuals should see a clinician in the first place is impactful. We make a combined dataset consisting of both tabular data and image data and train it using tabular modelling and various image recognition architectures.

<img width="601" alt="Screenshot 2024-08-20 at 20 47 09" src="https://github.com/user-attachments/assets/e6e31de4-4d49-4bda-88f2-226d30ccf2d8">

## Stakeholders

- underserved populations lacking specialized dermatologic care
- dermatologists and primary care clinicians  
- initiatives such as [SunSmart](https://www.sunsmart.com.au/), aiding in public awareness and education
  
## Key Performance Indicators (KPIs)


Our primary KPI for this project is the Partial Area Under the ROC Curve (pAUC) above 80% True Positive Rate (TPR). This metric evaluates the performance of the binary classification algorithm with a focus on clinical relevance and high sensitivity.

- pAUC above 80% TPR: The primary scoring metric for submissions. It measures the area under the ROC curve, but only for the portion where the TPR is above 80%. This ensures that the classifier maintains a high sensitivity, which is crucial for early cancer detection. Scores range from 0.0 to 0.2, reflecting the classifier’s ability to correctly identify malignant cases while minimizing false negatives.


## Data Overview
- We used the [SLICE-3D dataset](https://challenge2024.isic-archive.com/) available on the Kaggle competition. The image dataset contains every lesion from a subset of thousands of patients seen between the years 2015 and 2024 across nine institutions and three continents. The dataset consists of diagnostically labelled images with additional metadata in .csv file. These associated .csv file contains a binary diagnostic label (target), potential input variables (e.g. age_approx, sex, anatom_site_general, etc.)

### Meta Data (csv) and Exploratory Data Analysis (EDA)
  - 401059 Rows, 55 columns
  - Feature Types:  Includes both categorical and continuous variables.         
  - Missing Values: 3k  for age, 12k for sex, and 6k for anatomical site general. We imputed NAN values with the mode of the respective feature.
  - Multiple columns describing various attributes of the total body photo (tbp) images (e.g., eccentricity of lesion, anatomical location, perimeter of lesion, etc.) These      are significantly correlated.

### 3D Images
  - Images are in jpeg; vary in size, with a broad range of dimensions,  top 5: 133x133 - ~21k, 131x131 - ~21k, 129x129 - ~20k, 135x135 - ~20k, 137x137 - ~19k.
  - Here are some examples of images which were diagnosed benign and malignant respectively.

## Repository Structure

This repository is organized into the following folders and files, each serving a specific purpose in the development and evaluation of our models:

### Root Directory
- **`Presentation.pptx`**: Contains our presentation and results.
- 
### Articles Folder
Contains information about the image and tabular data:
- Provides explanations of the columns in the competition’s CSV file and details on the origin of the images used in the competition.
- 
### EDA Folder
Contains two notebooks for exploratory data analysis:
- **`Initial_Analysis.ipynb`**: Provides an initial analysis of the metadata, including the relevance of features using Random Forest and correlation between features.

### Fastai Folder
Contains notebooks for processing and modeling:
- **`Oversampling_Only.ipynb`**: Applies synthetic oversampling to the metadata and image augmentations (hue, contrast, rotation) to enhance the dataset.
- **`No_Oversampling_ResNet50.ipynb`**: Implements a ResNet50 model combining tabular and image data without oversampling. This model has been a top performer with a pAUC score of 0.133.
- **`10k_to_1k.ipynb`**: Our best-scoring model (pAUC=0.140), which uses an ensemble of ResNet50 and EfficientNetV2 trained on oversampled data (10k benigns to 1k melanomas) with the Adam optimizer and L2 weights.

### PyTorch Folder
- (Include details about this folder once it's populated or if there's specific content you want to describe.)

### Notes
- The Fastai code was developed using Kaggle's environment. To run these notebooks locally, you need to adjust the file paths and download the dataset from the Kaggle competition.
- This repository contains only the notebook with the best pAUC score. Other notebooks that were run are not included in this repository.
- Below, we list some results of the notebooks that were run, showcasing their performance metrics.

Feel free to explore each notebook and folder to understand the detailed implementation and results of our project.






### EDA:
- EDA (Random Forest).ipynb
- EDA_updated.ipynb
- ISIC.ipynb

### Training Codes:
- Image+MetaCombined.ipynb
- ISIC_MC_Deep Learning_Images.ipynb
- fastai-image-4.ipynb

### Testing Codes:
- ISIC_inference.ipynb
  
## Model Comparison

Here is a summary of the different models and their performances:

| **Oversampling** | **Image Model**              | **Epochs** | **Loss Function** | **Valid Score** | **pAUC** |
|------------------|-------------------------------|------------|-------------------|-----------------|----------|
| 100k:10K         | ResNet50 + Efficient_v2       | 0 + 5, 0 + 5  | CrossEntropy        | 0.0025          | 0.140    |
| 100k:10K         | ResNet50 + Efficient_v2       | 0 + 1    | CrossEntropy      | 0.0050          | 0.134    |
| No               | ResNet                        | 0 + 1    | CrossEntropy      | 0.0064          | 0.133    |
| 10k:1k           | ResNet50                      | 5 + 2    | CrossEntropy      | 0.108           | 0.127    |
| 100k:10k         | ResNet50                      | 7 + 3    | pAUC              | 0.962           | 0.018    |
| 100k:10k         | ResNet50 + Efficient_v2       | 5 + 2,5+2     | CrossEntropy      | 0.088           | 0.090    |
| No               | PyTorch ResNet50              | 22         | FocalLoss         | 0.0043          | 0.126    |
| No               | PyTorch ResNet50 + EfficientNet (Ensemble) | 22, 11 | FocalLoss | -               | 0.126    |

*Note: For entries with a `+` sign in the Epochs column, the first number corresponds to `learn_one_cycle` and the second number corresponds to `lr.fine_tune`. If there are two models, commas `,` separate the training cycles for each model.

## Conclusion
 - The FastAI ImageTab model using ResNet50 and EfficientNetV2, trained on oversampled data, achieved the best performance.
 - Using an ensemble approach that combined the metadata model and the image model did not improve the results compared to using the image model alone.
 - Challenges: High RAM usage for DataLoader (slow training and requires lots of memory), Possible overfitting
 - Advantages of our work: We explored multiple approaches and leveraged various pretrained models, which saved time and yielded good results.

## Future work
  - We plan to explore other ensemble models and techniques to improve image quality, such as hair removal. 
  - Why does more training lead to a lower score, despite overfitting penalties? 
  - Find more efficient ways to overcome the RAM requirements in the pytorch code.

### Sources

International Skin Imaging Collaboration. SLICE-3D 2024 Challenge Dataset. International Skin Imaging Collaboration (https://doi.org/10.34970/2024-slice-3d) (2024).
Creative Commons Attribution-Non Commercial 4.0 International License.
The dataset was generated by the International Skin Imaging Collaboration (ISIC) and images are from the following sources: Hospital Clínic de Barcelona, Memorial Sloan Kettering Cancer Center, Hospital of Basel, FNQH Cairns, The University of Queensland, Melanoma Institute Australia, Monash University and Alfred Health, University of Athens Medical School, and Medical University of Vienna.
