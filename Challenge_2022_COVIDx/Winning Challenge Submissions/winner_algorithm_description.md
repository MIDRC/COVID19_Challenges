# 1 - Submission_final_HFSPUW_ensemble

**Model architecture:** densenet121, efficientnetb6, efficientnetb7, seresnet152d from TIMM model library ([https://github.com/rwightman/pytorch-image-models](https://github.com/rwightman/pytorch-image-models)).

Input image size: 256 for densenet121 and seresnet152d; 528 for efficientnetb6; 600 for efficientnetb7

**Training data:**

(1). NIH chestx-ray8 dataset, 112,000 CXRs of 14 class labels (2). BIMCV COVID chest x-ray dataset: [https://bimcv.cipf.es/bimcv-projects/bimcv-covid19/](https://bimcv.cipf.es/bimcv-projects/bimcv-covid19/)

(3). Privately curated COVID chest x-ray datasets (HF, UW). Details can be found in these two papers:

A. Zhang R, Tie X, Qi Z, Bevins NB, Zhang C, Griner D, Song TK, Nadig JD, Schiebler ML, Garrett JW, Li K, Reeder SB, Chen GH. Diagnosis of Coronavirus Disease 2019 Pneumonia by Using Chest Radiography: Value of Artificial Intelligence. Radiology. 2021 Feb;298(2):E88-E97. doi: 10.1148/radiol.2020202944. Epub 2020 Sep 24. PMID: 32969761; PMCID: PMC7841876.

Zhang, R., Tie, X., Garrett, J. W., Griner, D., Qi, Z., Bevins, N. B., ... & Chen, G. H. (2022). A Generalizable Artificial Intelligence Model for COVID-19 Classification Task Using Chest X-ray Radiographs: Evaluated Over Four Clinical Datasets with 15,097 Patients. arXiv preprint arXiv:2210.02189.

**Training details:**

(1). Pretraining each model using the NIH dataset. Adam optimizer was used with an initial learning rate of 0.0001.

(2). Combining HF, UW, and BIMCV datasets for training. The downloaded MIDRC dataset was used as the validation set. Binary cross-entropy loss was used. Model weights correspond to the highest validation AUC was saved. Adam optimizer was used with an initial learning rate of 0.00005.

(3). For each model architecture, 5 models were trained.

(4). The top-performing models (8 models) were used to generate the ensemble score for the final prediction.

# 2 - Submission_1016_HF

**Model architecture:** densenet121, efficientnet_v2_m and swin_b from the torchvision library.

Input image size: 224.

**Training data:**

(1). NIH chestx-ray8 dataset, 112,000 CXRs of 14 class labels.

(2). Privately curated COVID chest x-ray dataset (HF). Details can be found in the following paper:

Zhang, R., Tie, X., Garrett, J. W., Griner, D., Qi, Z., Bevins, N. B., ... & Chen, G. H. (2022). A Generalizable Artificial Intelligence Model for COVID-19 Classification Task Using Chest X-ray Radiographs: Evaluated Over Four Clinical Datasets with 15,097 Patients. arXiv preprint arXiv:2210.02189.

**Training details:**

(1). Pretraining each model using the NIH dataset. Adam optimizer was used with an initial learning rate of 0.00001.

(2). Using HF dataset for training. The downloaded MIDRC dataset was used as the validation set. Cross-entropy loss was used. Model weights correspond to the highest validation AUC was saved. Adam optimizer was used with an initial learning rate of 0.00005.

(3). For each model architecture, 5 models were trained.

(4). The top-performing models (5 models) were used to generate the ensemble average for the final prediction.

# 3 - Submission_1102_HFMIDRC

Model architecture: efficientnet_v2_m and swin_b from the torchvision library.

Input image size: 480 for efficientnet_v2_m; 224 for swin_b.

**Training data:**

(1). NIH chestx-ray8 dataset, 112,000 CXRs of 14 class labels.

(2). Privately curated COVID chest x-ray dataset (HF). Details can be found in the following paper:

Zhang, R., Tie, X., Garrett, J. W., Griner, D., Qi, Z., Bevins, N. B., ... & Chen, G. H. (2022). A Generalizable Artificial Intelligence Model for COVID-19 Classification Task Using Chest X-ray Radiographs: Evaluated Over Four Clinical Datasets with 15,097 Patients. arXiv preprint arXiv:2210.02189.

(3). MIDRC training dataset. Images with abnormal image contrast were removed.

**Training details:**

(1). Pretraining each model using the NIH dataset. Cross-entropy loss was used. Adam optimizer was used with an initial learning rate of 0.0001.

(2). Combining the HF dataset and MIDRC dataset for training. A patient-based train/validation split was performed on the MIDRC dataset. Size of the validation set: 250 COVID+ cases and 1000 COVID- cases. The rest of the MIDRC dataset is combined with the HF dataset for model training. Cross-entropy loss was used. Model weights correspond to the highest validation AUC was saved. Adam optimizer was used with an initial learning rate of 0.00005.

(3). For each model architecture, 5 models were trained.

(4). The top-performing models (6 models) were used to generate the ensemble score for the