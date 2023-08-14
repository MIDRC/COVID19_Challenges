# Submission 3: 
**Data:**
1. Only the data provided by the organizer is utilized.
2. We utilized the label for both the left and right extent/density instead of solely using the score.

**Data preprocessing:**
1. Zero-margin areas in the image are eliminated, retaining only rows and columns with non-zero values.
2. Images are resized to 512 x 512, regardless of their original size.
3. The upper and lower thresholds are set as the 5th and 80th percentile of the histogram for each image.
4. Subsequently, each image is normalized to a range of 0 to 1 using the formula (x - min(x)) / (max(x) - min(x)).

**Model:**
1. We employed the Convnext2 pretrained model from timm, specifically the 'convnext_tiny.in12k_ft_in1k_384' initialization.
2. The output is modified to be a 4-element vector representing left/right density/extent, respectively.
Cost function:
1. Mean Squared Error (MSE) is utilized as the cost function, comparing the predicted values with the true values.

**Optimization:**
1. Batch size: 16
2. Learning rate: 1e-5
3. Optimizer: Adam
4. Data augmentation: Zoom in/out, rotation, translation, affine, contrast, horizontal flipping (labels need to be reversed as well).
5. For the first 10 epochs, only the classification head is trained, and after 10 epochs, all parameters are activated for training.

**Cross validation strategy:**
1. The initial 300 examples are designated as the validation set, without implementing cross validation due to time constraints. The model with the best validation results is selected and used for submission.

**Inference:**
1. In the inference phase, we enforce the value range in the expected range: 0-3 for density and 0-4 for extent.
2. Prediction based on multiple model can increase the performance on my validation dataset but didn’t successfully make the uploading. Compared to submission 3, submission 2 did not eliminate zero rows and vectors, nor did it employ warm-up training.