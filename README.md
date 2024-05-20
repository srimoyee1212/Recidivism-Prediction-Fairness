# Recidivism-Prediction-Fairness
As a part of Cornell CS 5382  Designing and Developing Fair Algorithms, analyzed Recidivism Bias and worked on improving it

This component of the assignment derives in part, with thanks and permission, from an [assignment](https://web.stanford.edu/class/cs182/assignments/AlgorithmicDecisionMaking.zip) in Stanford's CS182: Ethics, Public Policy, and Technological Change. Their assignment, in turn, is based on the journalistic organization ProPublica's [analysis](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) of a criminal risk prediction algorithm which we discussed in the algorithmic fairness lecture. Here, you will be assessing how a classifier designed to predict recidivism -- that is, whether someone will commit a crime in the future -- performs in terms of algorithmic fairness metrics.


Originally, Logistic Regression was used to predict recidivsim. While designing my own model, my focus was to reduce the predictive inequalites and difference in odds between the classes. I tried different models such as Random Forest and XGBoost. Out of the two XGBoost performed better. Furthermore, I did a bit of preliminary exploration and also learnt from the class lecture that there are often other factors that are strongly correlated with Race such as Age and Gender. Hence I dropped these before training my model. I also tried to use SMOTE (https://machinelearningmastery.com/smote-oversampling-for-imbalanced-classification/) to reduce the class imbalance a bit but it did not help so I left it out. I also modified the column names using regex because XGBoost was giving me an error in processing columns which has '[]' or other such characters. Further, since I was going to use the XGBoost classifier I used a Standard Scaler to scale my data before training the model.

I used grid search cross validation to obtain the best possible parameters for my XGBoost classifier model and then used these parameters to define the model. I then used the model for prediction and obtained class predictions as well as class probabilities. I used AUC score, F1 score, False Positive Rate, False Negative Rate and Accuracy (fraction of positive classifications) as metrics to evaluate my model. As you can see, the overall AUC is higher as compared to the logistic regression model. Also the AUC for White and Black defendants is higher. Furthermore, there is more the difference between the FPR and TPR rates becomes narrower than when logistic regression is used. 
Here are the values again for better comparison:

FPR -> A person who is not likely to re-offend is incorrectly labelled as likely to re-offend

FPR values using Logistic Regression:   

White: 0.1719  

Black: 0.4165     

FPR values using XGBoost:

White: 0.2321

Black: 0.3927

FNR -> A person likely to re-offend is incorrectly labelled as not likely to do so

FNR values using Logistic Regression:

White: 0.5018

Black: 0.2707                                             

FNR values using XGBoost:

White: 0.4770

Black: 0.3585

With the logistic regression model, there is a huge risk of very high number of false negatives(0.5018) for white defendants. What this means is that even when a white defendant recidivates, they are incorrectly labelled to not do so. The ethical implication of this is enormous and becomes even more critical if the degree of crime is severe.

These values indicate that the XGBoost Model is more balanced in terms of FPR and FNR for African American defendants which is something I was trying to achieve. Furthermore, it tries to balance out the very high False Negative Rate for white defendants.

I also used the F1 score in an attempt to understand the predictions better and a higher score for African American defendants meant that it was making good classifications for that particular class.

I think models could be used in criminal risk prediction but they should not be considered the absolute ground truth and the decisions made by the models should not be finalised without human input. This is because as seen from the two very different types of models implemented above, there is an issue of bias in the data which becomes really hard to de-bias when the model is designed. These models are thus propagating existing racial biases in the society through biased datasets and amplifying them. Especially in critical scenarios like healthcare and criminal justice, one should always proceed with caution while using these models, and if they are used at all, the decisions must be reviewed and examined by humans/subject matter experts before finalising.

I think the ProPublica Analysis gives an insight as to why there are several issues with deploying such models in the context of criminal justice. To add on to it, the metrics as highlighted above raise serious concerns with how accurate these models are. With hardly ~70% accuracy, it seems like a gamble on someone's life and freedom to treat these models' predictions as absolute ground truth and make decisions based on that.
