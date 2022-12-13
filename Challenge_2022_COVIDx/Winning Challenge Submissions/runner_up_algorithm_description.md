# Submission - codename DropRotx4

<ins>Training data selection and curation:</ins>

Chest radiograph from the data.midrc.org website were used, and downloaded using the Gen3 agent as described in the COVIDx [Challenge GitHub repo](https://github.com/MIDRC/COVID19_Challenges/tree/main/Challenge_2022_COVIDx). Further selection of radiograph was done as follow: 
    • If multiple imaging studies were available for a given patient, only the most recent from the COVID test performed was selected (i.e. the smaller ‘days from covid test’ data point)
    • Secondary Capture Image Storage DICOM types were automatically excluded.
    • Other post-processed images (i.e. modified from original using an image filter) were manually excluded
    • Excessively cropped images (i.e. missing more than around 25% of the lungs) were manually excluded
    • Other atypic image sets, such as very atypic anatomy, very poor contrast, or incorrect image orientation, were also manually excluded
    • If multiples images remained after curation, only one was kept for training, qualitatively based on position of the lungs and diaphragm in the images (i.e. most centered images were favored).

Every possible COVID-positive dataset was downloaded from the online database, which accounted for 1232 chest radiographs after curation. To save time on the curation part, only a similar amount (around 1500) of COVID-negative chest radiographs was downloaded from the database (selected as random) and curated.
To ensure minimal bias, only 1232 COVID-negative chest radiographs were selected for training (at random from all curated COVID-negative images).

<ins>Training data pre-processing:</ins>

Training data was pre-processed using the following method: 
1. Zero padding on each side of the radiograph was cropped
2. Lung segmentation was performed using a UNET + variational encoder model   and weights from R. Selvan et al<sup>1</sup>, the latter being free to use under the MIT licence.

<hr>

<sup>1</sup> R. Selvan et al., Lung Segmentation from Chest X-rays using Variational Data Imputation, ICML Workshop on The Art of Learning with Missing Values, July 2020, https://arxiv.org/pdf/2005.10052.pdf 

<hr>

3. Further steps were performed to ensure that only 2 segmented regions were present (i.e. keeping only the 2 largest segmented ROI, or splitting a single ROI in 2, although such occurrence was very rare).
4. Regions between the 2 segmented lungs was also included in the mask, i.e. that the pixels falling in the trapezoid region delimited by the two apex and two lower tips of the lungs was added to the lung mask in order to include information from the mediastinum and a part of the diaphragm.
5. The whole chest radiograph was cropped around the mask, and pixels falling outside of the lung mask were set to a value of 0.
6. The histogram of the obtained lung segmented image was equalized and its pixel value spread across 256 gray scales.
7. The image was resampled (using linear interpolation) to a resolution of 256x256.
8. The resulting images was converted to a 3-channel image to fit into a densenet model, a further normalization was performed to ensure a mean pixel value of [0.485, 0.456, 0.406] per channel and a standard deviation of [0.229, 0.224, 0.225] per channel.

Data augmentation prior to training was performed by rotating each original image (before the lung segmentation part in step 2 above), of [-15, -7.5, 7.5, 15] degrees. An additional left-right flip of each image (including flipped images) was also conducted for data augmentation.

<ins>Model and training:</ins>

The selected model was a pre-trained densenet-121 model obtained from the torchvision package in python. The last linear classification layer of the model was replaced by a series of linear dense layer (1024 -> 1024 -> 512 -> 256 -> 1) each separated by a ReLU activation layer.

The weights of the original densenet-121 model were frozen so that, during training, loss propagation only occurred on the additional dense layers. A dropout value of 30% was however kept inside the densenet-121 architecture for training.

Model was trained of the dataset described above using a Binary Cross Entropy loss function and the AdamW optimizer in pytorch version 1.8.2 (learning rate = 1e-6, batch size = 10). A NVidia Quadro RTX 6000 (24 Gb memory) was used for training using CUDA library 11.1.

A 5-fold cross-validation was performed to evaluate model performance, tweak parameters, and evaluate overfitting threshold. Final model was trained using 18 epochs on all images using the parameters described above.