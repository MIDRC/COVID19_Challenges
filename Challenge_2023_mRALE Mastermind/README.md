# mRALE Mastermind Challenge
**28-Apr-2023**

**CHALLENGE SUMMARY**: The goal of this Challenge is to train an AI/machine learning model in the task of predicting COVID severity on portable chest radiographs (CXRs).

**DATA**: This folder contains several items to get you started in building a training cohort for the MIDRC mRALE Mastermind Challenge: 

To provide a reference standard assessment of disease severity on chest radiographs, a simplified version of the Radiographic Assessment of Lung Edema (RALE) score is provided. This grading scale was originally validated for use in pulmonary edema assessment in acute respiratory distress syndrome and incorporates the extent and density of alveolar opacities on chest radiographs. The grading system is relevant to COVID-19 patients as the chest radiograph findings tend to involve multifocal alveolar opacities, and many hospitalized COVID-19 patients develop acute respiratory distress syndrome. Here we use a modified RALE (mRALE) score. Each lung is assigned a score for the extent of involvement by consolidation or ground glass/hazy opacities (0 = none; 1 ≤ 25%; 2 = 25%–50%; 3 = 51%–75%; 4 = >75% involvement). Each lung score is then multiplied by an overall density score (1 = hazy, 2 = moderate, 3 = dense). The sum of scores from each lung is the mRALE score. Thus, a normal chest radiograph receives a score of 0, while a chest radiograph with complete consolidation of both lungs receives the maximum score of 24. mRALE differs from the original RALE score in that the lungs are not divided into quadrants [1].

For the sake of completeness, the original 'raw' radiologist annotations are provided in the mRALE assessment data file. However, participants' AI/ML models should predict **only** the mRALE score derived from these annotations (last column in the annotation file).

The inclusion criteria used in the selection of suitable imaging studies were: 1) portable chest radiographs, 2) adult patients, 3) CXR acquisition within 2 days after a positive COVID test, and 4) CXR exam of sufficient image quality to allow for radiologist annotation. Note that the MIDRC data portal continues to make new imaging studies publicly available on a regular basis; as such, there may be other unannotated portable CXRs currently available in the data portal that satisfy these criteria but were not available at the time of the radiologist annotation project.

## File List
* **annotated_images_manifest_2079.json**: manifest file for use with the [`gen3-client`](https://github.com/uc-cdis/cdis-data-client/releases/latest) application to download image data
* **MIDRC mRALE Mastermind Training Annotations_2079_20230428.csv**: the mRALE assessment data for each imaging study that matches the cohort criteria

## Helpful Tips

### Downloading Tips
When using the [`gen3-client`](https://github.com/uc-cdis/cdis-data-client/releases/latest) application to download cases, we recommend using the following command line settings:

`gen3-client download-multiple --profile=<your profile name> --numparallel=2 --skip-completed=true --manifest=<location of manifest file> --download-path=<path_for_files>`

Where:
* `<your profile name>` is the name of the profile you created with your API credentials key
* `<location of manifest file>` is the location of the `annotated_images_manifest_2079.json` file on your local system
* `<path_for_files>` is the directory you would like to use to store your downloaded files

The `numparallel` option will improve the efficiency and performance of your download. The `skip-completed` option will make the application check to see if any of the cases in the manifest file have already been downloaded to the directory you selected. This is extremely helpful if you ever need to restart a download that was interrupted -- just run the same command again, and the application will rebuild your file list and then pick up right where it left off! Interrupted downloads may also happen if MIDRC is experiencing high usage, and you may receive some "503 errors" during your download. Simply run this command again, and the client will attempt to download all of the cases that were missed the first time.

The full download using our json file manifest will consist of **2,079** imaging studies and will require approx. **30 GB** of space (compressed). In previous tests, the full download took **45-60 minutes** with a broadband connection.

**NOTE:** You may receive an error like the one below during your download. This is due to a known bug in the client and a fix is currently in progress. It does not actually impact the download process and all of the cases in the manifest will indeed download correctly (the error itself is an error), so you may safely ignore this error message.

```
2023/04/28 08:37:52 2 files have encountered an error during downloading, detailed error messages are:
2023/04/28 08:37:52 Got a non-200 or non-206 response when making request to URL associated with GUID dg.MD1R/8da932f4-e28e-4eba-b946-acd541ac5561
 HTTP status code for response: 416
2023/04/28 08:37:52 Got a non-200 or non-206 response when making request to URL associated with GUID dg.MD1R/4009d590-3055-403b-aac3-f293b9e980eb
 HTTP status code for response: 416
 ```

If you would like more information about how to operate the [`gen3-client`](https://github.com/uc-cdis/cdis-data-client/releases/latest) app, please check out the reference below [2,3]!


## References
1.  Li MD, Arun NT, Gidwani M, Chang K, Deng F, Little BP, Mendoza DP, Lang M, Lee SI, O'Shea A, Parakh A, Singh P, Kalpathy-Cramer J. Automated Assessment and Tracking of COVID-19 Pulmonary Disease Severity on Chest Radiographs using Convolutional Siamese Neural Networks. Radiol Artif Intell. 2020 Jul 22;2(4):e200079. [doi: 10.1148/ryai.2020200079](https://doi.org/10.1148/ryai.2020200079). PMID: 33928256; PMCID: PMC7392327.
2.  For the MIDRC Quick Start Guide, please see https://www.midrc.org/s/Gen3-MIDRC-QRGv2.pdf
3.  For information on using the Gen3 Client App to download cases, please see https://gen3.org/resources/user/gen3-client/#5-multiple-file-download-with-manifest
