### An Analysis of Loan Default Likelihood

**George Li**

#### Executive summary

#### Rationale
Fintechs are increasingly relying on data beyond just a credit score to make credit decisions. This is because the credit score alone may have biases toward certain demographics and also only provides a limited view of a person who may not have a long credit history. Predictions like this allow fintechs to seek more customers without running the risks of taking on too many customers with higher likelihoods of delinquency.
#### Research Question
What factors are more predictive of whether a person is going to be able to pay back a loan or default?
#### Data Sources
Loan default dataset from Kaggle: https://www.kaggle.com/datasets/marcbuji/loan-default-prediction Links to an external site.
#### Methodology
Clustering Algorithms are necessary to determine the most significant features that contribute to whether someone defaults or not.
Classification algorithms to help predict whether someone defaults or not.
Hyperparams need to be tuned using grid search.
It would be optimal to use cross validation and to simplify things, I will be using 5-fold cross validation.
#### Results
Using LASSO to select the top 5 most important features, we found that those features were: ['Inquiries', 'Validation', 'Duration', 'Designation', 'File_Status']

After noticing this, I decided that I should drop the File_Status column because it might be messing up the results. This is because those who have a fully paid account
would not default for sure but that tells us nothing useful since "I'm fully paid" implies "I'm not in default" and thus would have high multi-colinearity.

After dropping that feature, this was the new top 5: ['Asst_Reg', 'Inquiries', 'Validation', 'Total_Unpaid_CL', 'Designation']
Using Logistic Regression, Decision Trees, Random Forest, and KNN, we determined that each model weighted the importance of these features quite differently.

 ![feature_importance_deci_tree.png](images/feature_importance_deci_tree.png)

 ![feature_importance_log_reg.png](images/feature_importance_log_reg.png)

 ![feature_importance_random_forest.png](images/feature_importance_random_forest.png)

 ![feature_importance_random_forest.png](images/feature_importance_knn.png)

That said, the models all seemed to factor in designation and total unpaid CL as more important features. This might make sense since having a 
higher unpaid debts usually means default could be more likely and the designation is the type of job they have which would determine a general income range as well as a general
amount of assets. 

Some Raw Results:

     Results for Logistic Regression:
     Best Parameters: {'C': 1, 'penalty': 'l2'}
     Best Cross-Validation F1 Score: 0.0198
     Test F1 Score: 0.0268
     Runtime: 1.45 seconds
     Classification Report:
                   precision    recall  f1-score   support
     
                0       0.81      1.00      0.90     14226
                1       0.52      0.01      0.03      3274
     
         accuracy                           0.81     17500
        macro avg       0.67      0.51      0.46     17500
     weighted avg       0.76      0.81      0.73     17500
     
     
     Feature Importance:
     Asst_Reg           0.092033
     Inquiries          0.178322
     Validation         0.203739
     Total_Unpaid_CL    0.261955
     Designation        0.203760
     dtype: float64
     
     Results for Decision Tree:
     Best Parameters: {'max_depth': None, 'min_samples_leaf': 1, 'min_samples_split': 2}
     Best Cross-Validation F1 Score: 0.0933
     Test F1 Score: 0.0797
     Runtime: 1.41 seconds
     Classification Report:
                   precision    recall  f1-score   support
     
                0       0.81      0.95      0.88     14226
                1       0.20      0.05      0.08      3274
     
         accuracy                           0.78     17500
        macro avg       0.50      0.50      0.48     17500
     weighted avg       0.70      0.78      0.73     17500
     
     
     Feature Importance:
     Asst_Reg           0.064061
     Inquiries          0.125015
     Validation         0.155368
     Total_Unpaid_CL    0.089860
     Designation        0.565695
     dtype: float64
     
     Results for Random Forest:
     Best Parameters: {'max_depth': None, 'min_samples_leaf': 1, 'min_samples_split': 2, 'n_estimators': 100}
     Best Cross-Validation F1 Score: 0.0948
     Test F1 Score: 0.0692
     Runtime: 129.22 seconds
     Classification Report:
                   precision    recall  f1-score   support
     
                0       0.82      0.98      0.89     14226
                1       0.34      0.04      0.07      3274
     
         accuracy                           0.81     17500
        macro avg       0.58      0.51      0.48     17500
     weighted avg       0.73      0.81      0.74     17500
     
     
     Feature Importance:
     Asst_Reg           0.036219
     Inquiries          0.092022
     Validation         0.076933
     Total_Unpaid_CL    0.054818
     Designation        0.740007
     dtype: float64
     
     Results for KNN:
     Best Parameters: {'metric': 'euclidean', 'n_neighbors': 3, 'weights': 'distance'}
     Best Cross-Validation F1 Score: 0.2156
     Test F1 Score: 0.1526
     Runtime: 4.71 seconds
     Classification Report:
                   precision    recall  f1-score   support
     
                0       0.82      0.94      0.88     14226
                1       0.29      0.10      0.15      3274
     
         accuracy                           0.78     17500
        macro avg       0.55      0.52      0.51     17500
     weighted avg       0.72      0.78      0.74     17500
     
     
     Feature Importance:
     Asst_Reg           0.000783
     Inquiries          0.004554
     Validation         0.002229
     Total_Unpaid_CL    0.003823
     Designation        0.006949
     dtype: float64
     
     Best overall model: KNN
     Best overall test F1 score: 0.1526
     Runtime for best model: 4.71 seconds

If we go by the F1-score measure, KNN has the best accuracy. It also turned out to be the more performant model with random forest taking the longest. The top 5 features were selected to improve performance with the models.
Otherwise, Random Forest might take even longer. 

However, it seems like the F1-scores for predicting no default were much better than predicting a default. And the overall test f1 scores did not appear to be particularly good although accuracy scores were decent for KNN and random forest. 
In an effort to improve this, I tried this again with the top 10 features.

I did get some modest improvement, but with 10 features, KNN was no longer the best model. Instead, Decision Trees became the best.

    Results for Decision Tree:
    Best Parameters: {'max_depth': None, 'min_samples_leaf': 1, 'min_samples_split': 2}
    Best Cross-Validation F1 Score: 0.2416
    Test F1 Score: 0.2318
    Runtime: 4.38 seconds
    Classification Report:
                  precision    recall  f1-score   support
    
               0       0.82      0.81      0.82     14226
               1       0.22      0.24      0.23      3274
    
        accuracy                           0.70     17500
       macro avg       0.52      0.52      0.52     17500
    weighted avg       0.71      0.70      0.71     17500
    
    
    Feature Importance:
    Debt_to_Income     0.199514
    State              0.148592
    Lend_Amount        0.185981
    Unpaid_Amount      0.076862
    Asst_Reg           0.017244
    File_Status        0.144429
    Inquiries          0.043663
    Validation         0.051791
    Total_Unpaid_CL    0.029917
    Designation        0.102007
    dtype: float64

#### Next steps
If possible, the goal would be to gather some real world results and use them for predictions. Other steps would be to optimize the number of features and use permutation importance to drop the least
important ones which could optimize the models further. In order to improve the models, more hyperparameter tuning and perhaps different levels of cross validation could be tried as well. While neural networks
could also be explored, in the real world, using a neural network model may run afoul of regulations since it would be hard to explain why a credit decision was made.
#### Outline of project

- [analysis.ipynb](analysis.ipynb)
- [analysis_top10 features.ipynb](analysis_top10 features.ipynb)

##### Contact and Further Information
George Li

gzli92@gmail.com

George.Li@bestegg.com (work)

LinkedIn: https://www.linkedin.com/in/gli92

```