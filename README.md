# ISIC2024_Skin_Cancer_Detection
Identify cancers among skin lesions from 3D photographs and metadata (Erdos Institute 2024 Deep Learning Project)

### Team Members

Madelyn Esther Cruz, Maksim Kosmakov, Sarasij Maitra, Jinjin Zhang

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

## Codes

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
  
## Models and Best pAUC
  - ImageTab (CNN with Optimizer) - 0.133
  - ResNet50 (finetuned https://huggingface.co/microsoft/resnet-50) - 0.126
  - Ensemble ResNet50 + EffNet (finetuned https://www.kaggle.com/code/nadhirhasan/tabular-based-pytorch-ann) - 0.126

## Conclusion
 - The FastAI ImageTab model delivered the best performance. 
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
