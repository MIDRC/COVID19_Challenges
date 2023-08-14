**Pretraining**
All models were first pretrained on RSNA Pneumonia Detection Challenge dataset using the 3 classification labels (normal, not normal/no opacity, opacity).

**Clean Data**
For studies with more than 1 image, manually select best image and exclude others.

**Model 1**
Trained on whole images. Predict 4 classes (density left, density right, extent left, extent right) via regression model. Sigmoid activation function was applied to output and multiplied by max value of class. To calculate mRALE, predictions were multiplied and added together (density left * extent left) + (density right * extent right), then rounded to the nearest integer.

**Model 2**
First, about 1,000 images were manually annotated with lung bounding boxes. Regression-type model (EfficientNet-B4) was used to predict coordinates of right and left lung bounding boxes. Then, individual lungs were extracted from each image. Similar training to model 1, except predict only 2 classes (density and extent) for each lung. mRALE was calculated after predictions for both lungs were obtained, as above.

**Training**
Image size: 512 x 512 (grayscale) - whole images, 512 x 320 - lung images
Data augmentation: RandAugment (flip, brightness/contrast, shift/scale/rotate, random noise)
Backbones: EfficientNet-B4, ConvNeXt-tiny, generalized mean pooling, dropout 0.2
Optimizer: AdamW
Loss: Average of L2 (MSE) and L1 (MAE) losses
Learning rate: 3.0e-5 with cosine annealing
Epochs: 10
Batch size: 16

**Inference**
Model 1 - Infer on whole resized image (2 ConvNeXt-tiny, 2 EfficientNet-B4)
Model 2 - Extract bounding boxes for each lung, infer on each resized lung image (1 EfficientNet-B4 for bounding box extraction; 2 ConvNeXt-tiny, 2 EfficientNet-B4 for mRALE prediction)
Ensemble - average all predictions, then round to nearest integer