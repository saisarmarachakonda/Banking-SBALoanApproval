          Helping SBA (Small Business Administration) to Minimize Loss Ratio Using Classification Model.
==============================================================================================

read full article here: https://amstat.tandfonline.com/doi/full/10.1080/10691898.2018.1434342

Introduction
============

The Small Business Administration (SBA) was established in 1953 with the aim of aiding small businesses in securing loans. Small businesses have long been the primary source of employment in the United States, and assisting them has a positive impact on job creation, ultimately reducing unemployment rates. Additionally, the growth of small businesses contributes to overall economic prosperity.

One of the methods employed by the SBA to support small businesses is by providing loan guarantees to banks. These guarantees serve to mitigate the risk for banks, making them more willing to lend to small businesses. In cases where a borrower defaults on the loan, the SBA covers the guaranteed portion, while the bank absorbs the loss for the remaining balance.

While there have been notable success stories in the realm of small businesses, such as FedEx and Apple, it is important to note that the default rate on SBA-backed loans is relatively high. Some economists argue that the banking market may function more efficiently without SBA intervention. On the other hand, proponents contend that the social benefits and job creation stemming from SBA assistance outweigh any financial costs incurred by the government due to defaulted loans.

According to this data,

We know that the average charge-off ratio is around 0.72 or 72%. This means that if a start-up or a company borrows $ 100,000 from a bank, then the charge-off or the bank loss possibility is around $72,000. It’s a quite amount of money! 💰💰💰

What to do
==========

In this project, we will attempt to assist the Small Business Administration (SBA) in making decisions about whether a company is eligible for a loan based on the company’s demographic data.

The Goal
========

To develop a robust and data-driven decision-making system that leverages company demographic data to accurately assess and determine the eligibility of businesses for loans, ultimately aiding the Small Business Administration (SBA) in making informed lending decisions and promoting responsible financial support for small enterprises.

The Values
==========

The project’s primary value lies in enhancing the efficiency and accuracy of the loan approval process for small businesses. By leveraging data-driven insights and analytics, it empowers the Small Business Administration (SBA) to:

1.  **Minimize Risk**: Reduce the likelihood of providing loans to businesses with a high risk of default, safeguarding financial resources.
2.  **Support Growth**: Facilitate access to capital for deserving small businesses, promoting growth and economic development.
3.  **Data-Driven Decisions**: Foster a culture of data-driven decision-making within the SBA, leading to more informed and equitable lending practices.
4.  **Cost-Efficiency**: Optimize resource allocation by focusing on businesses most likely to succeed, lowering administrative and operational costs.
5.  **Economic Impact**: Contribute to the success and sustainability of small businesses, positively impacting local economies and job creation.
6.  **Transparency**: Increase transparency in the lending process, building trust among applicants and stakeholders.

In summary, the project’s value is realized through improved loan approval outcomes, reduced risk exposure, and the promotion of financial inclusivity and responsible lending practices in support of small business success.

The Dataset
===========

The original data set is from the U.S.SBA loan database, which includes historical data from 1987 through 2014 (899,164 observations) with 27 variables. The data set includes information on whether the loan was paid off in full or if the SMA had to charge off any amount and how much that amount was.

