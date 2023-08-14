**Motivation**
We proposed a self-supervised machine learning method to automatically rate the severity of pulmonary edema of the frontal chest X-ray radiographs (CXR) of COVID-19 viral pneumonia with the modified radiographic assessment of lung edema (mRALE) scoring system.

**Model and Training**
The new model was first optimized with the simple Siamese network (SimSiam) architecture where a ResNet-50 pre-trained by ImageNet database was used as the backbone. The encoder projected 
a 2048-dimension embedding as representation features to a downstream fully connected deep neural network for mRALE score prediction.

**Inference**
A 5-fold cross-validation with 2,599 frontal CXRs was used to examine the new model's performance with comparison to a non-pretrained SimSiam encoder and a ResNet-50 trained from scratch. The mean absolute error (MAE) of the new model is 5.05 (95%CI 5.03-5.08), the mean squared error (MSE) is 66.67 (95%CI 66.29-67.06), and the Spearman's correlation coefficient (Spearman ρ) to the expert-annotated scores is 0.77 (95%CI 0.75-0.79).

**Conclusion**
All the performance metrics of the new model are superior to the two comparators (P<0.01), and the scores of MSE and Spearman ρ of the two comparators have no statistical difference (P>0.05). We conclude that the self-supervised contrastive learning method is an effective strategy for mRALE automated scoring. It provides a new approach to improve machine learning performance and minimize the expert knowledge involvement in quantitative medical image pattern learning.

[Complete and detailed description of algorithm](ninth_place_algorithm_description.pdf)