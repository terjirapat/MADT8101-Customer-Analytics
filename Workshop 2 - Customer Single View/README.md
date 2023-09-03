# Customer Single View
:pushpin: **GOAL** : 
> Design Customer Single View

**CODE:** 
- [Main](./main.ipynb)

**DATA:**  
- Supermarket Sales Data

Table Structure

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/5841a9f2-0c28-420d-884d-562795e2bdc8)

#
### **What is a single customer view?**

Customer Single View consolidates customer data from various sources for a comprehensive understanding of customer interactions.

### **The benefits of a single customer view**

A single customer view is crucial for businesses, providing valuable information for all departments to improve performance. This data enables executives to make informed decisions, marketers to deliver profitable campaigns, and customer support agents to solve issues faster.

#

I designed this based on the RFM Framework. That will help to understand customers about Recency, Frequency, and Monetary

**Columns of my Customer Single View**

- Average basket size
- Average basket size in 3 months
- Average basket size in 6 months
- Total number of transactions
- Number of transactions in 3 months
- Number of transactions in 6 months
- Total spending
- Total spending in 3 months
- Total spending in 6 months
- Time between purchase
- Average weekly transaction
- Average weekly spending
- Member duration (Day)
- Last visit

This is my Python code to create Customer Single View

```python
# create single customer view
df_scv = df[['CUST_CODE']].drop_duplicates().reset_index(drop=True)

df_scv = df_scv.merge(
    df.groupby(['CUST_CODE', 'BASKET_ID']).agg(
    spend = ('SPEND', 'sum'),
    first_visit = ('SHOP_DATE', 'min'),
    last_visit = ('SHOP_DATE', 'max'),
    unique_week = ('SHOP_WEEK', 'max')).reset_index().groupby('CUST_CODE').agg(avg_bkt_size = ('spend', 'mean'), 
                                                                    total_trans = ('spend', 'count'),
                                                                    total_spend = ('spend', 'sum'),
                                                                    first_visit = ('first_visit', 'min'),
                                                                    last_visit = ('last_visit', 'max'),
                                                                    unique_week = ('unique_week', 'count')),
    how='left', on='CUST_CODE')

df_scv = df_scv.merge(
    df.loc[df['SHOP_MONTH'] > (df['SHOP_MONTH'].max() - 3), :].groupby(['CUST_CODE', 'BASKET_ID']).agg(
    spend = ('SPEND', 'sum')).reset_index().groupby('CUST_CODE').agg(avg_bkt_size_3m = ('spend', 'mean'), 
                                                                    total_trans_3m = ('spend', 'count'),
                                                                    total_spend_3m = ('spend', 'sum')),
    how='left', on='CUST_CODE')

df_scv = df_scv.merge(
    df.loc[df['SHOP_MONTH'] > (df['SHOP_MONTH'].max() - 6), :].groupby(['CUST_CODE', 'BASKET_ID']).agg(
    spend = ('SPEND', 'sum')).reset_index().groupby('CUST_CODE').agg(avg_bkt_size_6m = ('spend', 'mean'), 
                                                                    total_trans_6m = ('spend', 'count'),
                                                                    total_spend_6m = ('spend', 'sum')),
    how='left', on='CUST_CODE')

df_scv['tbp'] = (df_scv['last_visit']-df_scv['first_visit']).dt.days / df_scv['total_trans']
df_scv['mem_dur'] = (df['SHOP_DATE'].max()-df_scv['first_visit']).dt.days
df_scv['avg_weekly_trans'] = df_scv['total_trans'] / df_scv['unique_week']
df_scv['avg_weekly_spend'] = df_scv['total_spend'] / df_scv['unique_week']

df_scv.drop(columns=['unique_week', 'first_visit'], inplace=True)

df_scv.tail(3)
```

Customer Single View example

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/3499bebe-a934-42d8-a792-66c7d346c2e2)
