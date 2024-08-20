# Model Card - Random Forest

Our model's goal is to classify events from sensors of simulated electrical grid pipeline scenarios between three classes: No Events, Natural Events or Attack (cyber-attack) Events. See our (README)[https://github.com/tuxnam/ML-ICL-Capstone-FinalProject/tree/main] and (data sheet)[https://github.com/tuxnam/ML-ICL-Capstone-FinalProject/blob/main/data_sheet.md] description.

This model card describes one of the supervised machine learning technique used to achieve this goal, Random Forest. 
Random Forest is a powerful and versatile supervised machine learning algorithm. Random Forest combines multiple decision trees to create a “forest.”
It’s used for both classification and regression tasks and is part of _sklearn_ library Python.

How It Works:
Grows an ensemble of uncorrelated decision trees.
Each tree “votes” on the classification (for classification tasks) or predicts a value (for regression tasks).
The final prediction is based on majority votes (classification) or averaging (regression).

As part of this exercice, we used RandomForestClassifier from sklearn. As per the library definition, a random forest is a meta estimator that fits a number of decision tree classifiers on various sub-samples of the dataset and uses averaging to improve the predictive accuracy and control over-fitting. 

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

For this model, we will use Random Forest regressor. A random forest is a meta estimator that fits a number of decision tree regressors on various sub-samples of the dataset and uses averaging to improve the predictive accuracy and control over-fitting. Due to high computing needs for the size of our dataset, GridSearch or RandomSearch are not convenient for hyperparameters tuning. We also want to avoid losing too much information. We will therefore use Bayesian Optimization. Bayesian optimization trains a machine learning model to predict the best hyperparameters. It is a function optimization: each set of hyperparameters corresponds to a different model performance. Unlike grid search or random search, Bayesian optimization explores the parameter space systematically and intelligently.

The following parameters resulted from the Bayesian Optimization:

Best hyperparameters: OrderedDict([('max_depth', 19), ('max_features', 0.2224536011385949), ('min_samples_leaf', 2), ('min_samples_split', 9), ('n_estimators', 151)])

These were applied to create and fit our model:

![image](https://github.com/user-attachments/assets/dbeeabd3-0c60-4e80-a9e8-87fdeb817fff)

While RandomForestClassifier has other parameters, we focused on these for this project.

## Performance

To measure model performaces, and be able to compare the three models we used for this problem (Gradient Boosting, Random Forest and  Support Vector Machines), we used a Confusion Matrix and computed the following metrics: Precision, Recall, Accuracy amd F1 score.

We meadured both on the training set and on the test set each time:

**Training set**

![image](https://github.com/user-attachments/assets/426b29f3-24e1-4cbe-ae6d-34b725192901)


**Test set**

![image](https://github.com/user-attachments/assets/dc472593-d2fc-4ac1-a68b-e916fb276a8c)


<br />
Precision: 0.7377<br />
Recall: 0.4612<br />
F1 Score: 0.4989<br />
Accuracy: 0.74<br />
<br />

The overall score is not satisfying, conpared to Gradient Boosting. 

## Limitations and trade-offs

While **Random Forest** offers numerous advantages, it also presents some challenges:

1. **Bias-Variance Tradeoff:** Random Forest aims to reduce variance compared to individual decision trees. However, it may sacrifice some interpretability (bias) due to the ensemble approach.
2. **Reduced Interpretability:** Random Forest combines multiple trees, making it less interpretable than a single decision tree. The trade-off is improved predictive performance at the cost of model transparency.
3. **Computational Cost:** Training multiple trees can be computationally expensive. Balancing performance gains with training time is essential.
4. **Hyperparameter Tuning:** Proper tuning of hyperparameters (like tree depth and number of trees) is crucial. Incorrect settings can impact model performance which might be a limitation of the fact that we applied hyperparameter tuning on only a subset of the hyperparameters, and with only 5 folds and 10 iterations. 
