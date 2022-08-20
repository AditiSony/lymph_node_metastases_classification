# **Use of Deep Learning for Automated Detection and Multi- Stage Classification of Breast Cancer Metastases from Whole-Slide Images**

Welcome to my project on automation of detection and multi-class classification of metastases type from Whole Slide Images (WSI)! This file introduces you to how to reproduce work done and the requirements.
## **Folder structure:**
•	Google Colab jupyter notebook titled “SonyA_200757766_CSC8639_Colab_notebook.ipynb” is the main script which can be run for reproducibility of results.
•	CLAM-master folder contains different folders with all the other scripts.

## **Installations and Environment Set-up:**
All work is done in Google Colab using Python3 as programming language and its various libraries. Please note that all files as well as data were saved in my personal Google Drive, hence you’ll need to save all the folders and files alongwith data files in your own Google Drive and mount it first before running any scripts.
All installations and set-ups can be done in Colab by running the first section titled “Initial Installation and Set-up” in the notebook “SonyA_200757766_CSC8639_Colab_notebook.ipynb”.
File named “clam.yaml “ under the folder CLAM-master > CLAM-master > docs is the environment configuration file which provides a full list of the libraries and their versions used. If you are running on a local machine, then please ensure you have all the required packages and set-ups in your system before running any other scripts.

## **Dataset:**
CAMELYON17 dataset is a publicly available dataset under the context of CAMELYON17 Grand Challenge and can be downloaded from the link- https://drive.google.com/drive/folders/0BzsdkU4jWx9BaXVHSXRJTnpLZU0?resourcekey=0-tyfGzeoOMAWlP_ogPt_4pw&usp=sharing . All WSIs are expected to be stored under a folder named DATA_DIRECTORY.
Please note the dataset is very large (>1TB) and consists of a total of 1000 whole-slide images (WSIs) of sentinel lymph node from 5 different medical centers. Data is divided into training and test sets with 20 patients from each center in both sets. There are 5 slides per patient.  However only 500 slides from the training set have ground truth labels and test set is unlabelled. Metastases class are available as slide level labels in a csv file.

## **Scripts:**
a)	**Image Segmentation & Patch Creation**- Cell titled “Image Segmentation & Patch Creation” in the Colab notebook can be run directly. User will need to change the filepath of script named “create_patches_fp.py” to point to CLAM-master > CLAM-master > create_patches_fp.py,  --source to point to the DATA_DIRECTORY, --save_dir to save results provide filepath to RESULTS_DIRECTORY.

b)	**Feature Extraction**- User will first need to download the “process_list_autogen.csv” file from “RESULTS_DIRECTORY” from previous step, remove the extensions .tif and save and upload it under CLAM-master > CLAM-master> process_list_autogen.csv.
Cell titled “Feature Extraction” in the Colab notebook can be run directly. User will need to change the filepath of script named “extract_features_fp.py” to point to CLAM-master > CLAM-master >  extract_features_fp.py, --data_h5_dir to point to “RESULTS_DIRECTORY” from previous step, --data_slide_dir to point to “DATA_DIRECTORY”, --csv_path to point to “process_list_autogen.csv”, --feat_dir to point to “FEATURES_DIRECTORY” to save results.
2 GPUs are used however it can be changed as per user’s needs.

c)	**Train, Validation, Test Split**- Label files will need to be prepared by user from the slide-level labels downloaded alongwith the dataset as csv file. The labels.csv file needs to have 3 columns namely case_id, slide_id and label. Filepath to this labels.csv file will need to be updated within the script “create_splits_seq.py” under CLAM-master > CLAM-master folder.
Cell titled “Train, Validation, Test Split” in the Colab notebook can be run directly. User will need to change the filepath of script named “create_splits_seq.py” to point to the updated file.

d)	**Training**- First, user will need to move folders named “h5_files” and “pt_files” created under “FEATURES_DIRECTORY” in the previous step b) in a folder named DATA_ROOT_DIR > tumor_subtyping_resnet_features.
Next, user will need to update within the script “main.py” under folder CLAM-master > CLAM-master to point to the filepath to the labels.csv file.
Then, Cell titled “Training” in the Colab notebook can be run directly. User will need to change the filepath of script named “main.py” to point to the updated file, --split_dir to point to the splits > task_2_tumor_subtyping_60 folder created in the previous step, --data_root_dir to point to the DATA_ROOT_DIR folder.

e)	**Evaluation**- Though training already covers 20% of the dataset as the test set, user can however further do evaluation on a separate test set if they wish so. For this user will need to update within the script “eval.py” under folder CLAM-master > CLAM-master to point to the filepath to the labels.csv file. Then, cell titled “Evaluation” in the Colab notebook can be run directly. User will need to change the filepath of script named “eval.py” to point to the updated file, --results_dir to point to the results folder created in the previous step, --data_root_dir to point to the DATA_ROOT_DIR folder.

## **Acknowledgement:**
Pipeline built by Lu et al. in their work (GitHub - mahmoodlab/CLAM: Data-efficient and weakly supervised computational pathology on whole slide images - Nature Biomedical Engineering) has been used in this project work as the benchmark and foundation work to serve as the baseline. Code for it is publicly available under the GPL V3 License and is available for non-commercial academic purposes.
