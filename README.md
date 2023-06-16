# botnet-traffic-analysis

This is a project for my thesis for IoT botnet traffic analysis *DETECTING, CLASSIFYING AND EXPLAINING IOT BOTNET ATTACKS USING DEEP LEARNING METHODS BASED ON NETWORK DATA*

Abstract:

The growing adoption of Internet-of-Things devices brings with it the increased participation of said devices in botnet attacks, and as such novel methods for IoT botnet attack detection are needed. This work demonstrates that deep learning models can be used to detect and classify IoT botnet attacks based on network data in a device agnostic way and that it can be more accurate than some more traditional machine learning methods, especially without feature selection. Furthermore, this works shows that the opaqueness of deep learning models can mitigated to some degree with Local Interpretable Model-Agnostic Explanations technique.

----------------------
# About this project
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

