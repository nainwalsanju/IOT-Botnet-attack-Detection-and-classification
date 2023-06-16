# botnet-traffic-analysis

This is a project for my thesis for IoT botnet traffic analysis *DETECTING, CLASSIFYING AND EXPLAINING IOT BOTNET ATTACKS USING DEEP LEARNING METHODS BASED ON NETWORK DATA*

Abstract:

The growing adoption of Internet-of-Things devices brings with it the increased participation of said devices in botnet attacks, and as such novel methods for IoT botnet attack detection are needed. This work demonstrates that deep learning models can be used to detect and classify IoT botnet attacks based on network data in a device agnostic way and that it can be more accurate than some more traditional machine learning methods, especially without feature selection. Furthermore, this works shows that the opaqueness of deep learning models can mitigated to some degree with Local Interpretable Model-Agnostic Explanations technique.

----------------------
# Anamoly Detection
The aim is to reproduce this paper's https://arxiv.org/pdf/1805.03409.pdf models.
This is the dataset used https://archive.ics.uci.edu/ml/machine-learning-databases/00442/

Some data about model used in papers is in `config.json` file.

## Python dependencies
Check that you have python dependencies installed listed in `requirements.txt` file

## Training models

* Download normal traffic training data using `download_data.py` script. For eachdevice this will create its folder and download there csv with normal traffic data.
* Run `train.py`, you can give it as parameters names of devices to train the models for. No parameters = train for all devices.

Training will use number of epochs and learning rate from `config.json` file for respective device. 

After successful training `model.h5` is saved to device folder and it can be used for testing.

## Training a combined model

You can run `train_combined.py` to train a model for all normal traffic. It will be saved as `combined_model.h5`

## Training logs
Training logs for devices are saved to `device/logs` folder and in case of combined model to `combined_model_logs` folder.

You can run tensorboard `tensorboard --logdir=device/logs` to see a summary of model training.

## Testing
Download attack data for testing purposes `download_attack_data.py`. You then need to manually extract these `gafgyt.rar` archives, csv attack files should be in the same folder as normal traffic csv.


Testing scripts `test.py` and `test_combined.py` will calculate anomaly treshold, compare it to the one claimed in paper; check number of false positives on normal traffic and check number of false alarms with window size specified in the paper.
Then it will take some of the attack data, equal in size to the nortal test data and calculate number of false negatives on it.


## Some remarks
The paper does not specify:
* batch size
* activation functions
* type of scaling used

Therefore, default keras batch size was chosen.
As activation function `tanh` was chosen for hidden layers, as it is a reasonable choice.
For scaling, `sklearn's StandardScaler` was used.

# Classification

## Python dependencies
Check that you have python dependencies installed listed in `requirements.txt` file

## Download the dataset
Use `download_data.py` script

Then extract rar files into respective `{device_name}/gafgyt_attack/` and `{device_name}/mirai_attack/` directories

## Run the training and evaluation script
`train.py`

You can specify if you just want to use just top N features

`train.py 5`


## Run just the test for the existing model
As input give it the top number of features to use and the name of the model to load

`test.py 5 'model_5.h5'`


---------------
## Results


For all features
Trained for 20 epochs for 42 mins.
Results are

Model evaluation
Loss, accuracy
[0.0010332345978360195, 0.9998208877454652]

SO Accuracy on test set is 0.99982


Confusion matrix
Benign     Gafgyt     Mirai
[[111369    114      2]
 [    34 567253      7]
 [     9     87 733647]]


For top 5 features, trained for 5 epochs
Loss, accuracy 
[0.004696070021679117, 0.9991879772492039]

Confusion matrix
Benign     Gafgyt     Mirai
[[111436     40      9]
 [   647 566647      0]
 [    63    388 733292]]


For top 3 features, trained for 5 epochs, 99s per epoch
Loss, accuracy 
[0.0032474066202494442, 0.9994704507257232]

Confusion matrix
benign  gafgyt  mirai
[[111439     40      6]
 [   460 566834      0]
 [    80    162 733501]]


For top 2 features, trained for 5 epochs, 99s per epoch
Loss                   Accuracy
[0.33539704368039586, 0.8430219139947991]
Confusion matrix
benign  gafgyt  mirai
[[ 58989  52297    199]
 [   523 436574 130197]
 [   486  38033 695224]]




