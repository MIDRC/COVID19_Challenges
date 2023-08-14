The training dataset provided contained 2598 images from 2078 patients. First, we kept only one image for each patient, which was manually selected as the intended examination: discarding post-processed images and choosing the one that shows most of the lungs. 

To train our models, we used Pytorch and Pytorch Lightning.

We first trained an efficient net-B4 with holdout validation (20% of data for validation) on the 2078 images using a learning rate of 1e-3 with stochastic depth=0.2. The images were loaded using the pydicom library. First, we normalized all images between 0 and 4080. Values lower than 5 and higher than 4000 were set to the mean (2040) in order to smooth annotations on the image (such as L and R indicating the sides). Then we normalized the values between 0 and 255 and converted the image to RGB using the PIL library, which was the model's input. We used the following data augmentation for training: rotation of up to 20 degrees, translation and scaling of 10% of image size, and altered the brightness and contrast up to 60% using the transforms for torchvision. All images were resized to 512x512 prior to model input.

We used the trained checkpoint to make predictions on the Chexpert dataset (we discarded all lateral view images, keeping only AP or PA view), which contains 224,316 chest X-ray images. We applied the same preprocessing and transforms on this dataset (aside from using the pydicom library, as the images were already in PNG). 

We downloaded extra data from the MIDRC database, including only portable CXR images from COVID-19 patients. All images were manually inspected, removing post-processed images and selecting only one per patient, resulting in 4562 images. The same inference process applied to Chexpert was applied to these images, which we will call "MIDRC extra images".

We pre-trained an efficient net-b7 from scratch using the images from Chexpert. We used a learning rate of 2e-3 with stochastic depth=0.2. We applied the same preprocessing,  data augmentation, and transforms on this dataset (aside from using the pydicom library, as the images were already in PNG). We used holdout validation (20% of data for validation) and achieved ~0.98 kappa quadratic score. 

We then fine-tuned this pre-trained efficient net-b7 on the MIDRC extra images, using the original 2078 selected images from the training dataset provided as validation data. In this fine-tuning process, we used a lower learning rate of 1e-4 and a stochastic depth=0.8 in order to increase the generability of the model. 

This fine-tuned checkpoint of the efficient net-b7 is the one that we submitted to make the final predictions. Since it is impossible to have mRALE values of 19, 22, and 23, we also post-processed the predictions. Predictions lower than 0 are set to 0, higher than 24 are set to 24, predictions between 18 and 19 are set to 18, between 19 and 20 are set to 20, between 21 and 22.5 are set to 21, and between 22.5 and 24 are set to 24. 
