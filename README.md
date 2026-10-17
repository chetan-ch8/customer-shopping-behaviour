# 🛍️ Customer Shopping Behavior Analysis

An end-to-end data analytics project that explores customer shopping behavior across **3,900 transactions**. The workflow covers data cleaning and feature engineering in **Python (pandas)**, loading into **MySQL**, business analysis with **SQL**, and an interactive **Power BI** dashboard.



# 📌 Table of Contents

- [Project Overview]
- [Tech Stack]
- [Repository Structure]
- [Dataset]
- [Workflow]
- [Business Questions Answered]
- [Power BI Dashboard]
- [Getting Started]
- [Author]



# 📖 Project Overview

Understanding who buys what, how much they spend, and what drives repeat purchases is central to retail strategy. This project analyzes a customer shopping dataset to answer questions about:

- Revenue contribution by gender and age group
- Product performance, ratings, and discount dependency
- The impact of subscriptions and shipping type on spending
- Customer segmentation by purchase history (New, Returning, Loyal)

The goal is to turn raw transactional data into insights a business can act on.



# 🧰 Tech Stack

| Stage | Tools |
|---|---|
| Data cleaning & feature engineering | Python, pandas, Jupyter Notebook |
| Database | MySQL, SQLAlchemy, PyMySQL, python-dotenv |
| Analysis | SQL (aggregations, subqueries, CTEs, window functions) |
| Visualization | Power BI |



# 📁 Repository Structure

```
csb/
├── csb_analysis.ipynb                 # Data cleaning, feature engineering, MySQL load
├── cb_sql_queries.sql                 # 10 business questions answered in SQL
├── customer behaviour dashboard.pbix  # Interactive Power BI dashboard
├── .gitignore                         # Ignores .venv, .env, and the SQL export
└── README.md
```



# 📊 Dataset

- **Rows:** 3,900 transactions
- **Columns:** 18 in the raw data
- **Granularity:** one row per customer purchase

| Column | Description |
|---|---|
| Customer ID | Unique customer identifier |
| Age, Gender | Customer demographics |
| Item Purchased, Category | Product bought and its category (25 items across 4 categories) |
| Purchase Amount (USD) | Transaction value |
| Location, Size, Color, Season | Purchase and product attributes |
| Review Rating | Customer rating of the product |
| Subscription Status | Whether the customer is subscribed |
| Shipping Type | Shipping method chosen |
| Discount Applied, Promo Code Used | Discount and promo indicators |
| Previous Purchases | Number of prior purchases |
| Payment Method | Payment type used |
| Frequency of Purchases | How often the customer buys |



# 🔄 Workflow

# 1. Data Cleaning & Feature Engineering (`csb_analysis.ipynb`)

- **Inspected** the data with `head()`, `info()`, and `describe()`.
- **Handled missing values:** 37 missing `Review Rating` values were imputed using the **median rating of each product category**.
- **Standardized column names** to lowercase snake_case (e.g. `Purchase Amount (USD)` → `purchase_amount`).
- **Created `age_group`:** customers split into four quartile-based groups (Young Adult, Adult, Middle-aged, Senior) with `pd.qcut`.
- **Created `purchase_frequency_days`:** converted text frequencies (Weekly, Monthly, Quarterly, etc.) into a numeric number of days.
- **Removed redundancy:** `promo_code_used` was identical to `discount_applied` in every row, so it was dropped.

# 2. Loading into MySQL

The cleaned DataFrame is written to a MySQL table named `customer` using SQLAlchemy and PyMySQL. Database credentials are read from a `.env` file, which is git-ignored so nothing sensitive is committed.

# 3. SQL Analysis (`cb_sql_queries.sql`)

Ten business questions answered with SQL, using `GROUP BY`, subqueries, `CASE` expressions, CTEs, and window functions such as `ROW_NUMBER() OVER (PARTITION BY ...)`.

# 4. Dashboard (`customer behaviour dashboard.pbix`)

An interactive Power BI report built on the cleaned data for exploring the findings visually.



# ❓ Business Questions Answered

| # | Question |
|---|---|
| 1 | What is the total revenue from male vs. female customers? |
| 2 | Which customers used a discount but still spent more than the average purchase amount? |
| 3 | Which 5 products have the highest average review rating? |
| 4 | How do average purchase amounts compare between Standard and Express shipping? |
| 5 | Do subscribed customers spend more than non-subscribers? |
| 6 | Which 5 products have the highest percentage of discounted purchases? |
| 7 | How many customers fall into the New, Returning, and Loyal segments? |
| 8 | What are the top 3 most purchased products within each category? |
| 9 | Are repeat buyers (more than 5 previous purchases) also likely to subscribe? |
| 10 | What is the revenue contribution of each age group? |

**Customer segmentation logic (Q7):**

| Segment | Previous purchases |
|---|---|
| New | 1 |
| Returning | 2–10 |
| Loyal | More than 10 |



## 📈 Power BI Dashboard

The dashboard includes:

- **KPI cards:** number of customers, average purchase amount, average review rating
- **Charts:**
  - Customers and revenue by product category, split by subscription status
  - Customers and revenue by age group, split by subscription status
  - Subscription status split (donut chart)
- **Interactive filters:** gender, category, subscription status, and shipping type



# 🚀 Getting Started

# Prerequisites

- Python 3.9+
- MySQL Server
- Power BI Desktop (to open the `.pbix` file)

# Installation

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>

# 2. Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate        # On Windows: .venv\Scripts\activate

# 3. Install dependencies
pip install pandas pymysql sqlalchemy python-dotenv jupyter
```

# Configuration

Add the dataset file `customer_shopping_behavior.csv` to the project root, then create a `.env` file with your MySQL credentials:

```env
DB_USERNAME=your_username
DB_PASSWORD=your_password
DB_HOST=localhost
DB_PORT=3306
DB_NAME=your_database
```

# Run

1. Open and run `csb_analysis.ipynb` to clean the data and create the `customer` table in MySQL.
2. Run the queries in `cb_sql_queries.sql` in MySQL Workbench or any MySQL client.
3. Open `customer behaviour dashboard.pbix` in Power BI Desktop to explore the dashboard.

