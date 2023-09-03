# Churn Scoring
:round_pushpin: **GOAL:**
> Define Churn Customer

**CODE:** 
- [Main](./main.ipynb)

**DATA:**  
- Supermarket Sales Data

**METHOD:**
- Cohort Analysis
- Customer Movement Analysis
- Churn Prediction

#

Period: 2006/04 - 2008/06

## Cohort Analysis

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/91e74e55-4328-486b-b3d2-5887dd457fab)

Based on the Cohort analysis It was discovered that approximately 10% of new customers will continue to use the service after around 4 months, implying that up to 90% will have churned.

## Customer Movement Analysis

Customer movement analysis shows customer behavior in terms of new, repeat, reactivated, and churn customers compared to the previous month

For each reporting month, customers are grouped into 4 categories defined by the definition below

| Status | Current | Previous | Before |
| --- | --- | --- | --- |
| Repeat | ✅ | ✅ | |
| Reactivated | ✅ | ❌ | ✅ |
| New | ✅ | ❌ | ❌ |
| Churn | ❌ | ✅ | |

Current: made purchases this month (M)

Previous: made purchases last month (M-1)

Before: made purchase before last month (< M-1)

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/bcad3657-eb0a-4caa-b789-d080c63bfd1c)

## Churn Prediction

Using propensity to purchase to predict churn customers

Train period: 2008/01 - 2008/03

Test period: 2008/04

**Feature**

- Average basket size (3 months)
- Average basket size in 1 month
- Average basket size in 2 months
- Total number of transactions (3 months)
- Number of transactions in 1 month
- Number of transactions in 2 months
- Total spending (3 months)
- Total spending in 1 month
- Total spending in 2 months
- Number of unique dates
- Number of unique weeks
- Time between purchase
- Recency

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/8a6ef590-d23d-4555-b38d-38807e5f509f)

**Feature Importance**

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/e8e27d51-b565-4d96-a35b-7ae7e6119885)

**Evaulation**

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/52d50c96-731c-4368-8a2d-84e022e83c66)

#

In order to reduce the churn rate of high-value customers, I apply the 80/20 rule by identifying 20% of customers who spend 80% of total sales but churn.

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/f5103032-52c2-4ba7-b442-e077f05b31c8)

This is a table of the 20% of customers who spend 80% of total sales.

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/94b59af5-d422-43f5-8ee4-99d1fe1109fa)


and this is a table of the 20% of customers who are likely to churn in the next month.

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/7e1430c3-e694-4d88-984d-89d59f237f57)


