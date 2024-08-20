# Sentinel of the Grid: Revealing Cyber Attacks through Sensor Anomalies


## NON-TECHNICAL EXPLANATION OF YOUR PROJECT
100 words to explain what your project is about to a general audience. 

This project is a capstone undertaking within the Artificial Intelligence and Machine Learning professional program at Imperial College London in 2024.
The idea behind the capstone project is to apply one or multiple, superviser or unsupervised, machine learning technique(s) to solve a real-world problem, measure a model's performance and conclude on the analysis.

In recent years, the risk of cyber attacks on Industrial Control Systems (ICS) has intensified. Threats targeting critical infrastructure, living off the land tactics, and gaps in incident response plans pose significant challenges. Proactive defense strategies and a focus on defense-in-depth are crucial. This project aims to detect cyber attacks and anomalies in electric transmission system from 37 power systems using measurements related to electric transmission system normal, disturbance, control, cyber attack behaviors. 

The approach taken for this project is to compare three supervised learning algorithms (K-Nearest Neighbors (k-NN), Gradient Boosting and Support Vecotr Machines) in order to predict the classification of an anomaly between three potential classes: Attack Events, Natural Events, No Events. 
The reason for selecting these three models, the rationale behind it and the approach are depicted further in the following sections, and in the associated model card. 

The project consist of:
- This README file introducing the project (README.md)
- A model card descibing each model used in more details, the trade-offs, performances and hyperparameters
  - Model_card-SVM.md - Support Vector Machines model
  - Model_card-GradientBoosting.md - Gradient Boosting model
  - Model_card-RandomForest.md - Random Forest model
- A more detailed analysis of the dataset (data_sheet.md)
- A set of Jupyter Notebooks with the actual code:
  - DataAnalysis-Cleaning.ipynb - The first notebook, used for data analysis and cleansing
  - Model-SVM.ipynb - The first model, support vector machines
  - Model-GradientBoosting.ipynb - The second model, gradient boosting
  - Model-RandomForest.ipynb - The third model, random forest
- A folder with the raw dataset (RawData/)
- A folder with the cleansed dataset (CleanData/)

## DATA

*Beaver, Justin M., Borges-Hink, Raymond C., Buckner, Mark A., "An Evaluation of Machine Learning Methods to Detect Malicious SCADA Communications," in the Proceedings of 2013 12th International Conference on Machine Learning and Applications (ICMLA), vol.2, pp.54-59, 2013. doi: 10.1109/ICMLA.2013.105 link*

The authors referenced here above have created 3 datasets which include measurements related to electric transmission system normal, disturbance, control, cyber attack behaviors. Measurements in the dataset include synchrophasor measurements and data logs from Snort, a simulated control panel, and relays.

The dataset can be found [here](https://sites.google.com/a/uah.edu/tommy-morris-uah/ics-data-sets). 
The dataset used for this specific exercise is the three-classes labeled dataset, a binary and multi-class datasets are also available.

The architecture of the Industrial Control System these datasets were extracted from is as follows:

![image](https://github.com/user-attachments/assets/756d186f-1a36-4b05-b0a6-23e8f33422ff)

Power System Attack Datasets - Mississippi State University and Oak Ridge National Laboratory - 4/15/2014
There are three datasets contained in this folder. They are made from one initial dataset consisting of fifteen sets with 37 power system
event scenarios in each. The multiclass datasets are in ARFF format for easy use with Weka and the others are in CSV format also
compatible with Weka. The 37 scenarios are divided into Natural Events (8), No Events (1) and Attack Events (28). The datasets were
randomly sampled at one percent and grouped into:
 Binary
 Three-class and
 Multiclass datasets.
The figure below shows the power system framework configuration used in generating these scenarios. In the network diagram we
have several components, firstly, G1 and G2 are power generators. R1 through R4 are Intelligent Electronic Devices (IEDs) that can
switch the breakers on or off. These breakers are labeled BR1 through BR4. We also have two lines. Line One spans from breaker one
(BR1) to breaker two (BR2) and Line Two spans from breaker three (BR3) to breaker four (BR4). Each IED automatically controls
one breaker. R1 controls BR1, R2 controls BR2 and son on accordingly. The IEDs use a distance protection scheme which trips the
breaker on detected faults whether actually valid or faked since they have no internal validation to detect the difference. Operators can
also manually issue commands to the IEDs R1 through R4 to manually trip the breakers BR1 through BR4. The manual override is
used when performing maintenance on the lines or other system components.
Types of Scenarios:
1. Short-circuit fault – this is a short in a power line and can occur in various locations along the line, the location is indicated
by the percentage range.
2. Line maintenance –one or more relays are disabled on a specific line to do maintenance for that line.
3. Remote tripping command injection (Attack) – this is an attack that sends a command to a relay which causes a breaker to
open. It can only be done once an attacker has penetrated outside defenses.
4. Relay setting change (Attack) – relays are configured with a distance protection scheme and the attacker changes the setting
to disable the relay function such that relay will not trip for a valid fault or a valid command.
5. Data Injection (Attack) – here we imitate a valid fault by changing values to parameters such as current, voltage, sequence
components etc. This attack aims to blind the operator and causes a black out

For the dataset we have picked, three classes, the labels are as follows:

![image](https://github.com/user-attachments/assets/6cba4ca5-1bfe-47aa-891d-6a7dd4ecd518)

THe dimensionality of the data is: 128 features, one prediction column and 78377 rows. 

The data is highly unbalanced:

Attack      0.7102
Natural     0.2336
NoEvents    0.0562

## MODEL 

We have chosen to use supervised learning and take the approach of a classification problem. We have chosen to use and compare three models from the wide range of classification models available, based on the nature of the data, the dataset, the course learning material, but also to understand the effects of dataset properties such as class imbalance on multiple classification techniques. The data is highly unbalanced and we therefore considered models which should not be affected too much by the imbalance or have proper methods to handle it. 

The three models are:

- Random Forests: Robust to imbalanced data due to ensemble nature.
- Gradient Boosting (e.g., XGBoost, LightGBM): Handles class imbalance well.
- Support Vector Machines (SVM) with Balanced Class Weights: Adjusts class weights to account for imbalance.

## HYPERPARAMETER OPTIMSATION

The hyperparameters and their optimizations are described in each model card respectively. However, we used Bayesian Optimization for Gradient Boosting and Random Forest, and we used Principal Compomnent Analysis with Grid Search for SVM model.
The idea was to test several techniques for optimization but also paliate to the perfornance trade-offs and computational needs of the models based on the size of the dataset.
All the models were trained and executed on a Laptop with 64GB of RAM and a 4-core CPU (i7).
No GPUs or optimized computing resources were used as this was not the intent behind this project.

## RESULTS

A summary of your results and what you can learn from your model 

You can include images of plots using the code below:
![Screenshot](image.png)
