# Final Submission 1:

**DATA**
We utilize the provided Challenge Data set. However, we manually filter out roughtly 200 bad examples. Furthermore, we only use one image per patient and discard all processed versions or other acquisitions. Furthermore, we randomly drop 50% of the images with label 0. This leaves us with a total number of 1795 images.
We split the data set into one test set of 224 images and perform 5-fold cross validation on the remaining 1571 images for training and validation.
We preprocess all images as described in: 
    Zhang, R., Tie, X., Garrett, J. W., Griner, D., Qi, Z., Bevins, N. B., ... & Chen, G. H. (2022). A Generalizable Artificial Intelligence Model for COVID-19 Classification Task Using Chest X-ray Radiographs: Evaluated Over Four Clinical Datasets with 15,097 Patients. arXiv preprint arXiv:2210.02189.
The final resolution is set to 224x224.

**MODEL**
We utilize a model ensemble of two different model architectures, namely SwinTransformer and ConvNext_small, each trained on 5 different folds. For SwinTransformer, we use Adam as optimizer, a batch size of 64 an initial learning rate of 0.00005 and cosine annealing as scheduling strategy. 
We use pretrained model checkpoints taken from the last MIDRC CovidX challenge first place (SwinTransformer) and third place (ConvNext) solution and fine tune them on the challenge data set. 

**Loss:**
We utilize different Loss functions for each model: For SwinTransformer, we use the "Conditional Ordinal Regression for Neural networks (CORN)" loss from 
    Xintong Shi, Wenzhi Cao, and Sebastian Raschka (2021). Deep Neural Networks for Rank-Consistent Ordinal Regression Based On Conditional Probabilities. Arxiv preprint; https://arxiv.org/abs/2111.08851 
For the ConvNext we use a combined loss that is based on the MAE loss and CORN loss, where we scale the CORN loss by a factor of 2.0. 
For all models, we directly predict the combined score for each patient.


# Final Submission 2:

**DATA**
We utilize the provided Challenge Data set. However, we manually filter out roughtly 200 bad examples. Furthermore, we only use one image per patient and discard all processed versions or other acquisitions. Furthermore, we randomly drop 50% of the images with label 0. This leaves us with a total number of 1795 images.
We split the data set into one test set of 224 images and perform 5-fold cross validation on the remaining 1571 images for training and validation.
In comparison to Submission 1: For the final submission, we fuse the training and test set. The validation set is kept for early stopping. 
We preprocess all images as described in: 
    Zhang, R., Tie, X., Garrett, J. W., Griner, D., Qi, Z., Bevins, N. B., ... & Chen, G. H. (2022). A Generalizable Artificial Intelligence Model for COVID-19 Classification Task Using Chest X-ray Radiographs: Evaluated Over Four Clinical Datasets with 15,097 Patients. arXiv preprint arXiv:2210.02189.
The final resolution is set to 224x224.

**MODEL**
We utilize a model ensemble of two different model architectures, namely SwinTransformer and ConvNext_small, each trained on 5 different folds. For SwinTransformer, we use Adam as optimizer, a batch size of 64 an initial learning rate of 0.00005 and cosine annealing as scheduling strategy. 
We use pretrained model checkpoints taken from the last MIDRC CovidX challenge first place (SwinTransformer) and third place (ConvNext) solution and fine tune them on the challenge data set. 

**Loss:**
We utilize different Loss functions for each model: For SwinTransformer, we use the "Conditional Ordinal Regression for Neural networks (CORN)" loss from 
    Xintong Shi, Wenzhi Cao, and Sebastian Raschka (2021). Deep Neural Networks for Rank-Consistent Ordinal Regression Based On Conditional Probabilities. Arxiv preprint; https://arxiv.org/abs/2111.08851 

For the ConvNext we use a combined loss that is based on the MAE loss and CORN loss, where we scale the CORN loss by a factor of 2.0. 
For all models, we directly predict the combined score for each patient.


# Final Submission 3:

**DATA**
We utilize the provided Challenge Data set. However, we manually filter out roughtly 200 bad examples. Furthermore, we only use one image per patient and discard all processed versions or other acquisitions. Furthermore, we randomly drop 50% of the images with label 0.This leaves us with a total number of 1795 images.
We split the data set into one test set of 224 images and perform 5-fold cross validation on the remaining 1571 images for training and validation.
In comparison to Submission 1: For the final submission, we fuse the training and test set. The validation set is kept for early stopping. 
We preprocess all images as described in: 
    Zhang, R., Tie, X., Garrett, J. W., Griner, D., Qi, Z., Bevins, N. B., ... & Chen, G. H. (2022). A Generalizable Artificial Intelligence Model for COVID-19 Classification Task Using Chest X-ray Radiographs: Evaluated Over Four Clinical Datasets with 15,097 Patients. arXiv preprint arXiv:2210.02189.
The final resolution is set to 224x224.

**MODEL**
We utilize a model ensemble of three different model architectures, namely SwinTransformer and ConvNext_small, and DeiT_base, each trained on 5 different folds. For SwinTransformer, we use Adam as optimizer, a batch size of 64 an initial learning rate of 0.00005 and cosine annealing as scheduling strategy. For ConvNext and DeiT, AdamW is used as optimizer with a batch size of 128 and a learning rate of 0.0001 and 0.00005, respectively. 
We use pretrained model checkpoints taken from the last MIDRC CovidX challenge first place (SwinTransformer) and third place (ConvNext) solution and fine tune them on the challenge data set. 

**Loss:**
We utilize different Loss functions for each model: For SwinTransformer, we use the "Conditional Ordinal Regression for Neural networks (CORN)" loss from 
    Xintong Shi, Wenzhi Cao, and Sebastian Raschka (2021). Deep Neural Networks for Rank-Consistent Ordinal Regression Based On Conditional Probabilities. Arxiv preprint; https://arxiv.org/abs/2111.08851 
For the ConvNext we use a combined loss that is based on the MAE loss and CORN loss, where we scale the CORN loss by a factor of 2.0. 
For the DeiT, we use a combined loss that is based on the MAE loss and CORN loss, where we scale the CORN loss by a factor of 4.0.
In comparsion to Submission 2, we utilize Stochastic Weight Averaging (SWA). 
For all models, we directly predict the combined score for each patient.
