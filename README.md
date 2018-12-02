# Natural-Language-Processing-for-predicting-hospital-readmission
A predictive model built using Natural Language Processing (NLP) to identify patients at the risk of readmission.

## Technical Requirements 
* Docker
* Ubuntu 16.04 or greater 
* Azure HDINSIGHTS cluster 

In order to re-run this code it is in the best interest of the user to use the same docker image. 
### Pull Docker Image 
sudo docker pull ucsddse230/cse255-dse230

### Run Docker Image 
docker run -it -m 6900 -p 8889:8888 -v /local/path/project:/home/ucsddse230/work ucsddse230/cse255-dse230 /bin/bas



## Data 
Using the MIMIC-III database
https://physionet.org/works/MIMICIIIClinicalDatabase/files/

**ADMISSIONS**: 
Contains unique hospitalizations for each patient in the database. It has 58,976 unique
admissions of 46,520 patients. 5,854 admissions have a date of death specified.

**NOTEEVENTS**: 
Contains deidentified notes, such as ECG, radiology reports, nursing and physician notes,
discharge summaries for each hospitalization. It has 2,083,180 unique notes.


## Processing Phases
Due to the limited resources for our project we divided our processng into 3 phases **Pre-Azure** ,**Azure**,**Post-Azure** if rebuilding this from scratch please note that the notebook usage is **sequantial**.

**Pre Azure**

Minor Preprocessing, change delimiter. 

**Azure**

Data Processing, tokenize features(heavy lifting) and  create label.

**Post Aure**

Vectorize features, implement ML models 

## Project Paper

Results and more detailed methodologies can be found here

https://drive.google.com/file/d/12XnBI6MXQLoGDVw4XwGe2ebAtybvt62v/view?usp=sharing


