# MIDRC mRALE Mastermind Challenge
Submission #1 description – codename 4Models_20230705
Mathieu Goulet, Team_CRIC

**Training data selection and curation:**
Chest radiographs from the data.midrc.org website were exclusively used, and downloaded using the Gen3 agent as described in the COVIDx Challenge GitHub repo. Further selection of radiographs was done as follow: 
- Secondary Capture Image Storage DICOM types were automatically excluded.
- Any chest radiograph with a mention in the column “quality_issue” of the annotation file  (e.g. missing lung) was also excluded
- Other post-processed images (i.e. modified from original using an image filter) were manually excluded
- Excessively cropped images (i.e. missing more than around 25% of the lungs) were manually excluded
- If multiples images remained after curation for a given study, only one was kept for training, qualitatively based on position of the lungs and diaphragm in the images (i.e. most centered images were favored).
Every annotated COVID-positive dataset was downloaded from the online database, which accounted for 2041 chest radiographs after curation.

**Training data pre-processing:**
Training data was pre-processed using the following method: 
1. Zero padding on each side of the radiograph was cropped
2. Lung segmentation was performed using a UNET + variational encoder model and weights from R. Selvan et al<sup>1</sup>, the latter being free to use under the MIT licence.
3. Further steps were performed to ensure that only 2 segmented regions were present (i.e. keeping only the 2 largest segmented ROI, or splitting a single ROI in 2, although such occurrence was very rare).
4. Regions between the 2 segmented lungs was also included in the mask, i.e. that the pixels falling in the trapezoid region delimited by the two apex and two lower tips of the lungs was added to the lung mask in order to include information from the mediastinum and a part of the diaphragm.
5. The mask was split into a Left lung and Right lung mask. The frontier between the two masks was taken as the line passing through the midpoint of the two lung apex and the midpoint of the two lower lung tips described in (4)
6. The whole chest radiograph was cropped around each the mask, and pixels falling outside each lung mask were set to a value of 0.
7. The histogram of each obtained lung segmented image was equalized and its pixel value spread across 256 gray scales.
8. Each image was resampled (using linear interpolation) to a resolution of 256x256.
a. Right lung images where stretch horizontally or vertically to ensure their fill the whole matrix width and height
b. Left lung image were not stretch, that is keeping their original width/height ratio
9. Each resulting images was converted to a 3-channel image to fit into a densenet model, a further normalization was performed to ensure a mean pixel value of [0.485, 0.456, 0.406] per channel and a standard deviation of [0.229, 0.224, 0.225] per channel.
Data augmentation prior to training was performed by rotating each original image (before the lung segmentation part in step 2 above), of [-20, -15, -10, -5, 5, 10, 15, 20] degrees. 

<sup>1</sup> R. Selvan et al., Lung Segmentation from Chest X-rays using Variational Data Imputation, ICML Workshop on The Art of Learning with Missing Values, July 2020, https://arxiv.org/pdf/2005.10052.pdf 


**Model and training:**
Four different models were trained to respectively infer the mRALE extent and density of each lung (i.e. density-left, density-right, extent-left, extent-right). The problem was treated as a regression problem (i.e. both extent and density was assumed to fall on a single scale from 0 to their respective max value).
The selected model for each was a pre-trained densenet-121 model obtained from the torchvision package in python. The last linear classification layer of the model was replaced by a series of linear dense layer (1024 -> 1024 -> 512 -> 256 -> 1) each separated by a ReLU activation layer. A sigmoid activation layer was also added at the end of the network to keep the output between 0 and 1.
The weights of the original densenet-121 model were frozen so that, during training, loss propagation only occurred on the additional dense layers. A dropout value of 30% was however kept inside the densenet-121 architecture for training.
Expected value of each mRALE sub score (extent and density numerical value) was normalized to fall between 0 and 1 (that is, dividing the extent score by 4 and the density score by 3).
Model was trained of the dataset described above using a Mean Square Error Loss function and the AdamW optimizer in pytorch version 1.13.1 (learning rate = 1e-6, batch size = 10). An NVidia Quadro RTX 6000 (24 Gb memory) was used for training using CUDA library 11.7.
A 5-fold cross-validation was performed to evaluate models performance, tweak parameters, and evaluate overfitting threshold. Final models were trained using 23 to 36 epochs (depending on the mRALE sub-score evaluated) on all images of the training set using the parameters described above.
Final inference
Using the four models described previously, a score between 0 and 1 was obtained for each mRALE sub-scores. Each score was multiplied by their respective normalisation value (4 for extent, 3 for density), to obtain a floating point mRALE sub-score value.
Each side mRALE score was obtained using the formula: 
- Left mRALE = (extent-left)*(density-left)
- Right mRALE = (extent-right)*(density-right) 
Each (floating-point) Left/Right mRALE score obtained was ten re-adjusted (re-interpolated) using a linear relation derived from the training data (that is, to account for some systematic underestimation or overestimation of the individual lung scores).
Each side mRALE score was then rounded up to the nearest integer, with the following exceptions:
- As a score of 5 is impossible numerically, all mRALE side score falling between 4.5 and 5.5 were rounded to the nearest of 4 or 6.
- As a score of 7 is impossible numerically, all mRALE side score falling between 6.5 and 7.5 were rounded to the nearest of 6 or 8.
- As a score of 10 or 11 is impossible numerically and perfect scores of maximal density and extent were a bit disadvantaged using the methodology, all mRALE side scores equals or greater than 9.5 was rounded up to 12.
The total mRALE score was then simply obtained using the following : 
- Total mRALE score = Left mRALE + Right mRALE