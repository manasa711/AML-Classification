# AML-Classification

The goal of this project is to predict patient's 'AML' or 'Normal' status from patient blood samples profiled by flow cytometry. The dataset contains 359 subjects, among whole 316 are normal and 43 have Acute Myeloid Leukemia (AML). Status labels for 179 samples are given and the task is to predict the status labels of the remaining 180 samples.

Download Dataset: [pengqiu.gatech.edu/MLB/CSV.zip](http://pengqiu.gatech.edu/MLB/CSV.zip.)

#### Data Preprocessing and Normalization: [data preprocessing.ipynb](https://github.com/manasa711/AML-Classification/blob/main/data_preprocessing_and_normalization.ipynb)

The values in the FCS measurements files were normalized with respect to each other and then I took a mean features approach and averaged all features in each file to create 7 cells per file. For each patient file, I appended the average measurements to create a 56 column (7 x 8) feature vector. With all the patient subjects, the combined data was a 359 rows x 56 columns matrix, with the rows as features of one patient and the columns are the measurement categories.

#### Data Splitting: [data_split.ipynb](https://github.com/manasa711/AML-Classification/blob/main/data_split.ipynb)

After data preprocessing and normalization, I split the training data randomly using a 80/20 split ratio. I used the split data to train multiple machine learning models on 80% of the data points and then used the other 20% for data validation

#### Testing and Choosing Machine Learning Models:

I tested out three different machine learning classification algorithms:
- [Logistic Regression](https://github.com/manasa711/AML-Classification/blob/main/Logistic_Regression_Model.ipynb)
- [KNN](https://github.com/manasa711/AML-Classification/blob/main/KNN_model.ipynb)
- [Random Forest](https://github.com/manasa711/AML-Classification/blob/main/Random_Forest_Model.ipynb)

I trained each of the 3 models on the 80% split and then used a validation dataset (the other 20%) to compare the performance of each model.

First, I tested and validated a logistic regression model on the training data and found that the model validation accuracy was low (<86%).

Second, I used a K-nearest Neighbors (KNN) model on the training data which yielded a 97.2% model accuracy. For this model, I hyperparameter tuned by checking multiple values from 2 to 9 for k, and got the same performance on the validation set for each value of k. To reduce model complexity, I used k=3 for the final KNN model.

Third, I used a Random Forest model and got a similar accuracy rate as the KNN model, 92.7%.

Since the Random Forst and KNN Model had the same accuracy scores, I relied on other performance metrics to choose the best prediction model. I calculated the F1 score, balanced accuracy score and ROC-AUC from prediction scores for both the models and got the same results for both:

Balanced Accuracy Score: 0.900
Accuracy Score: 0.972
F1 Score: 0.889
ROC-AUC Score: 0.900

Therefore, in order to decide which model to use for predicting on the test data set, I trained both models with all of the training data, preprocessed the test data similar to the train data, and then ran both models on the test data and produced confidence scores from each model for each individual prediction using sklearn.model.predict_proba().

After comparing the predictions of each test by the KNN and Random Forest models and I found 4 differences. To further discern the models on these 4 different predictions, we compared the prediction probabilities of each prediction in both the models and found that the KNN model had a higher prediction probability in all 4 cases (it was more confident in each of its answers than the Random Forest model was). Therefore, we used the KNN model to create our predictions.

#### Level of Confidence:

By assessing the prediction probabilities on each test sample using KNN model, I saw that 170 of the test cases had 1.0 prediction probability. However, the prediction probabilities varied between 0.62 and 0.74 for the remaining 10 predictions. Of these 10 cases, 4 of them were predicted differently in the Random Forest model. There are a total of 176 samples which produced the same prediction between both the KNN and the RF model. This included a combination of the 170 from KNN with 1.0 prediction probability and the other 6 that the two models did not disagree on. As a result, I found 176 predictions that are highly confident and the other 4 predictions that are slightly less confident (samples 239, 250, 337, and 357).

#### Result:

Using the KNN model, I have been able to identify 20 AML positive cases out of the 180 samples.
