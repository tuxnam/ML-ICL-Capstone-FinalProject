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

Number of Infinites after data pre-processing: 0.

**Output:** Describe the output(s) of your model

The output of the model is the expected classification of the measurements of each sensors in order to determine if the events might be linked to a cyber attack or not. 
The output can either be: NoEvents, NaturalEvents or AttackEvents.

**Model Architecture:** Describe the model architecture you’ve used

The original (cleaned) dataset, was split between 80% training set, and 20% test set.
As the dataset was highly imbalanced, we used stratified sampling to avoid bias:

![image](https://github.com/user-attachments/assets/cbce3d78-f0d1-436c-a06c-97db58cb8a0b)

The stratified sampling was simply done using _sklearn_ in Pyhton:

train, test = train_test_split(dataset, test_size=0.2, stratify=dataset.marker) 

## Hyperparameter Tuning

For this model, we used XGBoost (Extreme Gradient Boosting). When using the XGBClassifier from XGBoost, we can configure several hyperparameters to fine-tune the model. 
Due to high computing needs for the size of our dataset (128 features for more than 100K rows), GridSearch or RandomSearch are not convenient for hyperparameters tuning. 
We therefore used Bayesian Optimization.Bayesian optimization trains a machine learning model to predict the best hyperparameters. It is a function optimization: each set of hyperparameters corresponds to a different model performance (e.g., accuracy, AUC). Unlike grid search or random search, Bayesian optimization explores the parameter space systematically and intelligently.

The following parameters resulted from the Bayesian Optimization:

Best hyperparameters: {'learning_rate': 0.3, 'max_depth': 10.0, 'min_child_weight': 1.0}

These were applied to create and fit our model:

![image](https://github.com/user-attachments/assets/4604d094-67ee-46e7-a06c-1b2a68dd1750)

While XGBoost has other parameters, we focused on these for this project.

## Performance

To measure model performaces, and be able to compare the three models we used for this problem (Gradient Boosting, Random Forest and  Support Vector Machines), we used a Confusion Matrix and computed the following metrics: Precision, Recall, Accuracy amd F1 score.

We meadured both on the training set and on the test set each time:

**Training set**

![image](https://github.com/user-attachments/assets/9df3ab04-2744-4325-ad45-7aa1950b1d94)

**Test set**

![image](https://github.com/user-attachments/assets/db6adb06-df3b-412b-8591-6e66c7847197)

Precision: 0.9304
Recall: 0.8776
F1 Score: 0.8996
Accuracy: 0.91

The overall score is satisfying and Gradient Boosting ends up being the best model in the three compasred models for this project.

## Limitations and trade-offs

While **Gradient Boosting** offers numerous advantages, it also presents some challenges:

1. **Sensitivity to Hyperparameters**: Proper tuning of hyperparameters is crucial to prevent overfitting and achieve optimal performance. In our case, we focused on four parameters using Bayesian optimization, which might have limited the performace of our models. It would be wise to do the exercise on all the potential parameters. 
2. **Potential for Overfitting**: Gradient Boosting can overemphasize outliers, leading to overfitting. Cross-validation helps mitigate this issue, which we applied by splitting the training set and the test sets and measuring the F1 score for performance. We also scaled the data to avoid outliers having too much weight on the inputs. 
3. **Computational Complexity**: Training many trees can be computationally expensive in terms of time and memory. We limited to the default number of trees, 100, which can also impact model performance.
4. **Limited Interpretability**: Gradient Boosting models are more complex than simpler models like decision trees, making them less interpretable.
