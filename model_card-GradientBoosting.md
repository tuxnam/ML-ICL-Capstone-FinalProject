# Model Card - Gradient Boosting

Our model's goal is to classify events from sensors of simulated electrical grid pipeline scenarios between three classes: No Events, Natural Events or Attack (cyber-attack) Events. See our (README)[https://github.com/tuxnam/ML-ICL-Capstone-FinalProject/tree/main] and (data sheet)[https://github.com/tuxnam/ML-ICL-Capstone-FinalProject/blob/main/data_sheet.md] description.

This model card describes one of the model used to achieve this goal, Gradient Boosting. 
Gradient Boosting is a machine learning algorithm used for both classification and regression tasks. We used the classifier from the Python library (*XGBoost*)[https://xgboost.readthedocs.io/en/stable/] in this case.

**Principle:** Gradient Boosting works on the idea that combining many weak learners (such as shallow decision trees) can create a more accurate predictor.

**How It Works:** 
Sequentially builds simpler models, where each model tries to predict the error left over by the previous one. 
A weak learner is a model that performs slightly better than random predictions.
The algorithm focuses on minimizing prediction errors by iteratively adding new weak learners. 
Gradient Boosting is an ensemble technique that combines predictions from multiple models to improve overall accuracy.
It’s particularly useful for regression and classification problems.

XGBoost is an optimized distributed gradient boosting library designed to be highly efficient, flexible and portable. 
It implements machine learning algorithms under the Gradient Boosting framework. 
XGBoost provides a parallel tree boosting (also known as GBDT, GBM) that solve many data science problems in a fast and accurate way. 

## Model Description

**Input:** 

The model take the following inputs: 127 sensors measurements as described in the data sheet.

![image](https://github.com/user-attachments/assets/bd16bb7b-40c0-4462-9bfd-183a1c4f0900)

All the inputs are numerical, however the scales of each feature are quite spread. Example for the first nine features:

![image](https://github.com/user-attachments/assets/e20fe89d-f0c4-4d32-a1a0-89d4d1e57844)

Inputs were therefore scaled using a MinMax scaler (https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html):

![image](https://github.com/user-attachments/assets/934cd1af-97b5-41b0-bb99-217ae60d3e59)

Some of the inputs also contained infinite values for some sensor measures, as the number of rows having infinite values was considerable (around 12%), we did not remove the rows to avoid information loss. What we rather did is replace the infinite values by the median of the respective columns:

Number of NaNs:  0
float64    113
int64       15
Name: count, dtype: int64
Number of Infinites:  10906

With the following columns were infintie values were replaced:

Index(['R1-PA:Z', 'R2-PA:Z', 'R3-PA:Z', 'R4-PA:Z'], dtype='object')
Number of Infinites:  0


**Output:** Describe the output(s) of your model

**Model Architecture:** Describe the model architecture you’ve used

## Performance

Give a summary graph or metrics of how the model performs. Remember to include how you are measuring the performance and what data you analysed it on. 

## Limitations

Outline the limitations of your model.

## Trade-offs

Outline any trade-offs of your model, such as any circumstances where the model exhibits performance issues. 
