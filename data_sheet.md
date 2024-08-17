# Datasheet - Attacks on Industrial Control Systems (ICS) - Electrical Pipelines

## Motivation

Uttam Adhikari, Shengyi Pan, and Tommy Morris in collaboration with Raymond Borges and Justin Beaver of Oak Ridge National Laboratories (ORNL) have created 3 datasets which include measurements related to electric transmission system normal, disturbance, control, cyber attack behaviors. Measurements in the dataset include synchrophasor measurements and data logs from Snort, a simulated control panel, and relays.
The dataset was created as part of the Center for Cybersecurity Research and Engineering (CCRE). 

There are three datasets contained in this folder. They are made from one initial dataset consisting of fifteen sets with 37 power system event scenarios in each. The multiclass datasets are in ARFF format for easy use with Weka and the others are in CSV format also compatible with Weka. The 37 scenarios are divided into Natural Events (8), No Events (1) and Attack Events (28). The datasets were
randomly sampled at one percent and grouped into:
- Binary
- Three-class and
- Multiclass datasets.

The dataset used for this project is the three-class dataset. 
 
## Composition

The dataset contains 128 features (127 measurements and one output 'marker' column with the classification)

From http://www.ece.uah.edu/~thm0009/icsdatasets/PowerSystem_Dataset_README.pdf:

*"The 128 features are explained in the table below. There are 29 types of measurements from each phasor measurement units (PMU). A phasor measurement unit (PMU) or synchrophasor is a device which measures the electrical waves on an electricity grid, using a common time source for synchronization. In our system there are 4 PMUs which measure 29 features for 116 PMU measurement columns total. The index of each column is in the form of “R#-Signal Reference” that indicates a type of measurement from a PMU specified by “R#”. The signal references and corresponding descriptions are listed below. For example, R1-PA1:VH means Phase A voltage phase angle measured by PMU R1. After the PMU measurement columns, there are 12 columns for control panel logs, Snort alerts and relay logs of the 4 PMU/relay (relay and PMU are integrated together). The last column is the marker. The first three digits
on the right is the load condition (in Megawatt). Another three digits to their left is fault locations, for example, “085” means fault at 85% of the transmission line specified by scenario description. However, for those that do not involve fault, e.g. “line maintenance”, these digits will be set to 000. The most left one digit or two digits indicate(s) the scenario number."*

![image](https://github.com/user-attachments/assets/38d696c2-5042-4c2b-a0ff-27bb74c2939c)

As for the output column, named 'marker', it can take the following values for the three-class dataset that we leveraged for this project: 
- No event: no anomalies from the measurements
- Natural event: anomalies linked to a natural event (e.g.: Short-circuit fault, Line maintenance...)
- Attack event (e.g.: Relay setting change, Data injection...)

## Collection process

From http://www.ece.uah.edu/~thm0009/icsdatasets/PowerSystem_Dataset_README.pdf:

*"The figure below shows the power system framework configuration used in generating these scenarios. In the network diagram we
have several components, firstly, G1 and G2 are power generators. R1 through R4 are Intelligent Electronic Devices (IEDs) that can
switch the breakers on or off. These breakers are labeled BR1 through BR4. We also have two lines. Line One spans from breaker one
(BR1) to breaker two (BR2) and Line Two spans from breaker three (BR3) to breaker four (BR4). Each IED automatically controls
one breaker. R1 controls BR1, R2 controls BR2 and son on accordingly. The IEDs use a distance protection scheme which trips the
breaker on detected faults whether actually valid or faked since they have no internal validation to detect the difference. Operators can
also manually issue commands to the IEDs R1 through R4 to manually trip the breakers BR1 through BR4. The manual override is
used when performing maintenance on the lines or other system components."*

![image](https://github.com/user-attachments/assets/fddf5b3c-be98-4245-8f15-946060977f59)


## Preprocessing/cleaning/labelling

The raw dataset used for this project (three classes), can be found in the following folder: [RawData](https://github.com/tuxnam/ML-ICL-Capstone-FinalProject/tree/main/RawData).
The data was first analyzed and cleaned in the following Notebook: [DataAnalysis-Cleaning.ipynb](https://github.com/tuxnam/ML-ICL-Capstone-FinalProject/blob/main/DataAnalysis-Cleaning.ipynb). 

Here are the steps which were taken: 
- Analyzing the data sets and consolidating the data into a single file
- Removing infinite values: We have four columns with infinite values for approximately 10k rows (12%!). This is a considerable amount of data from the dataset. We decided to replace the infinite values with the median value of the column, to avoid losing information for the input of the model, but also avoid to create bias by zeroing the element.

The 'cleansed' dataset can be generated by running the above notebook on the raw data. It is however too large to upload on GitHub.
 
## Uses

- What other tasks could the dataset be used for? 
- Is there anything about the composition of the dataset or the way it was collected and preprocessed/cleaned/labeled that might impact future uses? For example, is there anything that a dataset consumer might need to know to avoid uses that could result in unfair treatment of individuals or groups (e.g., stereotyping, quality of service issues) or other risks or harms (e.g., legal risks, financial harms)? If so, please provide a description. Is there anything a dataset consumer could do to mitigate these risks or harms? 
- Are there tasks for which the dataset should not be used? If so, please provide a description.

## Distribution

The dataset is distributed from this website: https://sites.google.com/a/uah.edu/tommy-morris-uah/ics-data-sets.

The only requirement for its use is to cite its source: Beaver, Justin M., Borges-Hink, Raymond C., Buckner, Mark A., "An Evaluation of Machine Learning Methods to Detect Malicious SCADA Communications," in the Proceedings of 2013 12th International Conference on Machine Learning and Applications (ICMLA), vol.2, pp.54-59, 2013. doi: 10.1109/ICMLA.2013.105.

## Maintenance

The dataset has been created in 2014: Power System Attack Datasets - Mississippi State University and Oak Ridge National Laboratory - 4/15/2014. 
It does not require specific maintenance as it is a snapshot of system events during simulated scenarios. 

- Who maintains the dataset?

