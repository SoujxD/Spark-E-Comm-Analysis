# 🛒 E-Commerce Data Analysis on Databricks

## 📋 Introduction
In this project, I conducted an in-depth analysis of an e-commerce dataset using PySpark and Pandas on Databricks. My key focus areas included:
1. 📊 **Customer distribution**: Understanding where most of the customers are located.
2. ⭐ **Customer reviews analysis**: Analyzing average review scores for products and sellers.
3. 🗺️ **Geolocation**: Exploring the geographic locations of customers and sellers.
4. ☁️ **AWS S3 integration**: Using AWS S3 for cloud storage of the dataset.
5. 🔥 **Databricks and PySpark**: Leveraging Databricks for scalable data processing using PySpark.

---

## 🧐 Background
The e-commerce industry generates vast amounts of data, and handling such large datasets requires a robust and scalable platform. I chose **Databricks** because of its seamless integration with Apache Spark, enabling efficient big data processing. Additionally, Databricks' compatibility with **AWS S3** made it easier to store and access data in the cloud.

### 📦 Datasets Used:
The dataset was sourced from Kaggle and includes:
- **Customers**: Information about customer locations (city, state).
- **Orders**: Details about customer orders, delivery times, and statuses.
- **Reviews**: Customer feedback on various products and sellers.
- **Geolocation**: Latitude and longitude data for customers and sellers.

---

## 🛠️ Tools I Used
For this project, I used the following tools:

- **AWS S3**: ☁️ Cloud storage to upload and access datasets efficiently.
- **Databricks**: 🧠 A unified platform for big data analytics and machine learning, integrated with Apache Spark.
- **PySpark**: 🚀 The Python API for Apache Spark, used to process large datasets.
- **Pandas**: 🐼 Python library for data manipulation and analysis.
- **Matplotlib & Seaborn**: 📈 Python libraries for creating beautiful data visualizations.

---

## 📝 Analysis

### Step 1: Importing Datasets
The datasets were uploaded to **AWS S3** and imported into Databricks using PySpark schema reading functionalities.

```python
# Reading data from S3 into DataFrame
customer_df = spark.read.csv("s3://bucket-name/customer_data.csv", header=True, inferSchema=True)
orders_df = spark.read.csv("s3://bucket-name/orders_data.csv", header=True, inferSchema=True)
```

# 🛒 E-Commerce Data Analysis on Databricks

## 📋 Introduction
In this project, I conducted an in-depth analysis of an e-commerce dataset using PySpark and Pandas on Databricks. My key focus areas included:
1. 📊 **Customer distribution**: Understanding where most of the customers are located.
2. ⭐ **Customer reviews analysis**: Analyzing average review scores for products and sellers.
3. 🗺️ **Geolocation**: Exploring the geographic locations of customers and sellers.
4. ☁️ **AWS S3 integration**: Using AWS S3 for cloud storage of the dataset.
5. 🔥 **Databricks and PySpark**: Leveraging Databricks for scalable data processing using PySpark.

---

## 🧐 Background
The e-commerce industry generates vast amounts of data, and handling such large datasets requires a robust and scalable platform. I chose **Databricks** because of its seamless integration with Apache Spark, enabling efficient big data processing. Additionally, Databricks' compatibility with **AWS S3** made it easier to store and access data in the cloud.

### 📦 Datasets Used:
The dataset was sourced from Kaggle and includes:
- **Customers**: Information about customer locations (city, state).
- **Orders**: Details about customer orders, delivery times, and statuses.
- **Reviews**: Customer feedback on various products and sellers.
- **Geolocation**: Latitude and longitude data for customers and sellers.

---

## 🛠️ Tools I Used
For this project, I used the following tools:

- **AWS S3**: ☁️ Cloud storage to upload and access datasets efficiently.
- **Databricks**: 🧠 A unified platform for big data analytics and machine learning, integrated with Apache Spark.
- **PySpark**: 🚀 The Python API for Apache Spark, used to process large datasets.
- **Pandas**: 🐼 Python library for data manipulation and analysis.
- **Matplotlib & Seaborn**: 📈 Python libraries for creating beautiful data visualizations.

---

## 📝 Analysis

### Step 1: Importing Datasets
The datasets were uploaded to **AWS S3** and imported into Databricks using PySpark schema reading functionalities.

```python
# Reading data from S3 into DataFrame
customer_df = spark.read.csv("s3://bucket-name/customer_data.csv", header=True, inferSchema=True)
orders_df = spark.read.csv("s3://bucket-name/orders_data.csv", header=True, inferSchema=True)
```
### Step 2: Performing Analysis

### 1. Customer Distribution by City and State 🏙️🌎
This analysis helps identify where most of the customers are located.

```python 
# Customer distribution by city
customer_city_dist = customer_df.groupBy("customer_city").count().orderBy(F.desc("count"))
customer_city_dist.show()

# Customer distribution by state
customer_state_dist = customer_df.groupBy("customer_state").count().orderBy(F.desc("count"))
customer_state_dist.show()

# Convert to pandas for visualization
customer_state_dist_pd = customer_state_dist.toPandas()

# Plot
plt.figure(figsize=(12, 6))
sns.barplot(x="customer_state", y="count", data=customer_state_dist_pd)
plt.title("Customer Distribution by State")
plt.xticks(rotation=90)
plt.show()
```
### 2. Customer Reviews Analysis ⭐🛍️

Analyzing the average rating score for both products and sellers.

```python 
# Join review_df with order_items_df to get product and seller information
review_join = review_df.join(order_items_df, "order_id", "inner")

# Calculate the average review score for each product
product_reviews = review_join.groupBy("product_id").agg(F.avg("review_score").alias("avg_review_score"))

# Calculate the average review score for each seller
seller_reviews = review_join.groupBy("seller_id").agg(F.avg("review_score").alias("avg_review_score"))

# Display
product_reviews.show()
seller_reviews.show()
```
### 3. Geolocation of Customers and Sellers 🌍📍

Analyzing the geographic locations of customers and sellers using their geolocation data.
```python 
# Join customer_df with geolocation_df on zip code
customer_geo = customer_df.join(geolocation_df, customer_df["customer_zip_code_prefix"] == geolocation_df["geolocation_zip_code_prefix"], "inner")
customer_geo.select("customer_city", "geolocation_lat", "geolocation_lng").show()

# Similarly for sellers
seller_geo = seller_df.join(geolocation_df, seller_df["seller_zip_code_prefix"] == geolocation_df["geolocation_zip_code_prefix"], "inner")
seller_geo.select("seller_city", "geolocation_lat", "geolocation_lng").show()
```

## 🎓 What I Learned
This project helped me gain experience in several areas:

- **AWS S3**: I used AWS S3 for reliable cloud storage and easy data transfer into Databricks.
- **Databricks and PySpark**: Databricks, combined with PySpark, was incredibly effective for handling and processing large datasets. Its distributed computing capabilities made big data analysis efficient.
- **Big Data Processing**: I became more proficient in managing large datasets, performing analysis using PySpark, and creating visualizations using Python libraries like Pandas and Seaborn.

## 📌 Conclusion
This project provided me with valuable insights into customer behavior, review trends, and geographic distributions in the e-commerce industry. Databricks and PySpark made big data analysis faster and more efficient, and I gained hands-on experience working with cloud-based storage and scalable data processing systems.

By analyzing this dataset, I was able to generate data-driven insights that could inform better business decisions. Overall, this project strengthened my skills in data analysis and working with modern data platforms. 🚀