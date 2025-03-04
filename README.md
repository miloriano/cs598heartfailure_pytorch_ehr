# Predictive Modeling on Electronic Health Records(EHR) using Pytorch
***************** 

**Overview**

We decide to go with the paper \textbf{Endpoint prediction of heart failure using electronic health records} \cite{chu_dong_huang_2020}. This work is trying to predict the endpoint of heart failure prediction using electronic health records using a Recurrent Neural Network with two different learning strategies – adversarial learning and multi-task learning.. Unlike other methods, instead of just using the endpoints as target variables the paper also predicts the patients’ feature vectors as an auxiliary task and uses that information to optimize the HF endpoint prediction.

We reached out to fellow students who chose the same paper and was told they reached out to the authors and the Chinese Hospital since our classmate is Chinese and speaks their language, however the data retrieval was denied. As per approval from TA, we went ahead and got a comparable dataset from the public domain, which is where this forked data came from.

We re-used the data structure and data loader from the original code -- EHRDataloader.py. We wrote our own notebooks namely the following:

* CustomCode_Riano_Kalidas_RETAIN.ipynb
* CustomCode_Riano_Kalidas_RNN_GRU.ipynb
* CustomCode_Riano_Kalidas_RNN_LSTM.ipynb
* CustomCode_Riano_Kalidas_RNN_LSTM_DAL.ipynb

**Pipeline**

![pipeline](tutorials/Pipeline%20for%20data%20flow.png)


**Folder Organization**
* [ehr_pytorch](ehr_pytorch): same folder where we put our custom codes:
    * CustomCode_Riano_Kalidas_RETAIN.ipynb: where we wrote the RETAIN model
    * CustomCode_Riano_Kalidas_RNN_GRU.ipynb: where we wrote the RNN GRU model
    * CustomCode_Riano_Kalidas_RNN_LSTM.ipynb: where we wrote the RNN LSTM model
    * CustomCode_Riano_Kalidas_RNN_GRU_DAL.ipynb: where we wrote the advanced adversarial learning on top of GRU
* [ehr_pytorch](ehr_pytorch): main folder with modularized components:
    * EHREmb.py: EHR embeddings
    * EHRDataloader.py: a separate module to allow for creating batch preprocessed data with multiple functionalities including sorting on visit length and shuffle batches before feeding.
    * Models.py: multiple different models
    * Utils.py
    * main.py: main execution file
    * tplstm.py: tplstm package file
* [Data](data)
    * toy.train: pickle file of  toy data with the same structure (multi-level lists) of our processed Cerner data, can be directly utilized for our models for demonstration purpose;
* Preprocessing
    * [data_preprocessing_v1.py](Preprocessing/data_preprocessing_v1.py): preprocess the data from dataset to build the required multi-level input structure
      (clear description of how to run this file is in its document header)
* [Tutorials](tutorials)
    * RNN_tutorials_toy.ipynb: jupyter notebooks with examples on how to run our models with visuals and/or utilize our dataloader as a standalone;
    * HF prediction for Diabetic Patients.ipynb
    * Early Readmission v2.ipynb
* trained_models examples:
    * hf.trainEHRmodel.log: examples of the output of the model
    * hf.trainEHRmodel.pth: actual trained model
    * hf.trainEHRmodel.st: state dictionary

**Dependencies**

* import torch
* import torch.nn as nn
* from torch.autograd import Variable
* from torch import optim
* import torch.nn.functional as F
* from torch.utils.data import Dataset, DataLoader

**Data Download Instruction**
* download everything from this repository and upload "as is" to your juypter notebook. You should be able to run the codes.

**Preprocessing code**
* the preprocessing file can be found on the EHRDataLoader and on the custom code on #3. Preprocess Data for Training
 
**Overview of Code Structure**
* There are no special commands for each of these steps, just place the notebook on the correct folder structure (follow structure as outlined below) and then run step by step. The data is already in the proper place as outlined below, so make sure the folder structure is followed correctly, place the notebook on the specified path above and it should ran properly.
* Preprocessing code: On the notebook these sections are labeled accordingly:
    * 1. Load Dataset
    * 2. Sample of dataset
    * 3. Preprocess Data for Training
* This is labeled as "1. Load Dataset 2. 3. Preprocess Data for Training" in the notebook
* Training code: On the notebook these sections are labeled accordingly:
    * 4. Train Recurrent Neural Network    
* Pretrained model: We did not use any pretrained model
* Evaluation/Table of Results: On the notebook these sections are labeled accordingly:
    * 5. Results Recurrent Neural Network (LSTM and GRU)
    * 5. Results RETAIN MODEL (RETAIN)

**Training code**
* This is labeled accordingly in the jupyter notebook - 4. Train 

**Evaluation code**
* This is labeled accordingly in the jupyter notebook - 5. Results 

**Pretrained code**
* Not applicable, we didn't use pretrained

