# Sales Data Analysis Project

## Overview

This project analyzes sales data to provide insights into spending patterns using PySpark. The analysis covers various aspects such as total spending by country, order source, and customer behavior. The project includes data loading, processing, and visualization using Python libraries.

## Getting Started

To get started with this project, follow these instructions to set up your environment.

### Prerequisites

- Apache Spark (PySpark)
- Python 3.x
- Necessary Python libraries (see Installation section)

### Initial Setup

1. **Import Libraries and Initialize Spark Session**

```python
from pyspark.sql import SparkSession

# Create a Spark session
spark = SparkSession.builder \
    .appName("Sales Data Analysis") \
    .getOrCreate()
Load Data
python
Copy code
# Define the schema for the sales data
schema = "customer_id STRING, product_id STRING, order_date DATE, source_order STRING, location STRING, price DOUBLE"

# Load the sales data
sales_df = spark.read.format("csv") \
    .option("inferschema", "true") \
    .schema(schema) \
    .load("/home/bigdata/Desktop/pyspark project/sales.csv.txt")

# Display the DataFrame
display(sales_df)

Data Analysis
This project includes various analyses performed on the sales data. Below are some key steps:

Total Amount Spent by Country

total_amount_spent_By_Country = (sales_df
    .join(menu_df, 'product_id')
    .groupBy('location')
    .agg({'price': 'sum'})
)

total_amount_spent_By_Country.show()
Total Amount Spent by Order Source

total_amount_spent_By_order_source = (sales_df
    .join(menu_df, 'product_id')
    .groupBy('source_order')
    .agg({'price': 'sum'})
)

total_amount_spent_By_order_source.show()
Visualizations
Bar Chart for Total Amount Spent by Country

import matplotlib.pyplot as plt

# Collect the data
data = total_amount_spent_By_Country.collect()

# Prepare data for plotting
countries = [row['location'] for row in data]
total_spent = [row['sum(price)'] for row in data]

# Create a bar chart
plt.figure(figsize=(12, 6))
plt.bar(countries, total_spent, color='yellow')
plt.title('Total Amount Spent by Country')
plt.xlabel('Country')
plt.ylabel('Total Amount Spent')
plt.xticks(rotation=45)
plt.show()
Pie Chart for Total Amount Spent by Order Source

# Prepare data for pie chart
data = total_amount_spent_By_order_source.collect()
order_sources = [row['source_order'] for row in data]
total_spent = [row['sum(price)'] for row in data]

# Create a pie chart
plt.figure(figsize=(8, 8))
plt.pie(total_spent, labels=order_sources, autopct='%1.1f%%', startangle=90)
plt.title('Total Amount Spent by Order Source')
plt.show()
