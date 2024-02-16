# credit-risk-classification
 MSU Data Analytics BootCamp Module 20 Challenge

## Overview of the Analysis

The goal of this analysis was to generate a logistic regression model that could ultimately be applied to screen individuals for credit (loan) worthiness based on known information about those individuals. The logistic regression model evaluates credit-pertinent information about each individual borrower and attempts to categorize each individual loan into a good or delinquent loan status (after the repayment begins). 

We used a simulated loan dataset split into training and testing data subsets to train and evaluate our logistic regression model. Our raw dataset included the following variables for each individual: the total loan amount, the loan interest rate, the borrower income, borrower debt to income ratio, borrower total credit accounts, number of derogatory remarks on credit file, total outstanding debt, and a binary category of whether the loan in question was currently in good standing or not (0 = good standing, 1 = delinquent). We used the Python library sciki-learn's LogisticRegression function to fit our loan training data subset to a logistic regression model, which was subsequently tested using our testing data subset to determine how well the model was able to predict the correct loan status class based on the given variables. The training data subset was then randomly oversampled (using scikit-learn's RandomOverSampler function) to account for and potentially correct for bias introduced by the large discrepancy between good standing vs. delinquent loan data in our training dataset (over 75,000 loans in good standing vs. only 2,500 delinquent loans in our raw data). The resampled data was again fit to a logistic regression model using scikit-learn's LogisticRegression function and the resampled regression model was evaluated for classification performance with the test dataset.

## Results

* Machine Learning Model 1 (non-oversampled data split into 80% training and 20% testing data subsets):
  * Accuracy: for the entire logistic regression model, the balanced accuracy score was 0.97, indicating the model was very good overall at placing loans into the correct status category while accounting for class size imbalance.
  * Precision: for categorization into the “healthy” loan status class, the model had perfect precision (1.00), indicating in did not sort a single false positive into the “healthy” loan class; for the “high-risk” loan status class, the model had a precision of 0.86, which while generally good, does indicate the model has some trouble with sorting false positives into the high-risk status class (14% false positive rate for the high risk classification). This may suggest that at the margins or under certain relatively rare combinations of factors, perfectly good loans appear to the model to be high-risk.
  * Recall: for categorization into the “healthy” loan status class, the model had near perfect recall (0.99), indicating excellent performance in sorting every single instance of a “healthy” loan into the correct class (extremely low false negative rate); for the “high-risk” loan status class, the model had a recall of 0.94, which while also excellent, suggests the model is slightly weaker at catching high-risk loans compared to healthy ones. The higher recall for the high-risk category compared to precision suggests the model tends to “overpredict” loans into the high-risk classification.

* Machine Learning Model 2 (randomly oversampled data split into 80% training and 20% testing data subsets:
  * Accuracy: for the entire oversampled logistic regression model, the balanced accuracy score was 0.99, indicating the model was nearly perfect at placing loans into the correct status category.
  * Precision: for categorization into the “healthy” loan status class, the model again had perfect precision (1.00), indicating in did not sort a single false positive into the “healthy” loan class; for the “high-risk” loan status class, the model had a precision of 0.85, which while generally good, does indicate the model still has some trouble with sorting false positives into the high-risk status class (15% false positive rate for the high risk classification). This number is essentially unchanged from the original model. This suggests that at the margins or under certain relatively rare combinations of factors, perfectly good loans still appear to the model to be high-risk.
  * Recall: for categorization into the “healthy” loan status class, the model had near perfect recall (0.99), indicating excellent performance in sorting every single instance of a “healthy” loan into the correct class (extremely low false negative rate); for the “high-risk” loan status class, the model also had near perfect recall of 0.99. This suggests the oversampled model is slightly better at “catching” high-risk loans compared to the non-oversampled model. This does not come at the cost of worse model precision or overall worse model performance. The higher recall for the high-risk category compared to precision suggests the model still tends to “overpredict” loans into the high-risk classification (15% false positive rate), but also does a better job of correctly categorizing a higher proportion of the high-risk loans into the correct category vs. the non-oversampled model.

## Summary

The oversampled logistic model performs better than the non-oversampled model with functionally no downside. The higher balanced accuracy score, higher high-risk recall, and higher high-risk f1-score of the oversampled model, with no change to the healthy loan classification success for any metric, indicates the overall improved quality of our oversampled model vs. the non-oversampled model. Therefore, if we were to employ either of the two models, the second (oversampled) model is technically better at classifying loans into the correct status category and would be the best general classification model. I would absolutely recommend deploying this model generally, but the second model is particularly good if the following is true: our core goal is to minimize lending to risky borrowers. But what is the goal of the model? If our goal is to maximize lending (while still keeping risk controlled) through getting loans into the hands of as many acceptable borrowers as possible, the second model doesn't perform much better than the first and we should focus on improving the model precision for the high-risk loan class while keeping high-risk recall high. If our goal is to minimize risk as much as possible (while potentially rejecting a non-trivial number of applicants that would ultimately be good borrowers) then the second model does perform better than the first. Now we have to answer questions such as: what is the ultimate goal of the model (risk-reduction or lending-maximization), how much time and labor are we willing to give to reviewing loans at the margins to achieve those goals, and amount of misclassification towards the stated goals we are willing to tolerate.
