# Sentinel of the Grid: Revealing Cyber Attacks through Sensor Anomalies


## NON-TECHNICAL EXPLANATION OF YOUR PROJECT
100 words to explain what your project is about to a general audience. 

This project is a capstone undertaking within the Artificial Intelligence and Machine Learning professional program at Imperial College London in 2024.
The idea behind the capstone project is to apply one or multiple, superviser or unsupervised, machine learning technique(s) to solve a real-world problem, measure a model's performance and conclude on the analysis.

In recent years, the risk of cyber attacks on Industrial Control Systems (ICS) has intensified. Threats targeting critical infrastructure, living off the land tactics, and gaps in incident response plans pose significant challenges. Proactive defense strategies and a focus on defense-in-depth are crucial. This project aims to detect cyber attacks and anomalies in electric transmission system from 37 power systems using measurements related to electric transmission system normal, disturbance, control, cyber attack behaviors. 

The approach taken for this project is to compare three supervised learning algorithms (K-Nearest Neighbors (k-NN), Gradient Boosting and Support Vecotr Machines) in order to predict the classification of an anomaly between three potential classes: Attack Events, Natural Events, No Events. 
The reason for selecting these three models, the rationale behind it and the approach are depicted further in the following sections, and in the associated model card. 

The project consist of:
- This README file introducing the project
- A model card descibing the models used in more details
- A set of Jupyter Notebooks with the actual code 
- A folder with the raw dataset

## DATA
A summary of the data you’re using, remembering to include where you got it and any relevant citations. 
Uttam Adhikari, Shengyi Pan, and Tommy Morris in collaboration with Raymond Borges and Justin Beaver of Oak Ridge National Laboratories (ORNL) have created 3 datasets which include measurements related to electric transmission system normal, disturbance, control, cyber attack behaviors. Measurements in the dataset include synchrophasor measurements and data logs from Snort, a simulated control panel, and relays.

The dataset can be found [here](https://sites.google.com/a/uah.edu/tommy-morris-uah/ics-data-sets). 
The dataset used for this specific exercise is the three-classes labeled dataset, a binary and multi-class datasets are also available.

The architecture of the Industrial Control System these datasets were extracted from is as follows:



## MODEL 
A summary of the model you’re using and why you chose it. 

## HYPERPARAMETER OPTIMSATION
Description of which hyperparameters you have and how you chose to optimise them. 

## RESULTS
A summary of your results and what you can learn from your model 

You can include images of plots using the code below:
![Screenshot](image.png)

## (OPTIONAL: CONTACT DETAILS)
If you are planning on making your github repo public you may wish to include some contact information such as a link to your twitter or an email address. 

