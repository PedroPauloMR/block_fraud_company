# The Fraud Blocker Company - FBC
## Preventing fraudulent transactions for good 👊

![](img/banner_title.png)

<font size=2><span>Photo by <a href="https://unsplash.com/@cbpsc1?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Clint Patterson</a> on <a href="https://unsplash.com/s/photos/hacker?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span></font>

# 1.0 The context

## 1.1 What is fraud?
Fraud is an intentionally deceptive action designed to **provide the perpetrator with an unlawful gain or to deny a right to a victim**. In addition, it is **a deliberate act (or failure to act) with the intention of obtaining an unauthorized benefit**, either for oneself or for the institution, by using deception or false suggestions or suppression of truth or other unethical means, which are believed and relied upon by others. Depriving another person or the institution of a benefit to which he/she/it is entitled by using any of the means described above also constitutes fraud.

Types of fraud include tax fraud, credit card fraud, wire fraud, securities fraud, and bankruptcy fraud. Fraudulent activity can be carried out by one individual, multiple individuals or a business firm as a whole.

## 1.2 Key Facts (as Aug/2020)
- Fraud involves **deceit** with the intention to illegally or unethically gain at the expense of another.
- In **finance**, fraud can take on many forms including making false insurance claims, cooking the books, pump & dump schemes, and identity theft leading to unauthorized purchases.
- Fraud **costs the economy billions of dollars** each and every year, and those who are caught are subject to fines and jail time.
- **Consumer fraud** occurs when a person suffers from a financial loss involving the use of deceptive, unfair, or false business practices.
- With **identity theft**, thieves steal your personal information, assume your identity, open credit cards, bank accounts, and charge purchases.
- **Mortgage scams** are aimed at distressed homeowners to get money from them.
- **Credit and debit card fraud** is when someone takes your information off the card and makes purchases or offers to lower your credit card interest rate.
- **Fake charities** and lotteries prey on peoples' sympathy or greed.
- **Debt collection fraud** tries to collect on unpaid bills whether they are yours or not.
- **COVID-19 scams** are a new type of fraud designed to prey on your fear or financial need.

## 1.3 The Fraud Blocker Company - FBC
The Fraud Blocker Company (FBC) is a company specialized in detecting fraud in financial transactions made through mobile devices. The company has a service called “Blocker Fraud” in which it guarantees the blocking of fraudulent transactions.

The company's business model is of the Service type with the monetization made for the performance of the service provided, that is, the customer pays a fixed fee on the success in detecting fraud in the customer's transactions. In addition, the FBC is expanding and to acquire customers more quickly, it has adopted a very aggressive strategy, FBC will:  

> - Receive 25% of the value of each transaction that is truly detected as fraud (true positives).
> - Receive 5% of the value of each transaction detected as fraud, but the transaction is truly legitimate (false positives).
> - Refund 100% of the value to the customer, for each transaction detected as legitimate, however the transaction is truly a fraud (false negatives).

With this aggressive strategy, the company assumes the risks of failing to detect fraud and is remunerated for assertive fraud detection. For the client, it is an excellent business to hire the FBC. Although the fee charged is very high on success, 25%, the company reduces its costs with fraudulent transactions correctly detected and the damage caused by an error in the anti-fraud service will be covered by the FBC itself.

For FBC, in addition to getting many customers with this risky strategy to guarantee reimbursement in the event of a failure to detect customer fraud, it depends only on the precision and accuracy of the models built by its Data Scientists, that is, the greater the precision of “Fraud Blocker” model, the greater the company's revenue. However, if the model has low precision, the company could have a huge loss.

PS 1: All the references are stated at the end of this README.

