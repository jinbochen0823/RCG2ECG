# mmWave ECG Dataset (MMECG Dataset)
MMECG is an open-source dataset of 10 hours of processed mmWave radar data and synchronized ECG measurements collected from 35 participants under four different physiological statuses. It can facilitate digital health research based on mmWave radar. (RightNow, we only release 4.55 hours of data. The rest of the data are still under the authorizing process, considering the privacy concern. We will update this page once we finish the authorizing process.) Following, we introduce the composition and implementation details of this dataset. 


# Dataset Implementation
## Experimental Setup
During data acquisition, the participants are asked to lie in the supine position and remain in quasi-static status. The radar sensor is placed above the torso chest within 0.4-0.5m, and the main lobe of the antennas is directed to the sternum approximately. We conducted 200 experimental trials over 35 participants (22males and 14 females) between the ages of 18 and 65. The trials are designed consisting of 4 different physiological statuses:  normal-breath, irregular-breath, post-exercise (for instance, jumping jacks), and sleep to expand the diversity of cardiac rhythms (including arrhythmia, bradycardia, tachycardia, normal rhythm) in the datasets. Each trial lasts for 3 minutes.
Experimental settings are shown as follows:

![ep](https://github.com/jinbochen0823/RCG2ECG/blob/b56cba7b073065eaf1a38cca3ae47d1cac9fab8d/expsettings.png)


## Hardware Configuration
This dataset is collected by TI AWR1843 mmWave radar (left) and DCA1000 real-time data acquisition board (right). Specifically, we activate 3 transmitters (Tx) and 4 receivers (Rx) to achieve a virtual 2D antenna array with 12 channels. Time division multiplexing strategy is exploited to achieve signal orthogonal in time among multiple Tx antennas. During one frame of radar sensing, all the 3 Tx transmit chirps of RF signal successively with $45\mu s$ interval to acquire the baseband signal from one channel to the entire 4 Rxs. 

![dca](https://github.com/jinbochen0823/RCG2ECG/blob/af7a37891caa07e4640241334d77a2c2aedc7a57/awr1843dca1000.png)

The parameters of the radar are set as follows:

Parameter|Value|Parameter|Value
:--:|:--:|:--:|:--:
Start frequency|77GHz|Sample points |256
Frequency slope|65MHz/µs|Sample rate |5MHz
Idle time |10µs| Ramp end time |60µs
Frame periodicity |5ms

Under these settings, the radar achieves a frame rate of **200Hz**, total **3.32Ghz** bandwidth.

## Data preprocessing
The radar raw signals are processed to 4D cardiac motion measurements by sequence of signal processing algorithms introduced in **Cardiac Motion Measurements in Radar Section** of the paper, which can be expressed in the representation as $C_{S} = \{\mu_{n},l_{\mu_{n}}\}, \forall n \in 1,2,...,N$, where $\mu_{n}$ is a cardiac motion measurement sequence with k frames respect to the 3D location $l_{\mu_{n}}$. In this dataset N is set to 50. The x, y, z axis of 3D coordination system are parallel to the direction of body height, body width, and vertical to chest approximately.


![sp](https://github.com/jinbochen0823/RCG2ECG/blob/6fac444dc0af4307f1b05089e6ba14faa2740623/sigprocess.png)
## Dataset Structure
- Each 3 minutes trial data are saved in a Matlab mat file which named with trails index.
- Each mat file is a structure array consisting of 6 fields of data (RCG, ECG, id, age, gender, physiological status).  RCG is the 4D cardiac motion measurement. The first dimension is sample points, and the second dimension is N mentioned in the last section. ECG is the synchronized ECG measurements. ID, age, and gender represent the participant’s index, age, and gender respectively. The physiological status of normal-breath, irregular-breath, post-exercise and sleep are denoted as 'NB', 'IB', 'PE','SP', respectively.


# How to access the dataset
To obtain the dataset, please sign the [agreement](datasetAgreement.pdf), scan and send it to jinbochen@mail.ustc.edu.cn. You will receive a notification email which includes the download links of the dataset in three days.

## Citation
-If you use this dataset, please cite the following paper :

**Chen, Jinbo, et al. "Contactless electrocardiogram monitoring with millimeter wave radar." IEEE Transactions on Mobile Computing 2022 doi: 10.1109/TMC.2022.3214721.**

-You may also be interested in the Human Indoor Behavior Exclusive RF dataset [HIBER](https://github.com/wuzhiwyyx/HIBER/tree/master).

Z. Wu et al., "RFMask: A Simple Baseline for Human Silhouette Segmentation With Radio Signals," in IEEE Transactions on Multimedia, 2022, doi: 10.1109/TMM.2022.3181455.

C. Yu, Z. Wu, D. Zhang, Z. Lu, Y. Hu and Y. Chen, "RFGAN: RF-Based Human Synthesis," in IEEE Transactions on Multimedia, doi: 10.1109/TMM.2022.3153136.

and mmWave cross domain gesture dataset [MCD-Gesture Dataset](https://github.com/DI-HGR/cross_domain_gesture_dataset)

Y. Li et al., "Towards Domain-Independent and Real-Time Gesture Recognition Using Mmwave Signal," in IEEE Transactions on Mobile Computing, 2022, doi: 10.1109/TMC.2022.3207570.
