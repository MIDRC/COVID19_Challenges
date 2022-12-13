**Approach:** small densenets, pretrained together with data-efficient vision transformers (DEIT) and a small ConvneXt on a vast amount of chest x-rays

**Data:**
We use a subset of the data set provided by the midrc challenge hosts.
We only use one x-ray per patient and filter for the smallest duration between covid test and data study date (days_from_test_to_study) if there is more than one image for one patient. 
Furthermore, we manually filter out 241 acquisitions of bad quality, wrong projections (lateral views for example) or x-rays with string differences in the contrast characteristics (post processing).
This leaves us with a total amount of 6555 x-rays.
We sample 189 images with a positive covid label together with 190 images without covid label as an independent and balanced test set. 
These test samples are used to evaluate the modes on a balanced test set and are fused to the training set within each fold for the final submission.
With the remaining 6176 samples a 5-fold cross-validation is applied with 4941 samples for training and 1235 samples for validation, respectively for each fold.

**Deep Learning models:**
We us an ensemble of 5 different folds for Densenet121 at two different resolutions (224x224 and 320x320) and ConvNext_small and the transformer-based Deit_base at a resolution of 224x224 resulting in 20 checkpoints in total. 
During inference, all individual predictions are averaged. 

The models include: 
- DenseNet 121 224 px
- DenseNet 121 320 px
- ConvNext_small 224 px
- Deit_base 224 px

While the DenseNets are trained with Adam, for Deit_base, and ConvNext_small we use AdamW.
For Densenet 121 a learning rate of 0.001, a weight decay of 1e-5 and a batch size of 64 us used.
For ConvNext_small a learning rate of 9.4e-6, a weight decay of 0.01 and a batch size of 128 us used.
Lastly, for Deit_base a learning rate of 6.9e-6, a weight decay of 0.03 and a batch size of 64 us used.       

We decrease the learning rate during training by cosine annealing and use early stopping based on the Cross entropy loss on the validation set. All hyper-parameters are tuned based on the validation and (balanced) test set performance.

For Densenets, we utilize checkpoints from the torch x-ray vision repository. Here the models are pre-trained on 6 different large-scale x-ray data sets at a resoluiton of 224x224 px.
For data augmentation with DenseNets, we use random rotation of up to 45 degree, random scale (factor 0.15) and random translation (factor 0.15).
For Data augmentation with Deit and ConvNext,  we use randaugment and random erasing (p=0.1).

All images are resized to a resolution of 224x224 px or 320x320 px depending on the model.