PS 2: You can find useful information at **section 1** of my [notebook](#).

<br>

# 2.0 The challenges
Given that we have a historical data set containing 6MM+ transactions, how might we solve FBC business challenges in order to **maximize its revenue and minizes its risks and losses**?


<br>

# 3.0 The solution

In this project, I managed to address the challenge by developing a classification model that can be deployed to FBC. The model has a **precision** that ranges from **96.52% to 98.38%** and can help FBC in calculating the expected revenue taking into account the business model. Thus, the company can experiment different values for charging its clients while knowing the model precision.

<br>

## 3.1 What drove the solution

### 3.1.1 Exploratory Data Analysis

#### Descriptive Analysis

![](img/descriptive-statistics.png)

Key points:
- The **min amount** for a trasaction is **0.01**.
- The **min balance** for both origin and destination is **0.00**.
- The **max amount** for a transaction is **92,445,520.00**.

<br>

#### Hypothesis Map

This map to help us to decide which variables we need in order to validate the hypotheses.

![](img/hypotheses-map.png)

#### Univariate Analysis - Target

![](img/univariate-analysis-target.png)

Number of fraudulent transactions: 8097 (0.13% of the total transactions)

Number of non fraudulent transactions: 6252434 (99.87% of the total transactions)

<br>

#### Univariate Analysis - Numerical

![](img/univariate-analysis-numerical.png)

As observed, 

- There is a **higher concentration** for amount between 59873.14 and 442412.39.
- In general, the **major concentration of balance is around zero**.

<br>

#### Univariate Analysis - Categorical

![](img/univariate-analysis-categorical.png)

Key points:

- Cash-out and payment represent the major part of the total transactions.
- The great majority of the transactions are not flagged as fraud.
- There is only one type of origin: customer.
- There are more transactions whose destination is customer.
- The origin old balance status is non zero for the majority of transactions.
- There are more transactions whose the origin new balance status is zero.
- There are more transactions whose the destination old balance status is non zero.
- There are more transactions whose the destination new balance status is non zero.
- No transactions has the the same origin as destination.
- There are more transactions whose origin old balance is higher than the new.
- The destination old balance is higher than the new for the majority of the transctions.
- The transaction direction C2C is almost the double of C2M.
- Number of unique values for `name_orig`: 6,251,489 (1.0% of the total transactions)
- Number of unique values for `name_dest`: 2,677,202 (0.43% of the total transactions)

<br>

### 3.1.2 Hypothesis validation - Bivariate Analysis

#### Main Hypothesis

#### H1. Payment represent 50% of the total fraudulent transactions. (FALSE)

| Transaction Type | Percentage of the total |
|------------------|-------------------------|
| CASH\_OUT        | 50\.02                  |
| TRANSFER         | 49\.98                  |
| PAYMENT          | 0\.00                   |
| DEBIT            | 0\.00                   |
| CASH\_IN         | 0\.00                   |


![](img/H1_payment_total.png)

As observed, there is no fraudulent transaction for payment. Fraudulent transactions occur for cash out and transfer.

> Thus, the hypothesis is **FALSE**.

#### H2. Transfer represent 50% of the total non fraudulent transactions. (FALSE)

| Transaction Type | Percentage of the total |
|------------------|-------------------------|
| CASH\_OUT        | 35\.20                  |
| TRANSFER         | 33\.85                  |
| PAYMENT          | 21\.99                  |
| DEBIT            | 8\.31                   |
| CASH\_IN         | 0\.65                   |

![](img/H2_transfer_total.png)

As observed, transfer represents `8.3%` of the total non fraudulent transactions. Cash-out and payment are the majority of transaction types.

> Thus, the hypothesis is **FALSE**.

### H3. Customer to Merchant (C2M) represent at least 50% of the total fraudulent transactions. (FALSE)

| Transaction direction | Percentage of the total |
|-----------------------|-------------------------|
| C2C                   | 100\.00                 |
| C2M                   | 0\.00                   |

![](img/H3_C2M_fraud.png)

As observed, C2C represents `100%` of the total fraudulent transactions, while there is no C2M.

> Thus, the hypothesis is **FALSE**.

### H8. Destination non zero new balance represents 50% of the total fraudulent transactions. (TRUE)

| Destination New Balance status | Percentage of the total |
|--------------------------------|-------------------------|
| Non zero                       | 50\.19                  |
| Zero                           | 49\.81                  |

![](img/H8_destination_new_balance_fraud.png)

As observed, destination non zero new balance represents `~50%` of the total fraudulent transactions.

> Thus, the hypothesis is **TRUE**.

### H11. At least 95% of the total fraudulent transactions are flagged as fraud. (FALSE)

| Flagged as fraud? | Percentage of the total |
|-------------------|-------------------------|
| Yes               | 99\.80                  |
| No                | 0\.20                   |

![](img/H11_flag_fraud_fraud.png)

As observed, `~99.8%`of the total fraudulent transactions are not flagged as fraud.

> Thus, the hypothesis is **FALSE**.

<br>

#### Hypothesis summary

| ID  | Hypothesis                                                                                                    | Conclusion |
|:-----:|:---------------------------------------------------------------------------------------------------------------|:------------|
| H1  | Payment represent 50% of the total fraudulent transactions                                                    | FALSE      |
| H2  | Transfer represent 50% of the total non fraudulent transactions                                               | FALSE      |
| H3  | Customer to Merchant \(C2M\) represent at least 50% of the total fraudulent transactions                      | FALSE      |
| H4  | Customer to Customer \(C2C\) represent at least 50% of the total non fraudulent transactions                  | TRUE       |
| H5  | The average amount of a non fraudulent transaction is equivalent to 20% of the average amount of a fraudulent | FALSE      |
| H6  | Merchant destination represents 40% of the total fraudulent transactions                                      | FALSE      |
| H7  | Customer destination represents 40% of the total non fraudulent transactions                                  | FALSE      |
| H8  | Destination non zero new balance represents 50% of the total fraudulent transactions                          | TRUE       |
| H9  | Origin non zero old balance represents 80% of the total fraudulent transactions                               | FALSE      |
| H10 | Origin zero new balance represents 40% of the total fraudulent transactions                                   | FALSE      |
| H11 | At least 95% of the total fraudulent transactions are flagged as fraud                                        | FALSE      |
| H12 | At least 20% of the total non fraudulent transactions are flagged as fraud                                    | FALSE      |

<br>

### 3.1.3 Machine Learning

Tests were made using different algorithms.

#### Performance Metrics

![](img/ml_alg_comparison.png)

The <mark>**highlighted cells**</mark> correspond to the max value at each column.

As observed, for the context of our project, we want to maximize the precision and recall scores, so both metric are important for us. Thus, we can look for the highest f1-score. Looking at the results, `XGBClassifier` is the algorithm that satisfies our need.

#### Confusion Matrices

![](img/ml_alg_cm.png)

As observed, `XGBClassifier` gives us higher TP and TN rates while keeping the FP and, more importantly, the FN at minimum.

<br>

### 3.1.4 Business Performance

Let's recall the FBC business model.

- Receive 25% of the value of each transaction that is truly detected as fraud (true positives).
- Receive 5% of the value of each transaction detected as fraud, but the transaction is truly legitimate (false positives).
- Refund 100% of the value to the customer, for each transaction detected as legitimate, however the transaction is truly a fraud (false negatives).

Considering the parameters:
- **Median amount of a transaction:** \$7,4871.8 (we're using the median because the amount distribution is highly skewed)
- **Portfolio:** 6,362,620 transactions (fraudulent + non fraudulent)

#### True positive amount = [0.25 x (median amount) x (number of TP transactions)]
#### False positive amount = [0.05 x (median amount) x (number of FP transactions)]
#### False negative amount = [1 x (median amount) x (number of FN transactions)]

### Total revenue =  True positive amount + False positive amount - False negative amount

Doing the calculations, we have:

> Median transaction amount: $74,871.80

> Best scenario total revenue = $879,336,709.79

> Worst scenario total revenue = $1,404,441,971.97

Note that the worst scenario of our ML model gives us higher revenue, this is due to the FBC princing model and the higher number of FP that model generates.

<br>

### 3.1.5 Machine Learning Performance for the chosen algorithm

The chosen algorithm was the **XGBoost Classifier**. In addition, I made a performance calibration on it.

#### Precision, Recall, ROC AUC and other metrics

These are the metrics obtained from the test set.

| precision | recall  | f1\-score | roc auc | cohen kappa | accuracy |
|-----------|---------|-----------|---------|-------------|----------|
| 0\.8894   | 0\.9930 | 0\.9383   | 0\.9989 | 0\.9378     | 0\.9989  |

<br>

The summary below shows the metrics comparison after running a cross validation score with stratified K-Fold with 10 splits in the full data set.

| Algorithm                                 | avg precision             | avg recall                | avg f1\-score             | avg roc auc               |
|-------------------------------------------|---------------------------|---------------------------|---------------------------|---------------------------|
| XGBoost Classifier                        | 0\.9795 \(\+/\- 0\.0090\) | 0\.9447 \(\+/\- 0\.0087\)   | 0\.9618 \(\+/\- 0\.0065\) | 0\.9996 \(\+/\- 0\.0006\) |
| XGBoost Classifier \+ Tuned               | 0\.9819 \(\+/\- 0\.0075\) | 0\.9329 \(\+/\- 0\.0235\) | 0\.9568 \(\+/\- 0\.0128\) | 0\.9996 \(\+/\- 0\.0007\) |
| XGBoost Classifier \+ Tuned \+ Calibrated | 0\.9745 \(\+/\- 0\.0093\) | 0\.9570 \(\+/\- 0\.0114\) | 0\.9657 \(\+/\- 0\.0070\) | 0\.9995 \(\+/\- 0\.0010\) |

Although the **Tuned HP + Calibrated** model has a slightly lower precision, it has a higher recall and f1-score which is fair enough for our project needs. In addition, for being calibrated, in the future predictions it will be more stable and confident which is good for both businesses.

<br>

# 4.0 Next Steps

**4.1** **Develop an app** that intakes a portfolio of transactions and assigns for each transaction its respective probability of being fraudulent.

**4.2** **Run a Design Discovery** to uncover facts that could be missing in our analysis in order to enrich the data that we have and improve the model performance.

**4.3** Build a **model retraining pipeline**.

<br>

# References

### Context
- https://www.investopedia.com/terms/f/fraud.asp
- https://www.usi.edu/internalaudit/what-is-fraud/
- https://en.wikipedia.org/wiki/Financial_transaction
- https://cleartax.in/g/terms/cash-transaction
- https://www.investopedia.com/terms/d/debit.asp
- https://www.southpointfinancial.com/whats-difference-debit-credit/
- https://www.handbook.fca.org.uk/handbook/glossary/G3490p.html
- https://www.investopedia.com/terms/t/transfer.asp
- https://www.investopedia.com/financial-edge/0512/the-most-common-types-of-consumer-fraud.aspx
- https://www.intellias.com/how-to-use-machine-learning-in-fraud-detection/
- https://www.infosecurity-magazine.com/news/global-fraud-hits-32-trillion/
- https://www.tandfonline.com/doi/full/10.1080/21697213.2018.1480005

### Business case
- https://sejaumdatascientist.com/crie-uma-solucao-para-fraudes-em-transacoes-financeiras-usando-machine-learning/
