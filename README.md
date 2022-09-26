# Brazil-Ecommerce-Data-Analysis

## **Business Problem Understanding**

**Context**

With the development of technology, everything we need can be fulfilled from cellphones everywhere and anytime you want as long as there is an internet connection. Our needs can be fulfilled efficiently. As the demand grows, a lot of sellers develop their selling, buying and transaction for their product system to online.

As technology developed and gives profit to shareholder, new entrants and competitors has emerge within the incumbent. As Brazil is one of the fastest growth e-commerce industry in Latin America and our company as a marketplace which provide people a place to sell their product based in Brazil, and  also in order for the company to compete in the industry we need to make an improvement by analyzing the data we have and get insights out of it. 

## **Data Understanding**

**Customers Dataset**

| **Attribute** | **Description** |
| --- | --- | 
| customer_id | Customer ID per order |
| customer_unique_id | Unique ID of customer |
| customer_zip_code_prefix | Customer location's zip code |
| customer_city | Customer's city location |
| customer_state | The city's state |

**Geolocation Dataset**

| **Attribute** | **Description** |
| --- | --- | 
| geolocation_zip_code_prefix | Zip code's location |
| geolocation_lat | Latitude coordinate |
| geolocation_lng | Longitude coordinate |
| geolocation_city | City name |
| geolocation_state | The city's state |

**Order Items Dataset**

| **Attribute** | **Description** |
| --- | --- | 
| order_id | Order unique ID |
| order_item_id | Sequential number for items in the same order |
| product_id | Product unique ID |
| seller_id | Seller unique ID |
| shipping_limit_date | Seller limit shipping date |
| price | item price |
| freight_value | transportation cost |

**Order Payments Dataset**

| **Attribute** | **Description** |
| --- | --- | 
| order_id | Order unique ID |
| payment_sequential | Sequential number for customer payment method |
| payment_type | Customer payment method |
| payment_installments | number of installments |
| payment_value | transaction value |

**Order Reviews Dataset**

| **Attribute** | **Description** |
| --- | --- | 
| review_id | Review unique ID |
| order_id | Unique order ID |
| review_score | Customer satisfaction survey ranging 1 to 5 |
| review_comment_title | Comment title made by customer |
| review_comment_message | comment message made by customer |
| review_creation_date | The timestamp in which the survey was sent to the customer |
| review_answer_timestamp | The timestamp in which the customer answer the satisfaction survey |

**Orders Dataset**

| **Attribute** | **Description** |
| --- | --- | 
| order_id | Order unique ID |
| customer_id | Customer unique ID |
| order_status | Order status |
| order_purchase_timestamp | Purchase timestamp |
| order_approved_at | Payment approval timestamp |
| order_delivered_carrier_date | The timestamp in which the order was handled to the logistic division/partner |
| order_delivered_customer_date | The delivery date to the customer |
| order_estimated_delivery_date | Estimated delivery date that was showed to customer |

**Products Dataset**

| **Attribute** | **Description** |
| --- | --- | 
| product_id | Product unique ID |
| product_category_name | Category of product |
| product_name_lenght | Number of characters from the product  |
| product_description_lenght | Number of characters from the product description |
| product_photos_qty | Number of photo's product |
| product_weight_g | Product weight in gram |
| product_length_cm | Product length in centimeter |
| product_height_cm | Producty height in centimeter |
| product_width_cm | Producty width in centimeter |

**Sellers Dataset**

| **Attribute** | **Description** |
| --- | --- | 
| seller_id | Seller unique ID |
| seller_zip_code_prefix | Seller location zip code |
| seller_city | Seller city location  |
| seller_state | The state where the city located |

#### The Relationship Between Datasets

