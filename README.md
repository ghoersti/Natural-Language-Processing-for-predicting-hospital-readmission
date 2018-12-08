

# Software/data 
* This guide is intended for **Linux system only**


---
## Required software/ Applications
Below are links/guides  to download/install the necessary software
* [Anaconda Python 3.6+ ](https://www.anaconda.com/download/#linux)
* [Docker on Linux installation Guide](https://runnable.com/docker/install-docker-on-linux)
* [Create HDINSIGHTS Spark cluster with pyspark](https://docs.microsoft.com/en-us/azure/hdinsight/spark/apache-spark-jupyter-notebook-kernels)
* [Jupyter on HDINSIGHTs ](https://docs.microsoft.com/en-us/azure/hdinsight/spark/apache-spark-jupyter-notebook-kernels)

In order to re-run this code it is in the best interest of the user to use the same docker image. Running below command will pull docker image with Ubuntu 16.04 and a clean Anaconda 4.3 with python 3.6, jupyter 5.4, spark 2.2 installation. 

**Pull Docker Image** 

>`sudo docker pull ucsddse230/cse255-dse230`

**Run Docker Image** 

>```docker run -it -m 6900 -p 8889:8888 -v /local/path/project:/home/ucsddse230/work ucsddse230/cse255-dse230 /bin/bas```

**Extract code**

Extract "Team13_NLP.tar.gz" and copy final_project folder to /local/path/project so that you can see it inside docker under path     "/home/ucsddse230/work"

From within the docker container

Run:
>`pwd`
you should be at path "/home/ucsddse230/work"


Run : 

>```jupyter notebook```

Go to http://localhost:8889

Once inside Jupyter Notebook you can run all the notebooks as described below.


## Final Model validation
To run the final model on test data please go to post_azure and execute "Final_Model_Validation.ipynb". This step can be completed without downloading data from the MIMIC III source website.

*****Before you run the notebook, 
1. Please make sure MyMLP.pth file exists in "final_project/dl_model" folder. If not , please download final saved model from the link and copy it into "final_project/dl_model" folder. 
    https://drive.google.com/open?id=1cP56sytzc3uKvZVF9j2Ia0RAvnvvAN0T

2. please make sure that below folders exist in "final_project/post_azure":

    testing_features.parquet
    
    training_features.parquet
---

## Data Access
As the data used for this project requires CITI certification please follow below instructions to get access to data.

[Instructions for MIMIC-III access](https://mimic.physionet.org/gettingstarted/access/)

---


## Data 
Using the MIMIC-III database
https://physionet.org/works/MIMICIIIClinicalDatabase/files/

After recieving access download these files

**ADMISSIONS**: 
Contains unique hospitalizations for each patient in the database. It has 58,976 unique
admissions of 46,520 patients. 5,854 admissions have a date of death specified.

**NOTEEVENTS**: 
Contains deidentified notes, such as ECG, radiology reports, nursing and physician notes,
discharge summaries for each hospitalization. It has 2,083,180 unique notes.

---

## Usage

The usage for this project  is divided into **three phases:**

1. **Pre-azure** : Minor Preprocessing, change delimiter. 
2. **Azure** : Data Processing, tokenize features(heavy lifting) and  create label.
3. **Post_azure** : Vectorize features, implement ML models 

---

### Pre_Azure
> This must be run first follow instructions in the notebook `cse6250_NLP_Pre_process.ipynb`

**inputs:**
```python
'idc9_short.txt', 'NOTEEVENTS.csv'
```

**outputs:**
```python
'notes_discharge_pd.csv' , 'ADMISSIONS.CSV'  and 'IDC9_filter.csv'
```



**contents:**
* ICD-9-CM-v32-master-descriptions
> This folder contains all of the ICD9 diagnosis and procedure codes and words downloaded from [here](https://www.cms.gov/Medicare/Coding/ICD9ProviderDiagnosticCodes/codes.html)
>this is the file used `idc9_short.txt`
* cse6250_NLP_Pre_process.ipynb
> 1. Preprocessing done to allow readability by spark
> 2. creation of ICD9 Filter

---

### Azure

**create labels , prepare date , create tokens**

**Inputs:**
> Upload these files to your azure blob storage for the HDINSIGHTS cluster
```python
'notes_discharge_pd.csv' , 'ADMISSIONS.CSV'  and 'IDC9_filter.csv'
```

**Outputs:**
> Download these files from your azure blob storage for the HDINSIGHTS cluster
to `/home/ucsddse230/work/azureoutputs`
```python
'final_tokens_with_text.parquet'
```

**Contents:**

* CSE6250_azure.ipynb



---


### Post_Azure

**Contents:**
* Reconstruct from orginal text.ipynb

>Inputs: 
```Python
'final_tokens_with_text.parquet'
```

>Outputs: 
```Python
"testing_features.parquet","training_features.parquet"
```

* local_modeling_with_azure_features.ipynb

>Inputs:`
```Python
"testing_features.parquet","training_features.parquet"
```

>Outputs: 
```Python
'MyMLP.pth'
```

* utils.py

---

## Final Directory Structure


```python
final_project
├── azure
│   └── CSE6250_azure.ipynb
├── azureoutputs
│   └── final_tokens_with_text.parquet
│       ├── part-00000-dad216ad-f371-44f0-8aaa-1dbcd7c5242b-c000.snappy.parquet
│       └── _SUCCESS
├── dl_model
│   └── MyMLP.pth
├── post_azure
│   ├── Final_Model_Validation.ipynb
│   ├── local_modeling_with_azure_features.ipynb
│   ├── Reconstruct from orginal text.ipynb
│   ├── testing_features.parquet
│   │   ├── part-00000-0a590164-62ce-4aff-aaaf-3baa3f3c18b5-c000.snappy.parquet
│   │   ├── part-00001-0a590164-62ce-4aff-aaaf-3baa3f3c18b5-c000.snappy.parquet
│   │   ├── part-00002-0a590164-62ce-4aff-aaaf-3baa3f3c18b5-c000.snappy.parquet
│   │   ├── part-00003-0a590164-62ce-4aff-aaaf-3baa3f3c18b5-c000.snappy.parquet
│   │   ├── part-00004-0a590164-62ce-4aff-aaaf-3baa3f3c18b5-c000.snappy.parquet
│   │   ├── part-00005-0a590164-62ce-4aff-aaaf-3baa3f3c18b5-c000.snappy.parquet
│   │   └── _SUCCESS
│   ├── training_features.parquet
│   │   ├── part-00000-d19cbfcd-5fb9-4715-a708-739e81e14410-c000.snappy.parquet
│   │   ├── part-00001-d19cbfcd-5fb9-4715-a708-739e81e14410-c000.snappy.parquet
│   │   ├── part-00002-d19cbfcd-5fb9-4715-a708-739e81e14410-c000.snappy.parquet
│   │   ├── part-00003-d19cbfcd-5fb9-4715-a708-739e81e14410-c000.snappy.parquet
│   │   ├── part-00004-d19cbfcd-5fb9-4715-a708-739e81e14410-c000.snappy.parquet
│   │   ├── part-00005-d19cbfcd-5fb9-4715-a708-739e81e14410-c000.snappy.parquet
│   │   └── _SUCCESS
│   └── utils.py
├── pre_azure
│   ├── cse6250_NLP_Pre_process.ipynb
│   ├── ICD-9-CM-v32-master-descriptions
│   │   ├── CMS32_DESC_LONG_DX (copy).txt
│   │   ├── CMS32_DESC_LONG_DX.txt
│   │   ├── CMS32_DESC_LONG_SG.txt
│   │   ├── CMS32_DESC_LONG_SHORT_DX.xlsx
│   │   ├── CMS32_DESC_LONG_SHORT_SG.xlsx
│   │   ├── CMS32_DESC_SHORT_DX.txt
│   │   ├── CMS32_DESC_SHORT_SG.txt
│   │   └── idc9_short.txt
│   ├── IDC9_filter.csv
│   └── idc9_short.txt
└── README.md
└── Usage_instructions.ipynb

```