**Table of Results**
* ![alt text](https://github.com/miloriano/cs598heartfailure_pytorch_ehr/blob/master/results.JPG?raw=true) 

Information below are mostly from original Readme, forked here -- https://github.com/ZhiGroup/pytorch_ehr. We didn't think we need to update or changed anything since we have forked this repository here and the information below holds true.

**Data Structure**

*  We followed the data structure used in the RETAIN. Encounters may include pharmacy, clinical and microbiology laboratory, admission, and billing information from affiliated patient care locations. All admissions, medication orders and dispensing, laboratory orders, and specimens are date and time stamped, providing a temporal relationship between treatment patterns and clinical information.These clinical data are mapped to the most common standards, for example, diagnoses and procedures are mapped to the International Classification of Diseases (ICD) codes, medimultications information include the national drug codes (NDCs), and laboratory tests are linked to their LOINIC codes.


*  Our processed pickle data: multi-level lists. From most outmost to gradually inside (assume we have loaded them as X)
    * Outmost level: patients level, e.g. X[0] is the records for patient indexed 0
    * 2nd level: patient information indicated in X[0][0], X[0][1], X[0][2] are patient id, disease status (1: yes, 0: no disease), and records
    * 3rd level: a list of length of total visits. Each element will be an element of two lists (as indicated in 4)
    * 4th level: for each row in the 3rd-level list. 
        *  1st element, e.g. X[0][2][0][0] is list of visit_time (since last time)
        *  2nd element, e.g. X[0][2][0][1] is a list of codes corresponding to a single visit
    * 5th level: either a visit_time, or a single code
*  An illustration of the data structure is shown below: 

![data structure](https://github.com/ZhiGroup/pytorch_ehr/blob/master/tutorials/Data%20structure%20with%20explanation.png)

In the implementation, the medical codes are tokenized with a unified dictionary for all patients.
![data example](https://github.com/ZhiGroup/pytorch_ehr/blob/MasterUpdateJun2019/tutorials/data.png)
* Notes: as long as you have multi-level list you can use our EHRdataloader to generate batch data and feed them to your model

**Paper Reference**

Original Paper we are reproducing -- https://www.sciencedirect.com/science/article/pii/S1532046420301465?via%3Dihub

The [paper](https://github.com/ZhiGroup/pytorch_ehr/blob/master/Medinfo2019_PA_SimpleRNNisAllweNeed.pdf) upon which this repo was built (https://github.com/ZhiGroup/pytorch_ehr/blob/master/Medinfo2019_PA_SimpleRNNisAllweNeed.pdf). 
Code structure and data forked here -- https://github.com/ZhiGroup/pytorch_ehr

**Versions**
This is Version 0.2, more details in the [release notes](https://github.com/ZhiGroup/pytorch_ehr/releases/tag/v0.2-Feb20)

**Dependencies**
* [Pytorch 0.4.0](http://pytorch.org) (All models except T-LSTM are compatible with pytorch version 1.4.0) , <b> Issues appear with pytorch 1.5 solved in 1.6 version</b>
* [Torchqrnn](https://github.com/salesforce/pytorch-qrnn)
* Pynvrtc
* sklearn
* Matplotlib (for visualizations)
* tqdm
* Python: 3.6+

**Usage**
* For preprocessing
 python data_preprocessing.py <Case File> <Control File> <types dictionary if available,otherwise use 'NA' to build new one> <output Files Prefix> 
The above case and control files each is just a three columns table like pt_id | medical_code | visit/event_date  

* To run our models, directly use (you don't need to separately run dataloader, everything can be specified in args here):
<pre>
python3 main.py -root_dir<'your folder that contains data file(s)'> -files<['filename(train)' 'filename(valid)' 'filename(test)']> -which_model<'RNN'> -optimizer<'adam'> ....(feed as many args as you please)
</pre>
* Example:

<pre>
python3.7 main.py -root_dir /.../Data/ -files sample.train sample.valid sample.test -input_size 15800 -batch_size 100 -which_model LR -lr 0.01 -eps 1e-06 -L2 1e-04
</pre>


* To singly use our dataloader for generating data batches, use:
<pre>
data = EHRdataFromPickles(root_dir = '../data/', 
                          file = ['toy.train'])
loader =  EHRdataLoader(data, batch_size = 128)
</pre>  
  #Note: If you want to split data, you must specify the ratios in EHRdataFromPickles()
         otherwise, call separate loaders for your seperate data files 
         If you want to shuffle batches before using them, add this line 
 <pre>
loader = iter_batch2(loader = loader, len(loader))
</pre>
otherwise, directly call 

<pre>
for i, batch in enumerate(loader): 
    #feed the batch to do things
</pre>

Check out this [notebook](tutorials/RNN_tutorials_toy.ipynb) with a step by step guide of how to utilize our package. 

**Warning**

* This repo is for research purpose. Using it at your own risk. 
* This repo is under GPL-v3 license. 

**Acknowledgements**
Hat-tip to:
* [DRNN github](https://github.com/zalandoresearch/pt-dilate-rnn)
* [QRNN github](https://github.com/salesforce/pytorch-qrnn)
* [T-LSTM paper](http://biometrics.cse.msu.edu/Publications/MachineLearning/Baytasetal_PatientSubtypingViaTimeAwareLSTMNetworks.pdf)