![image](https://user-images.githubusercontent.com/99155979/192191557-85ebbcec-de4d-4406-80a1-beb4d2c46913.png)

## Data Cleaning

```python
#import libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')
import dateparser
```

##### Read Datasets

```python
# Customers Dataset
df = pd.read_csv('p_customers_dataset.csv')

# Geolocation Dataset
df2 = pd.read_csv('p_geolocation_dataset.csv')

# Order Items Dataset
df3 = pd.read_csv('p_order_items_dataset.csv')

# Order Payments Dataset
df4 = pd.read_csv('p_order_payments_dataset.csv')

# Order Reviews Dataset
df5 = pd.read_csv('p_order_reviews_dataset.csv')

# Orders Dataset
df6 = pd.read_csv('p_orders_dataset.csv')

# Products Dataset
df7 = pd.read_csv('p_products_dataset.csv')

# Sellers Dataset
df8 = pd.read_csv('p_sellers_dataset.csv')
```

##### Check Missing Value, Data Type, Drop Duplicate

```python
# Customers Dataset
Desc = []
for i in df.columns:
    Desc.append([
        i,
        df[i].dtypes, #check data type
        df[i].isna().sum(),#check missing value
        round(((df[i].isna().sum() / len(df)) * 100), 2), # check percentage of the missing value
        df[i].nunique(), #check the number of the unique value
        df[i].drop_duplicates().sample(2).values #drop duplicates data, show two samples of the data
    ])

pd.DataFrame(Desc, columns=[
    "Data Features",
    "Data Types",
    "Null",
    "Null Percentage",
    "Unique",
    "Unique Sample"
])
```

<img width="559" alt="image" src="https://user-images.githubusercontent.com/99155979/192192045-cd4d07fc-ce22-41e2-94b9-06412d4e926f.png">

```python
#No unsuitable data types
#No missing value
```

```python
# Order Dataset
Desc6 = []
for i in df6.columns:
    Desc6.append([
        i,
        df6[i].dtypes, #check data type
        df6[i].isna().sum(),#check missing value
        round(((df6[i].isna().sum() / len(df6)) * 100), 2), # check percentage of the missing value
        df6[i].nunique(), #check the number of the unique value
        df6[i].drop_duplicates().sample(2).values #drop duplicates data, show two samples of the data
    ])

pd.DataFrame(Desc6, columns=[
    "Data Features",
    "Data Types",
    "Null",
    "Null Percentage",
    "Unique",
    "Unique Sample"
])
```
<img width="572" alt="image" src="https://user-images.githubusercontent.com/99155979/192192452-62dc89b5-cdf8-4a2a-87b6-bbc9c878f99f.png">

```python
#Unsuitable data types on features: 
# -order_purchase_timestamp
# -order_approved_at 
# -order_delivered_carrier_date
# -order_delivered_customer_date
# -order_estimated_delivery_date

#Missing value on features: 
# -order_approved_at
# -order_delivered_carrier_date 
# -order_delivered_customer_date
```

```python
#convert data types from object to date time
df6['order_purchase_timestamp'] = pd.to_datetime(df6['order_purchase_timestamp'])
df6['order_approved_at'] = pd.to_datetime(df6['order_approved_at'])
df6['order_delivered_carrier_date'] = pd.to_datetime(df6['order_delivered_carrier_date'])
df6['order_delivered_customer_date'] = pd.to_datetime(df6['order_delivered_customer_date'])
df6['order_estimated_delivery_date'] = pd.to_datetime(df6['order_estimated_delivery_date'])

#drop data rows with missing value
df6.dropna(inplace = True)

df6.info()
```
<img width="374" alt="image" src="https://user-images.githubusercontent.com/99155979/192192578-44397036-0b1a-4892-af53-2cef07a1eb8c.png">

```python
time = df6['order_purchase_timestamp'].dt #datetime index
df6['Month'] = time.month_name() #adding new feature month
df6['Day'] = time.day_name() #adding new feature day
df6['Hour'] = time.hour #adding new feature hour
df6
```
<img width="588" alt="image" src="https://user-images.githubusercontent.com/99155979/192192657-35b8499c-cf6b-47d9-bb43-9e814b1206d7.png">

## Exploratory Data Analysis

#### Sales Trend

<img width="502" alt="image" src="https://user-images.githubusercontent.com/99155979/192193044-5ece18c5-ff69-4044-858e-379129e6096b.png">

<img width="515" alt="download" src="https://user-images.githubusercontent.com/99155979/192192732-c6a746a8-cd28-4914-9312-8b8569cd57d5.png">

From the graphs and bar chart above we can conclude there's an anomali sales at 24th of November. As I did some research People in Brazil celebrate an event at 24th of November in 2017 which is Black Friday. After 24th of November sales relatively going down but still higher than the sales before the Event. As we can also see the sales magnificently increasing after Black Friday.

<img width="510" alt="download" src="https://user-images.githubusercontent.com/99155979/192193270-2d535620-4e1f-46c6-b06f-35f1c1df3e71.png">

The company should make a campaign every year on Black Friday event to get more profit and also more engagement, but it should be emphasised that Black Friday in every year is not always on the same date.

#### Sales Percentage Outside Black Friday

<img width="505" alt="image" src="https://user-images.githubusercontent.com/99155979/192193574-3cdeb24d-64e4-4dd1-81db-8e27430a60f9.png">

How about monthly and daily orders outside Black Friday Event?

These two pie charts only including sales in 2017 and it is not include 24th of November. 
From those pie charts we conclude there is not much difference and the highest sales still on November follow by December as it is after the month of Black Friday also Christmast on December.

There is not much difference on daily sales, but is is unique as we can see people in Brazil tend to buy or order things at the beginning of the week where Tuesday has the most percentage of orders follow by Monday.

A campaign at the beginning of the week could make a good promotion besides A big campaign such as Black Friday. Based on the chart probably it is not going to affect sales significantly, but it's worth to try.

![image](https://user-images.githubusercontent.com/99155979/192193665-206d5f3b-e89a-4623-b330-f41b65802842.png)

At what time people purchase their needs?

I divide times into four period:
- Morning: 05.00-11.00
- Afternoon: 12.00-17.00
- Evening: 18.00-22.00
- Night: 23.00-04.00

From the chart above we can see people tend more to buy their needs in the afternoon. The company could make a campaign at that time also combine the campaign with the campaign at the beginning of the week.

#### Where do the majority customers live?

<img width="503" alt="image" src="https://user-images.githubusercontent.com/99155979/192192008-260a648d-431e-4717-9fc0-b861448f324e.png">

Customers live spread around South America and most of them are based in Brazil, it's make sense as the company also based in Brazil. From the chart we can see majority customers live in Sao Paulo with 37,26% followed by Rio de Janeiro with 19,99% and Minas Gerais with 19,09%.

Company should make campaign based on that. For example if customer order from Rio de Janeiro and Minas Gerais they got discount on transportation fee, and if customer order from Rio de Janeiro they got free transportation fee.
