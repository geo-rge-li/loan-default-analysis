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
#### Results
Using Logistic Regression, Decision Trees, and Random Forest, we found that the most consequential feature that factors into defaults are the total amount of assets which is shown as Asst_Reg_encoded.
![feature_importance_deci_tree.png](images/feature_importance_deci_tree.png)
![feature_importance_log_reg.png](images/feature_importance_log_reg.png)
![feature_importance_random_forest.png](images/feature_importance_random_forest.png)
The other common feature that seems to be found pretty important in all three models is the Debt_to_Income ratio. Both of those would make sense in that if a person has fewer assets or higher amount of existing debt, the likelihood of default is significantly higher. 


#### Next steps
What suggestions do you have for next steps?

#### Outline of project

- [Link to notebook 1]([analysis.ipynb](analysis.ipynb))


##### Contact and Further Information