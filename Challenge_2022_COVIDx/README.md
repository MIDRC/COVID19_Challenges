# COVIDx Challenge
**29-Aug-2022**

**CHALLENGE SUMMARY**: The goal of this Challenge is to train an AI/machine learning model in the task of distinguishing between COVID-negative and COVID-positive patients using anteroposterior (AP) view portable chest radiographs (CXRs).

**DATA**: This folder contains several items to get you started in building a training cohort for the MIDRC COVIDx Challenge: 1) a Jupyter Notebook that demonstrates how a training dataset can be created from current publicly available MIDRC data, and 2) as an alternative to running this notebook yourself, we have provided output of the notebook applied to data.midrc.org on August 29, 2022: a) metadata files in tsv format and b) the corresponding Gen3 json manifest file for downloading imaging studies from data.midrc.org.

The inclusion criteria used in the Jupyter notebook for selection of suitable imaging studies were: 1) portable chest radiographs, 2) adult patients, 3) a valid COVID test result within 0 to 14 days prior to the imaging study. We strongly encourage you to further curate the data after downloading to eliminate images that may not be useful for model training (such as processed images etc.). You may wish to modify the notebook to suit your particular needs.  

## File List
* **node_tsvs**: folder containing the raw metadata downloaded from the MIDRC Datacommons
* **COVIDx_Challenge_Study_Selection.ipynb**: Jupyter Notebook that demonstrates how the training dataset was generated
* **COVIDx_training_imagingStudy_COVIDstatus.tsv**: 
* **DX_imaging_studies_plus_covid_tests.tsv**: 
* **Imaging_Covid_Status_Object_Names.tsv**: a list of the object names (and case IDs) for each imaging study that matches the cohort criteria
* **MIDRC_COVIDx_challenge_1_imaging_studies_covid_manifest.json**: manifest file for use with the [`gen3-client`](https://github.com/uc-cdis/cdis-data-client/releases/latest) application to download image data

## Helpful Tips

### Notebook Tips
If you would like to run this notebook on your own to build a cohort, you can do so within the Workspace tab on [data.midrc.org](https://data.midrc.org) or locally on your own computer. Note that some cells in the notebook are specific to running it within the MIDRC Workspace and others are specific to running it locally, as indicated. When doing so locally, you must have Python 3.x and the Jupyter python package installed, and first create and use a MIDRC "API key" credentials file as decribed in the Quick Start Guide (steps 1-4) [1]. Creating an "API key" needs to be done before querying or downloading data. 

If you need assistance with Jupyter Notebooks, please check out some of the references below [2,3]!

### Downloading Tips
When using the [`gen3-client`](https://github.com/uc-cdis/cdis-data-client/releases/latest) application to download cases, we recommend using the following command line settings:

`gen3-client download-multiple --profile=<your profile name> --numparallel=2 --skip-completed --manifest=<location of manifest file> --download-path=<path_for_files>`

Where:
* `<your profile name>` is the name of the profile you created with your API credentials key
* `<location of manifest file>` is the location of the `MIDRC_COVIDx_challenge_1_imaging_studies_covid_manifest.json` file on your local system
* `<path_for_files>` is the directory you would like to use to store your downloaded files

The `numparallel` option will improve the efficiency and performance of your download. The `skip-completed` option will make the application check to see if any of the cases in the manifest file have already been downloaded to the directory you selected. This is extremely helpful if you ever need to restart a download that was interrupted -- just run the same command again, and the application will rebuild your file list and then pick up right where it left off! 

The full download using our json file manifest will consist of **6,649** patients (22,000+ imaging studies) and will require approx. **285 GB** of space (compressed). In previous tests, the full download took **7-8 hours** with a broadband connection.

If you would like more information about how to operate the [`gen3-client`](https://github.com/uc-cdis/cdis-data-client/releases/latest) app, please check out the reference below [4]!


## References
1.  For the MIDRC Quick Start Guide, please see https://www.midrc.org/s/Gen3-MIDRC-QRGv2.pdf
2.  For information on installing the Jupyter Notebook system, please see https://jupyter.org/install
3.  For information on using Jupyter Notebooks, please see https://www.codecademy.com/article/how-to-use-jupyter-notebooks
4.  For information on using the Gen3 Client App to download cases, please see https://gen3.org/resources/user/gen3-client/#5-multiple-file-download-with-manifest
