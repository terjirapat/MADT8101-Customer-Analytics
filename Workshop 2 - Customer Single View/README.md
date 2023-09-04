# Customer Single View
:pushpin: **GOAL** : 
> Design Customer Single View

**CODE:** 
- [Main](./main.ipynb)

**DATA:**  
- Supermarket Sales Data

Table structure of supermarket sales data 

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/5841a9f2-0c28-420d-884d-562795e2bdc8)

This dataset contains a row of sales transaction data at the product level

#
### **What is a single customer view?**

Customer Single View consolidates customer data from various sources for a comprehensive understanding of customer interactions.

### **The benefits of a single customer view**

A single customer view is crucial for businesses, providing valuable information for all departments to improve performance. This data enables executives to make informed decisions, marketers to deliver profitable campaigns, and customer support agents to solve issues faster.

#

I designed this based on the RFM Framework. That will help to understand customers about Recency, Frequency, and Monetary

**Columns of my Customer Single View**

- Average basket size
- Average basket size in 3 month
- Average basket size in 6 month
- Total number of transactions
- Number of transactions in 3 month
- Number of transactions in 6 month
- Total spending
- Total spending in 3 month
- Total spending in 6 month
- Time between purchase
- Spending per day
- Transaction per day
- Member duration (Day)
- Last visit
- Most buying product

This is my Python code to create a Customer Single View

Find the most bought product for each customer
```python
# Find the most bought product for each customer
cust_dict = {'CUST_CODE':[],
            'most_buying_pdt':[]}

cust_pdt = df.groupby(['CUST_CODE','PROD_CODE'])['PROD_CODE'].agg(qty='count').reset_index()

for i in cust_pdt['CUST_CODE'].unique():
    cust_dict['CUST_CODE'].append(i)
    cust = cust_pdt.loc[cust_pdt['CUST_CODE']==i,:]

    if len(cust.loc[cust['qty']==cust['qty'].max()]['PROD_CODE']) == 1:
        cust_dict['most_buying_pdt'].append(cust.loc[cust['qty']==cust['qty'].max()]['PROD_CODE'].reset_index(drop=True)[0])
    elif len(cust.loc[cust['qty']==cust['qty'].max()]['PROD_CODE']) > 1:
        # if most buying pdt more than one convert to list
        cust_dict['most_buying_pdt'].append(list(cust.loc[cust['qty']==cust['qty'].max()]['PROD_CODE']))
```

Aggregate others features

```python
# Create a single customer view
df_scv = df[['CUST_CODE']].drop_duplicates().reset_index(drop=True)

# aggregate for the total period
df_scv = df_scv.merge(
    df.groupby(['CUST_CODE', 'BASKET_ID']).agg(
    spend = ('SPEND', 'sum'),
    first_visit = ('SHOP_DATE', 'min'),
    last_visit = ('SHOP_DATE', 'max')).reset_index().groupby('CUST_CODE').agg(avg_bkt_size = ('spend', 'mean'), 
                                                                    total_trans = ('spend', 'count'),
                                                                    total_spend = ('spend', 'sum'),
                                                                    first_visit = ('first_visit', 'min'),
                                                                    last_visit = ('last_visit', 'max')),
    how='left', on='CUST_CODE')

# aggregate for 3 month period
df_scv = df_scv.merge(
    df.loc[df['SHOP_MONTH'] > (df['SHOP_MONTH'].max() - 3), :].groupby(['CUST_CODE', 'BASKET_ID']).agg(
    spend = ('SPEND', 'sum')).reset_index().groupby('CUST_CODE').agg(avg_bkt_size_3m = ('spend', 'mean'), 
                                                                    total_trans_3m = ('spend', 'count'),
                                                                    total_spend_3m = ('spend', 'sum')),
    how='left', on='CUST_CODE')

# aggregate for 6 month period
df_scv = df_scv.merge(
    df.loc[df['SHOP_MONTH'] > (df['SHOP_MONTH'].max() - 6), :].groupby(['CUST_CODE', 'BASKET_ID']).agg(
    spend = ('SPEND', 'sum')).reset_index().groupby('CUST_CODE').agg(avg_bkt_size_6m = ('spend', 'mean'), 
                                                                    total_trans_6m = ('spend', 'count'),
                                                                    total_spend_6m = ('spend', 'sum')),
    how='left', on='CUST_CODE')

df_scv['tbp'] = (df_scv['last_visit']-df_scv['first_visit']).dt.days / df_scv['total_trans']
df_scv['mem_dur'] = (df['SHOP_DATE'].max()-df_scv['first_visit']).dt.days
df_scv['spending_per_day'] = df_scv['total_spend']/df_scv['mem_dur']
df_scv['trans_per_day'] = df_scv['total_trans']/df_scv['mem_dur']

# Join with most buying pdt DF
df_scv = df_scv.merge(pd.DataFrame(cust_dict), how='inner', on='CUST_CODE')

# drop column
df_scv.drop(columns=['first_visit'], inplace=True)

df_scv = df_scv.fillna(0)

display(df_scv)
```

Table of Customer Single View

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/56612df4-a888-45ce-adc7-a5439590b12e)

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/a1c60d20-3b31-4aa3-bd3b-ca11dd91c147)

#

# Customer Segmentation and 

## EDA

It was discovered that 27% of a total of 3,439 customers purchased only once and never returned.

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/0a3856ca-ffdd-4482-92ae-3ebba81126d6)

So I decided to cluster two times 
- First one basket customer
- Second more than one basket customer

## Customer Segmentation

**Method: K-mean clustering**

#

### One basket customer

**Feature**

- Average basket size
- Total spending

#

### More than one basket customer

**Feature**

- Average basket size
- Average basket size in 3 month
- Average basket size in 6 month
- Total number of transactions
- Number of transactions in 3 month
- Number of transactions in 6 month
- Total spending
- Total spending in 3 month
- Total spending in 6 month
- Time between purchase
- Spending per day
- Transaction per day

# Product Recommendation

**Method: Association rule**








