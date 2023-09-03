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

**Result**

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/52d50c96-731c-4368-8a2d-84e022e83c66)

#

I apply the 80/20 rule by finding 20% of customers who spend 80% of total sales but tend to churn in order to reduce the churn rate of these customers

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/f5103032-52c2-4ba7-b442-e077f05b31c8)

These are some of the customers that match the rule and tend to churn in the next month

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/91217796-cb72-43cc-a4a7-edaa41233b4f)


