# MIDRC XAI Grand Challenge
**28-Oct-2024**

**PROBLEM DEFINITION**: This MIDRC XAI Challenge addressed the critical unmet need of making medical image analysis AI models explainable. Existing saliency map techniques, such as Grad-CAM and LIME, offer visual explanations of AI decision-making processes but have notable shortcomings; 1) these techniques often lack consistency and reliability, sometimes producing different explanations for similar inputs, 2) different techniques can offer drastically different 'explanations' for output of the same AI model for the same input, 3) various techniques can be impacted by minor perturbations in the input data, leading to variations in the generated saliency maps that undermine their trustworthiness [1], and 4) saliency maps only typically highlight regions of an image without providing clear, interpretable insights into why those regions are significant. 

The black-box nature of these AI models, combined with the opaque explanations provided by current saliency map techniques, hinders their potential utility in clinical settings, where transparency and interpretability are paramount for gaining the trust of healthcare professionals. Thus, there is a pressing need for more robust, reliable, and interpretable explainability methods in AI-driven medical image analysis.

**CHALLENGE SUMMARY**: The XAI Challenge aimed to advance explainable AI for medical image analysis. Participants were tasked with developing and training explainable artificial intelligence/machine learning (AI/ML) model(s) in the task of classifying frontal-view MIDRC portable chest radiographs (CXRs) for the presence of lung opacities associated with any type of pneumonia for evaluation against the reference standard for the validation and test datasets. The AI/ML output for each CXR was 1) a likelihood that the patient presented with pneumonia of any type, and 2) an 'explainability' map, interpretable as the probability of presence of lung opacity at each pixel (of the same size as the input image). This Challenge used Docker as a containerization solution.

**REPOSITORY CONTENTS**: For each of the top 15 participants, this repository contains:
* A zip archive with everything needed to run the trained model: code, weights, Dockerfile, and requirements
* A description of the training data, training approaches, and model architecture, including literature references


## References
1. https://doi.org/10.1148/ryai.2021200267
