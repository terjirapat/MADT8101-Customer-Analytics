![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/58d58bf0-0685-4003-9af3-0a01beaf9fad)

[Code](./main.ipynb)

Goal: Sentiment Analysis

Data: [ครกไม้ไทยลาว (Krok Mai Thai Lao)](https://www.wongnai.com/r/12231Lf) restaurant customers review from Wongnai

Method: 
- Topic Modeling with LDA
- Document clustering using K-mean and Cosine Similarity

## Topic Modeling using LDA

number of topic = 3

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/072ead4c-e76c-4cfe-a470-b80fe2158618)
![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/57878762-2b1a-420c-b148-ea104c14ddf6)
![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/e6032eec-bebb-4e2d-95d8-672832aca3ac)

As most of the reviews are positive about the food and deliciousness. and a small number of reviews Therefore, no differences between the groups could be found. 

The same keyword in the three groups is 'อาหาร', 'อร่อย'

Obtained an understanding that customers have a positive opinion of the food.

##  Document clustering

### K-mean

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/9025657e-5ef3-44ac-b293-b3a5ae1c9cac)

### Cosine Similarity

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/78935580-b781-451b-80ce-73e102986640)

In this case, Cosine Similarity can clearly define the difference between group

It is possible to assign names to the clusters using the Cosine Similarity as follows

Cluster 0: Liked Food

Cluster 1: Good Service and Atmosphere

Cluster 2: Disliked Food
