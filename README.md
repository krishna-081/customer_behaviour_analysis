# Customer Behavior Analytics Dashboard

## Project Overview

This project focuses on analyzing customer shopping behavior using Python, PostgreSQL, SQL, and Power BI. The goal of the project is to clean customer purchase data, store it in a PostgreSQL database, perform SQL-based analysis, and build an interactive Power BI dashboard for business insights.

The dashboard helps understand customer demographics, purchase patterns, product category performance, subscription behavior, review ratings, shipping preferences, and revenue distribution.

---

## Tools and Technologies Used

* Python
* Pandas
* Jupyter Notebook
* PostgreSQL
* pgAdmin
* SQL
* Power BI
* GitHub

---

## Project Workflow

1. Collected customer shopping behavior dataset
2. Loaded the dataset into Jupyter Notebook
3. Cleaned and transformed the data using Python and Pandas
4. Created new analytical columns such as:

   * age_group
   * purchase_frequency_days
5. Connected Python with PostgreSQL using SQLAlchemy and pg8000
6. Loaded the cleaned dataframe into PostgreSQL
7. Performed SQL queries for business analysis
8. Connected PostgreSQL database to Power BI
9. Built an interactive customer behavior dashboard
10. Created insights for business decision-making

---

## Dataset Description

The dataset contains customer shopping behavior information such as:

* Customer ID
* Age
* Gender
* Item Purchased
* Product Category
* Purchase Amount
* Location
* Size
* Color
* Season
* Review Rating
* Subscription Status
* Shipping Type
* Discount Applied
* Previous Purchases
* Payment Method
* Frequency of Purchases

---

## Data Cleaning and Transformation

The dataset was cleaned using Python and Pandas.

Main cleaning steps included:

* Standardized column names
* Converted column names to lowercase
* Replaced spaces with underscores
* Renamed purchase amount column
* Created age group column
* Created purchase frequency days column
* Removed unnecessary duplicate columns
* Prepared the dataset for PostgreSQL import

Example transformation:

```python
df.columns = df.columns.str.strip().str.lower().str.replace(" ", "_", regex=False)

df = df.rename(columns={"purchase_amount_(usd)": "purchase_amount"})
```

Age group creation:

```python
labels = ['Young Adult', 'Adult', 'Middle-aged', 'Senior']

df['age_group'] = pd.qcut(
    df['age'],
    q=4,
    labels=labels
)
```

Purchase frequency mapping:

```python
frequency_mapping = {
    'Fortnightly': 14,
    'Weekly': 7,
    'Monthly': 30,
    'Quarterly': 90,
    'Bi-Weekly': 14,
    'Annually': 365,
    'Every 3 Months': 90
}

df['purchase_frequency_days'] = df['frequency_of_purchases'].map(frequency_mapping)
```

---

## PostgreSQL Integration

The cleaned dataset was loaded into PostgreSQL using Python.

Database used:

```text
customer_behaviour
```

Table created:

```text
customer_shopping
```

Python connection was created using SQLAlchemy and pg8000.

```python
from sqlalchemy import create_engine
from sqlalchemy.engine import URL

url = URL.create(
    drivername="postgresql+pg8000",
    username="postgres",
    password="YOUR_PASSWORD",
    host="localhost",
    port=5432,
    database="customer_behaviour"
)

engine = create_engine(url)
```

Dataframe uploaded to PostgreSQL:

```python
df.to_sql(
    "customer_shopping",
    engine,
    if_exists="replace",
    index=False
)
```

---

## SQL Analysis

Several SQL queries were performed to analyze the data.

Example queries:

### Total Records

```sql
SELECT COUNT(*)
FROM customer_shopping;
```

### Revenue by Category

```sql
SELECT 
    category,
    SUM(purchase_amount) AS total_revenue
FROM customer_shopping
GROUP BY category
ORDER BY total_revenue DESC;
```

### Average Product Rating

```sql
SELECT 
    item_purchased,
    ROUND(AVG(review_rating)::numeric, 2) AS average_product_rating
FROM customer_shopping
GROUP BY item_purchased
ORDER BY average_product_rating DESC
LIMIT 5;
```

### Revenue by Age Group

```sql
SELECT 
    age_group,
    SUM(purchase_amount) AS total_revenue
FROM customer_shopping
GROUP BY age_group
ORDER BY total_revenue DESC;
```

---

## Power BI Dashboard

The cleaned PostgreSQL data was connected directly to Power BI.

Power BI connection:

```text
Get Data → PostgreSQL Database → localhost → customer_behaviour
```

Dashboard visuals include:

* Number of customers
* Average purchase amount
* Average review rating
* Subscription status percentage
* Revenue by category
* Sales by category
* Customers by age group
* Revenue by age group
* Gender slicer
* Subscription status slicer
* Category slicer
* Shipping type slicer

---

## Dashboard Preview

Add your dashboard screenshot here:

```markdown
![Customer Behavior Dashboard](images/customer_behavior_dashboard.png)
```

---

## Key Business Insights

* Clothing generated the highest revenue among all categories.
* Accessories showed strong sales performance after clothing.
* Most customers were not subscribed.
* Young Adults contributed the highest revenue among age groups.
* Average review rating was around 3.75.
* Average purchase amount was approximately $59.76.
* Interactive slicers help filter insights by gender, subscription status, category, and shipping type.

---

## Project Learnings

Through this project, I learned how to:

* Clean and transform data using Python and Pandas
* Create new analytical columns
* Connect Python with PostgreSQL
* Load cleaned data into a PostgreSQL database
* Write SQL queries for business analysis
* Connect PostgreSQL with Power BI
* Build an interactive business dashboard
* Present insights for decision-making

---

## Repository Structure

```text
Customer-Behavior-Analytics-Dashboard/
│
├── data/
│   └── customer_shopping_behavior.csv
│
├── notebooks/
│   └── customer_shopping_behaviour.ipynb
│
├── sql/
│   └── analysis_queries.sql
│
├── dashboard/
│   └── customer_behavior_dashboard.pbix
│
├── images/
│   └── customer_behavior_dashboard.png
│
└── README.md
```

---

## How to Run This Project

1. Clone the repository

```bash
git clone https://github.com/your-username/customer-behavior-analytics-dashboard.git
```

2. Open the Jupyter notebook

```text
notebooks/customer_shopping_behaviour.ipynb
```

3. Run the Python data cleaning steps

4. Create PostgreSQL database

```sql
CREATE DATABASE customer_behaviour;
```

5. Load cleaned data into PostgreSQL using Python

6. Open Power BI file

```text
dashboard/customer_behavior_dashboard.pbix
```

7. Refresh the PostgreSQL connection in Power BI

---

## Future Improvements

* Add customer segmentation analysis
* Add seasonal sales analysis
* Add payment method performance dashboard
* Add product-level profitability analysis
* Build predictive model for customer purchase behavior
* Publish dashboard to Power BI Service

---

## Author

Vamshi Krishna

Master’s Student in Information Technology
Specialization: AI and Data Analytics

---

## Project Status

Completed basic version of the customer behavior dashboard using Python, PostgreSQL, SQL, and Power BI.
