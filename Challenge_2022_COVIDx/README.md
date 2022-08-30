# COVIDx Challenge
**29-Aug-2022**

**CHALLENGE SUMMARY**: The goal of this Challenge is to train an AI/machine learning model in the task of distinguishing between COVID-negative and COVID-positive patients using frontal-view portable chest radiographs (CXRs).

**DATA**: This folder contains several items to get you started in building a training cohort for the MIDRC COVIDx Challenge: 1) a Jupyter Notebook that demonstrates how a training dataset can be created using publicly available MIDRC data, and 2) output of this notebook applied to data.midrc.org on August 29, 2022: a) metadata files and b) the corresponding Gen3 json manifest file for downloading imaging studies from data.midrc.org.

The inclusion criteria used in the Jupyter notebook for selection of suitable imaging studies were: 1) portable chest radiographs, 2) adult patients, 3) a valid COVID test result within 0 to 14 days prior to the imaging study. We strongly encourage you to further curate the data after downloading to eliminate images that may not be useful for model training (such as processed images etc.). You may also wish to modify the notebook to suit your particular needs, e.g., to download a smaller cohort if bandwidth or data storage are concerns.  

If you would like to run this notebook on your own to build a cohort, you can do so within the Workspace tab on data.midrc.org or locally. When doing so locally, you must have Python 3.x (and the Jupyter python package) installed on your local system [2]. In addition, make sure you follow the instructions in the MIDRC Quick Start Guide [1] to set up your MIDRC account. If you would like to run the notebook locally, you will need to create and use a MIDRC "API key" credentials file [1]. If you need assistance with Jupyter Notebooks, please check out some of the references below [2,3]!

References
---
1)  For the MIDRC Quick Start Guide, please see https://www.midrc.org/s/Gen3-MIDRC-QRGv2.pdf
2)  For information on installing the Jupyter Notebook system, please see https://jupyter.org/install
3)  For information on using Jupyter Notebooks, please see https://www.codecademy.com/article/how-to-use-jupyter-notebooks
