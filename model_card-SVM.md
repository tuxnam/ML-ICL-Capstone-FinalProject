# Model Card - Support Vector Machines

Our model's goal is to classify events from sensors of simulated electrical grid pipeline scenarios between three classes: No Events, Natural Events or Attack (cyber-attack) Events. See our (README)[https://github.com/tuxnam/ML-ICL-Capstone-FinalProject/tree/main] and (data sheet)[https://github.com/tuxnam/ML-ICL-Capstone-FinalProject/blob/main/data_sheet.md] description.

This model card describes one of the supervised machine learning technique used to achieve this goal, Support Vector Machines (SVMs). 
Support Vector Machine (SVM) is a powerful supervised machine learning algorithm used for both classification and regression tasks. We focused on the classification aspect for our problem.

- SVM finds an optimal hyperplane in an N-dimensional space to separate data points into different classes.
- The goal is to maximize the margin between the closest points of different classes.
- SVM is robust to outliers and can handle high-dimensional data.

As part of this exercice, we used SVC from sklearn. The implementation is based on libsvm. The fit time scales at least quadratically with the number of samples which was one of the limitation encounted for the hyperparameter tuning and the overall performance of this model. 

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

SVM models have multiple hyperparameters, the most two important ones being the Kernel and the Cost Parameter. 
The following kernels will be tested and chosen through cross-validation: Linear, Polynomial, RBF. 
As for the Cost, it controls the trade-off between achieving a low training error and a few training error, to avoid overfitting. We will use gridsearch, and apply it so that the computation cost is reasonnable. We added use of PCA as the performance was still not manageable. We also used univeriate feature selection for performance reasons.

The following parameters resulted from the grid search and PCA:

Best parameters found:  {'C': 10, 'gamma': 1, 'kernel': 'rbf'} 

These were applied to create and fit our model:

![image](https://github.com/user-attachments/assets/32e11a40-65c0-4e8f-8193-671d5703d384)

While SVC() has other parameters, we focused on these for this project.

## Performance

To measure model performaces, and be able to compare the three models we used for this problem (Gradient Boosting, Random Forest and  Support Vector Machines), we used a Confusion Matrix and computed the following metrics: Precision, Recall, Accuracy amd F1 score.

We meadured both on the training set and on the test set each time:

**Training set**

![image](https://github.com/user-attachments/assets/e6d812d9-9a7d-46e2-a292-225f6dac43b6)


**Test set**

![image](https://github.com/user-attachments/assets/405e0cdf-534f-4034-8c60-6f5b64e2e63b)

The overall score is not satisfying, conpared to Gradient Boosting, but still performs better than Random Forests.

## Limitations and trade-offs

While **SVMs** offers numerous advantages, they also presents some challenges:

1. **Computational Complexity:**  SVMs can become computationally demanding for large datasets or high-dimensional spaces. Proper parameter selection is essential, as poorly chosen values can impact performance.
They may require significant memory resources, and without proper tuning, they can overfit small datasets.
2. **Reduced Interpretability:** SVMs aim to find an optimal hyperplane, but the resulting model may be less interpretable than simpler classifiers. The trade-off is improved accuracy at the cost of transparency.
3. **Non-Probabilistic Nature:** SVMs assign data points to classes with 100% certainty (though a poorly tuned SVM may still make mistakes). Unlike probabilistic models, SVMs don’t provide class probabilities.

The other tradeoff is that we used SVC() fron libsvm in sklearn. However, the fit time scales at least quadratically with the number of samples, this was therefore less recommended for >100K samples.
Alternatives were for instance LinearSVC or SGDClassifier. 
This led to multiple information loss in the hyperparameter tuning phase to circumvent or performance issues, with univariate feature selection and PCA both used before the grid search, which probably led to information loss and introduction of bias into our model.
