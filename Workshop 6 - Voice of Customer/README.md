# Voice of the customer

![image](https://github.com/terjirapat/MADT8101-Customer-Analytics/assets/77285026/58d58bf0-0685-4003-9af3-0a01beaf9fad)

:round_pushpin: **GOAL :** 
- Sentiment Analysis of the Voice of the Customers

**CODE:** 
- [Code](./main.ipynb)

**DATA:**  
- [ครกไม้ไทยลาว (Krok Mai Thai Lao)](https://www.wongnai.com/r/12231Lf) restaurant customers review from Wongnai

**METHOD:**  
- Topic Modeling with LDA
- Document clustering using K-mean and Cosine Similarity

#

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

```
Cluster ID : 0

Most common words include : [('อาหาร', 4), ('คน', 4), ('ไม้', 2), ('สาขา', 2), ('สั่ง', 2), ('ดี', 2), ('เมนู', 2), ('โต๊ะ', 2), ('รถ', 1), ('ศูนย์', 1)]

Cluster ID : 1

Most common words include : [('อาหาร', 11), ('รสชาติ', 6), ('อีสาน', 4), ('กิน', 3), ('วัตถุ', 2), ('แซ่บ', 2), ('เมนู', 2), ('อร่อย', 2), ('สัมผัส', 1), ('ดิบ', 1)]

Cluster ID : 2

Most common words include : [('ทาน', 3), ('ส้มตำลาว', 2), ('พน', 2), ('ครก', 2), ('ส้มตำมั่ว', 1), ('ซั่ว', 1), ('ไม้', 1), ('เด็ด', 1), ('ปี', 1), ('ลอง', 1)]

```


### Cosine Similarity

```
Cluster ID : 0

Most common words include :[('อาหาร', 35), ('อร่อย', 25), ('เมนู', 24), ('ดี', 19), ('ย่าง', 16), ('คน', 15), ('รสชาติ', 15), ('ทาน', 13), ('ไข่', 12), ('สั่ง', 11)]

Cluster ID : 1

Most common words include :[('อาหาร', 3), ('ดี', 2), ('โต๊ะ', 2), ('โปร่ง', 1), ('นั่ง', 1), ('ไม้', 1), ('พนักงาน', 1), ('ตอน', 1), ('คน', 1), ('เมนู', 1)]

Cluster ID : 2

Most common words include :[('แวะ', 1), ('กิน', 1), ('รสชาติ', 1), ('อาหาร', 1), ('เหมือน', 1), ('อัด', 1), ('ผงชูรส', 1), ('เค็ม', 1), ('ไก่', 1), ('ย่าง', 1)]
```

In this case, Cosine Similarity can clearly define the difference between group

It is possible to assign names to the clusters using the Cosine Similarity as follows

Cluster 0: Liked Food

Cluster 1: Good Service and Atmosphere

Cluster 2: Disliked Food

