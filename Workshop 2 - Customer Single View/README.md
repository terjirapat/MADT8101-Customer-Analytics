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

## EDA

It was discovered that 27% of a total of 3,439 customers purchased only once and never returned.

I divided the customers into two groups in order to analyze them separately.

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/48a8adfb-90dd-4099-be5b-254ce7df232a)

# Customer Segmentation

## One basket customer

### **Framework: The 80/20 rule**

From the 80/20 rule, 20% of the customers represent 80% of total sales.

Finding 20% of the customers

<img src="https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/cb9e233a-1a80-4d31-bac3-db701e3d1e3c" height="300" width="400"><br /><br />

### Result

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/edc1e9cc-24a6-4e8f-babd-d4c902455e86)

The table of the customers who represent 80% of total sales.

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/9c1646b2-8e3e-4066-9a47-4fd31e452c3c)

**Action Suggestions**: Push them to make further purchases.

**Next steps suggested**: Move them to the 'more than one basket customer' group and make recommendations based on their cluster

#

## More than one basket customer

### **Clustering Method: K-mean clustering**
### **Normalization Method: Z-score normalization**

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

### Determining the optimal number of clusters

Choose k=3

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/c808fd75-b767-4c25-99f6-836220f3c508)

### Clustering result

**PCA Scatter plot**

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/c394f8af-7103-405e-b9ed-cef3505ef4ae)

### EDA

**Number of Customers in each Cluster**

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/e95e90b8-98ea-43e8-85a6-5a69edef2893)

**Box plot**

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/976da07f-99b8-42a8-8d3c-0b5546d66d96)

**Histogram**

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/a8d98144-a1de-4429-9f90-065126c6ce3c)

### Interpretation

**Feature Important**

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/d3554544-bea9-49d1-9fbe-cb4ce109001b)

**Median value of each feature by cluster**

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/67936934-c4fa-4753-92ee-8918fd448231)
![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/2a6c79dc-e9cb-412e-b56b-3bf0a38575e8)

### **Cluster 0: Inactive Minimal Spenders**

**Description**: This cluster consists of customers who have made very few or no purchases in the past 6 months and have a low average basket size.

**Suggestions**: Consider re-engagement strategies, such as personalized email offers, reminders, or incentives to encourage them to make a purchase.

**Next Steps**: 

- Segment this cluster further to understand if there are specific reasons for inactivity and address those issues.

- Continuously monitor and analyze their behavior to adjust your re-engagement efforts.

### **Cluster 1: High-Value Occasional Shoppers**

**Description**: This cluster represents customers who make occasional but high-value purchases.

**Action Suggestions**: Recognize and reward their loyalty with exclusive offers or loyalty programs.

**Next steps suggested**: 

- Collect more data to understand the factors that trigger their purchases and tailor marketing efforts accordingly.

- Encourage them to become more frequent shoppers by offering incentives for repeat purchases.

### **Cluster 2: Frequent Moderate Spenders**

**Description**: This cluster comprises customers who make frequent purchases with moderate spending.

**Action Suggestions**: Focus on upselling and cross-selling related products to increase their average basket size.

**Next steps suggested**:

- Implement targeted marketing campaigns based on their purchase history to encourage them to buy complementary products.

- Monitor their behavior to ensure they remain engaged and continue to make purchases.

# Product Recommendation

### **Recommendation Method: Association rule**

Product recommendation from association rule on column 'PROD_CODE_10' by cluster

### Cluster 0

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/677969df-e130-486f-a7e3-aefa2ed824fe)

### Cluster 1

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/0ec4c32e-c568-460c-b07e-17ecffb3400a)

### Cluster 2

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/815eec60-767a-480f-86b0-069b135578c5)









