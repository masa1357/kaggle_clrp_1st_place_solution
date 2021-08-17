# Introduction

This repository contains code that was used to generate the winning solution in the commonlit readability prize.

A high level overview outlining this solution can be found at https://www.kaggle.com/c/commonlitreadabilityprize/discussion/257844 .

The solution was developed using Google Colab Notebooks. For best reproducability, you can use these Notebooks hosted on Google Colab:

- data preparation: https://colab.research.google.com/drive/1pUI3MOQncUXLlIOidOkFvOpzOCNLGksm?usp=sharing
- external data preparation: https://drive.google.com/file/d/1Lram_ON5_gB_VrBIMHQqGEA8aiFc0arT/view?usp=sharing
- external data labeling: https://colab.research.google.com/drive/1wQSeD4Swf_z59DqQCG2sSWYAr3ME3SUE?usp=sharing
- model training: https://colab.research.google.com/drive/1DxfF_qp323oibSpevw94mIkv3woRrkVO?usp=sharing
- inference: https://colab.research.google.com/drive/1ijMpPgYVXe4zFus_P56854sbdqUT62H_?usp=sharing

Additionally, one notebook is hosted on Kaggle. This notebook is used to pseudo-label the selected data:

- https://www.kaggle.com/mathislucka/commonlit-two-models

A cleaner version of the inference notebook is included in this repository. The original inference notebook can be found here:

- https://www.kaggle.com/mathislucka/commonlitreadability-r3


# Hardware requirements
During the training, I used Google Colab Notebook instances with 12GB of RAM and a NVidia V100 or P100 GPU with 16 GB of memory. If you experience CUDA out of memory errors, you will probably have to increase GPU memory.

# Preparing the Notebooks

For each notebook, you will have to open the notebook, navigate to the `Constants` section and replace the `BASE_PATH` variable with the path were you've put this project folder.

You will also have to download the models, if you do not want to generate the models from scratch. Models can be downloaded from Kaggle at the following destinations:

- ALBERT_TRAINED_1
    - https://www.kaggle.com/mathislucka/albertxxlarge2models
    - https://www.kaggle.com/mathislucka/albertxxlarge2modelspt2
- ALBERT_TRAINED_2 -> https://www.kaggle.com/mathislucka/albertxxlargelowlr
- ALBERT_TRAINED_3 -> https://www.kaggle.com/mathislucka/albertall
- DEBERTA_TRAINED_1
    - https://www.kaggle.com/mathislucka/debertalarge
    - https://www.kaggle.com/mathislucka/debertalargept2
- DEBERTA_TRAINED_2 -> https://www.kaggle.com/mathislucka/debertalargelowlr
- DEBERTA_TRAINED_3 -> https://www.kaggle.com/mathislucka/debertabootstrap
- ROBERTA_TRAINED_1 -> https://www.kaggle.com/mathislucka/robertalargetwomodels 
- ELECTRA_TRAINED_1 -> https://www.kaggle.com/mathislucka/electralarge
- RIDGE_ENSEMBLE_1 -> https://www.kaggle.com/mathislucka/electraensembling
- RIDGE_ENSEMBLE_2 -> https://www.kaggle.com/mathislucka/hugeensembler



Download each model, unzip them and place them in the `models` folder of this project. You might have to adjust the path to these models when running inference. This can be done in the `Constants` section of the inference notebook.

Put each model into a folder that you also reference in the `Constants` section of the inference notebook. Each model folder should have the following sub-structure (omit the `best` folder for ridge regression models):

```
.
└── model_name/
    ├── model_fold_0/
    │   └── best
    ├── model_fold_1/
    │   └── best
    ├── model_fold_2/
    │   └── best
    ├── model_fold_3/
    │   └── best
    ├── model_fold_4/
    │   └── best
    └── model_fold_5/
        └── best
```

For pseudo-labeling, you will have to take the output from 03_clrp_data_labeling.ipynb and upload it as a Kaggle dataset. Then use this Kaggle notebook to predict the uploaded data: https://www.kaggle.com/mathislucka/commonlit-two-models

Afterwards, you will have to download the predicted data and save it in the following directory "/data/training/predicted/".


# Running the Notebooks
Running a notebook will replace the files that were already generated by this notebook. For the complete solution, you should run these notebooks in the following order:

- 01_clrp_data_prep.ipynb
- 02_clrp_external_data_prep.ipynb
- 03_clrp_external_data_labeling.ipynb
- Kaggle Notebook: https://www.kaggle.com/mathislucka/commonlit-two-models
- 04_clrp_training.ipynb
- 05_clrp_inference.ipynb


# Troubleshooting
If you encounter errors:

1. check if all paths are set correctly
2. deberta models sometimes do not free GPU memory which leads to CUDA out of memory errors. In that case, restarting the notebook instance and picking up training where the error occured usually solves the problem.

# References

Kaggle Resources:
- https://www.kaggle.com/andretugan/commonlit-two-models published by Kaggle User andretugan
- https://www.kaggle.com/andretugan/pre-trained-roberta-solution-in-pytorch published by Kaggle User andretugan
- https://www.kaggle.com/rhtsingh/commonlit-readability-prize-roberta-torch-infer-3 published by Kaggle User rhtsingh
- https://www.kaggle.com/teeyee314/readability-url-scrape published by Kaggle User teeyee314

Other:

Nandan Thakur, Nils Reimers, Johannes Daxenberger, Iryna Gurevych. 2020. Augmented SBERT: Data Augmentation Method for Improving Bi-Encoders for Pairwise Sentence Scoring Tasks.

Jingfei Du, Edouard Grave, Beliz Gunel, Vishrav Chaudhary, Onur Celebi, Michael Auli, Ves Stoyanov, Alexis Conneau. 2020. Self-training Improves Pre-training for Natural Language Understanding.

Qizhe Xie, Minh-Thang Luong, Eduard Hovy, Quoc V. Le. 2019. Self-training with Noisy Student improves ImageNet classification.