For more information on this data, please visit this [link](https://amstat.tandfonline.com/doi/full/10.1080/10691898.2018.1434342).

Constrain
=========

For the sake of learning and due to computation limitations, we will only focus on California State, since it is the most state there are.

Source Code
===========

Please visit this link to get the source code >> [Kaggle](https://www.kaggle.com/code/saisarmarachakonda/sba-loan-approval-model) <<

Data Cleansing and Preprocessing
================================

At this stage, we will conduct data cleaning and processing. We will also normalize the data if there are inconsistencies. We will use the Numpy and Pandas libraries to do this, most of the time.

1.  **MIS\_Status**:  Since this column is the target column, we will remove all NaN values from it.
2.  **State**: As mentioned before, we will focus on California State. So we will filter out any observations that are not from California State.
3.  **ChgOffDate**: There are too many missing values in this column. Moreover, we will not be using this column as a feature because it is impossible to know when the company will be charge-off by the time we want to predict their charge-off status.
4.  **ApprovalDate**: In the future, we won’t need this column because it contains information that is not relevant to our objectives. However, for analysis purposes, we will keep it to determine which approval months have a high Charge-Off ratio.
5.  **NAICS**: Which stands for **North American Industry Classification System**, is used to classify business establishments according to the type of economic activity (process of production) in Canada, Mexico, and the United States. This column contains a unique categorical (nominal) value. We will only extract the first two codes from each NAICS code, and transform them into relevant business sectors.
6.  **Term**: It shows how long the company will pay back the loan. It’s a numerical column which refers to the number of days. We will process this into three bins which are divided into: “Below 3 months”, “3–6 months”, “6–12 months”, and “More than a year”.
7.  **NewExist**: The purpose of this column is to identify a company whether they are included in a New Business (2) or Existing Business (1). Unfortunately, there are several observations that have a value that is missing or does not refer to those groups (0.0). So we will exclude them, and keep the rest of it.
8.  **BalanceGross**: Drop this column, because it only contains a single value (Variance=0).
9.  **ChgOffPrinGr**: We will drop this column since it will leak information to the algorithm. Every time it has a value, the MIS\_Status column is always “CHGOFF”.
10.  **SBA\_Appv, ApprovalFY, GrAppv**: We will drop any columns that are related to approval information since they are not relevant features.
11.  For more details about data cleansing and preprocessing, you can visit this link: [Cleansing and Preprocessing](https://www.kaggle.com/code/ridhoaryo/sba-loan-approval-model?scriptVersionId=143598431&cellId=12).

Exploratory Data Analysis — Categorical Features
================================================

At this stage, we will carry out data visualization and explanation. Here, we hope to find features that have a strong influence on our target variable.

**NewExist**
------------

New businesses have a **1.2x (0.25/0.20) higher chance of charge-off compared to established businesses**. This makes sense because new businesses are typically less stable in their early periods. It is also evident from the chi-squared test that NewExist has a strong relationship with ChargeOff.

**UrbanRural**
--------------

Interestingly, borrowers with an **undefined Urban/Rural** category **have a 1.4x (0.95/0.67) higher chance of being able to repay** their loans compared to companies with a known Urban/Rural category. This means the **default risk for these companies is very low (4.6%)**. However, if we exclude the Undefined category, companies in both Urban and Rural areas have roughly equal chances of defaulting.

**RevLineCr**
-------------

Companies that borrow through **Revolving Credit methods have almost 2x (0.31/0.16) higher chance of defaulting** compared to those that do not use this method. This column has a strong relationship with the ChargeOff column.

**LowDoc**
----------

Companies that use the **LowDoc loan program have a very low chance of default (0.18x)**. This column also has a strong relationship with the ChargeOff column.

**ApprovalMonth**
-----------------

Companies whose proposals were **approved in July and September have a lower chance of defaulting (0.85–0.81x)** compared to those approved in other months. However, we will not use this column further because its value appears after a company’s loan proposal has been approved. What we want to predict occurs before the ApprovalMonth value appears, so it wouldn’t make sense to use this column as a feature.

**Sector**
----------

The Sector column has a strong relationship with ChargeOff. The probability of charge-off varies significantly across different sectors. The **highest charge-off ratio is found in companies in the “Real Estate and Rental and Leasing” sector**. In contrast, the sector with the lowest charge-off ratio is “Agriculture, Forestry, Fishing and Hunting.”

**Term**
--------

Companies with a loan term of **less than 3 months have a default ratio of 0.34 (34%)**, meaning that out of 100 borrowers who choose a term of less than 3 months, 34 of them will default. **On the other hand, companies with a term of 6–12 months have a low default ratio (0.017)**.

**BankState**
-------------

The banks located in the state of Virginia experience the highest cases of defaults among borrowers.

Exploratory Data Analysis — Numerical Features
==============================================

There is **no numerical column that has a strong relationship** with ChargeOff **except for the Number of Employees**. In the boxplot, it can be observed that companies with a **larger number of employees tend to be in the “Paid in Full” (PIF)** category rather than “Charge-Off.”

To get more details about the EDA part, you can visit this [link](https://www.kaggle.com/code/ridhoaryo/sba-loan-approval-model?scriptVersionId=143598431&cellId=91).

Machine Learning Modelling
==========================

For the current machine learning modelling, we aim to minimize the model’s prediction of companies that will actually Charge Off but are predicted as Paid in Full (PIF). Hence, we will focus on the Recall Score.

“Charge-Off” is a condition where a lender determines that a debt is unlikely to be collected and treats it as a loss on their financial records. This would result in losses for the borrowers.

According to the data we currently have, the Charge-Off rate stands at 72.4%. This means that if a company has a debt of USD 100,000, it cannot pay USD 72,400 to the lender.

Strategy
--------

In the modelling step, I will use a Pipeline to help me build the model. First of all, I will divide the data into two categories:

1.  Categorical Variables
2.  Quantitative Variables

A categorical variable is data that takes category or label values. On the other hand, quantitative variables take numerical values and represent some kind of measurement.

There are 13 columns that I have listed as categorical variables. Since each column values has no order with each other, I will encode them using **One-Hot Encoding**.

The rest of the columns should be listed in the other category, and since each column has a significantly different value range from each other and is highly skewed, I will try to scale them using **Robust Scaler**.

Since there are no missing values, I don’t need an imputer in this process.

The models that I will use are **Logistic Regression** and **Random Forest Classifier**. We will compare both of them and choose the model which gives us the best result.

Split The Data
--------------

First, we will split the data into training and testing sets. We will try using 75% of the data for training and the rest as testing data. From the 75% that we use for training, we will conduct cross-validation to assess the stability of each model, as well as determine the average recall score they produce from that cross-validation process.

```
((60805, 13), (20269, 13))
```

Model Benchmark
---------------

Second, we will initiate a benchmark model, which will be used as our initial foundation for choosing a machine learning model, before we eventually conduct hyperparameter tuning and other optimization processes.

*   **Logistic Regression**

*   **Random Forest Classifier**

Cross Validation
----------------

Then, we will perform cross-validation using 5 CV iterations.

Based on the cross-validation results above, we found that the Random Forest Classifier model produces the highest average Recall value of 0.861. This value is also not too different from its training value of 0.863, meaning the likelihood of it overfitting is low. On the other hand, Logistic Regression has a lower value (0.84). Therefore, at this stage, we decided to proceed with modelling using the Random Forest Classifier.

Hyperparameter Tuning for Random Forest Classifier
--------------------------------------------------

Next, we will conduct hyperparameter tuning. This step aims to find the best parameters for the Machine Learning model to learn from the data. Unlike previous projects, this time we will try a new method using Optuna. The concept is the same as GridSearch CV or RandomizedSearch CV. However, the difference with Optuna is that we have more flexibility in conducting hyperparameter tuning by optimizing a value that we define ourselves within a function.

```
\[I 2023-09-20 02:13:28,127\] A new study created in memory with name: RFCSBALoanApproval 1.0  
\[I 2023-09-20 02:44:04,567\] Trial 0 finished with value: 0.5460919435014552 and parameters: {'max\_depth': 6, 'min\_samples\_split': 19, 'min\_samples\_leaf': 7, 'criterion': 'entropy'}. Best is trial 0 with value: 0.5460919435014552.  
\[I 2023-09-20 03:14:59,024\] Trial 1 finished with value: 0.5480187355121117 and parameters: {'max\_depth': 7, 'min\_samples\_split': 10, 'min\_samples\_leaf': 8, 'criterion': 'entropy'}. Best is trial 1 with value: 0.5480187355121117.  
\[I 2023-09-20 03:45:37,790\] Trial 2 finished with value: 0.5476977991966149 and parameters: {'max\_depth': 7, 'min\_samples\_split': 15, 'min\_samples\_leaf': 6, 'criterion': 'entropy'}. Best is trial 1 with value: 0.5480187355121117.  
\[I 2023-09-20 04:16:09,677\] Trial 3 finished with value: 0.5455815189710406 and parameters: {'max\_depth': 5, 'min\_samples\_split': 13, 'min\_samples\_leaf': 5, 'criterion': 'gini'}. Best is trial 1 with value: 0.5480187355121117.  
\[I 2023-09-20 04:46:49,492\] Trial 4 finished with value: 0.5521447816283224 and parameters: {'max\_depth': 8, 'min\_samples\_split': 12, 'min\_samples\_leaf': 6, 'criterion': 'gini'}. Best is trial 4 with value: 0.5521447816283224.
```

```
{'max\_depth': 8, 'min\_samples\_split': 12, 'min\_samples\_leaf': 6, 'criterion': 'gini'}
```

After obtaining the best hyperparameters, we will train our machine learning model using the training set with those hyperparameters.

Here, we achieved a recall score of 0.875. This is quite good, but since our objective is to minimize the default ratio as much as possible, we will try to optimize the threshold using the Precision-Recall Curve, hoping to improve the Recall score.

Of course, how much we will improve a metric depends on the business decisions and the specific use case. If at a certain value, stakeholders consider it sufficient, then we can proceed to the next process with that value. Currently, let’s assume that stakeholders want a Recall value >90%. Even though there will be consequences of a lower Precision value, the business is currently prioritizing reducing the loss ratio.

Using the Precision-Recall curve, we successfully achieved a Recall value of 0.901 or 90.1% and a Precision of 0.387 or 38.7% by shifting the classification probability threshold to 0.403914.

Then, how to interpret this?

Conclusion
==========

Up to this point, we have been able to improve the Recall to 90%. This means that out of 10 companies applying for loans and actually being potential defaulters, we can accurately predict 9 of these companies. The remaining 1 companies, we mispredict as non-defaulters, and we continue to lend money to them.

If we lend 100,000 USD to these 10 companies and, based on our previous analysis, the average charge-off rate is around 72.4%, then one company that mispredicted will charge off approximately 72,400 USD.

On the other hand, there is a note we should also consider. In addition to recall, we also have a precision score of 39%. This means that if we accurately predict CHGOFF (True Positive) for 90 cases, we predict CHGOFF (False Positive) for about 231 companies that are actually capable of paying in full (PIF).

SBA can further develop this model depending on SBA’s needs. Because the current objective of this project is to minimize losses from Charge Off, increasing recall is the right step.

However, without using machine learning, we have limitations in assessing whether a company will default or not. At best, we can predict 50% accurately. In the same context, this would result in a loss of 3,620,000 USD from defaulting companies.

Here we can see that by using Machine Learning, we can reduce the risk of losses when providing capital loans.

Thank You!
==========
        