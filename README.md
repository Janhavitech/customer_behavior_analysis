# Customer Shopping Behavior Analysis

An end-to-end data analytics project analyzing 3,900 customer shopping transactions to uncover behavior patterns and support data-driven marketing and retention decisions — from data cleaning (Python) → business analysis (SQL) → interactive dashboard (Power BI).

## 📌 Business Problem

A retail company wants to understand shifting customer purchasing patterns across demographics, product categories, and sales channels — and which factors (discounts, reviews, seasons, payment preferences) drive purchase decisions and repeat business.

**Business question:** How can the company leverage consumer shopping data to identify trends, improve customer engagement, and optimize marketing and product strategies?

## 🧰 Tech Stack

- **Python** (Pandas) — data cleaning & feature engineering
- **MySQL**  — data warehousing & business-question queries
- **Power BI** — interactive dashboard
- **Jupyter Notebook**

## 📂 Repository Structure

```
Customer-Shopping-Behavior-Analysis/
│
├── Customer_Shopping_Behavior_Analysis.ipynb   # Python: data cleaning & feature engineering
├── customer_behavior.sql                       # SQL: 10 business-question queries
├── Customer_Behavior_Dashboard.pbix            # Power BI dashboard
├── customer_shopping_behavior.csv              # Raw dataset (3,900 rows x 18 columns)
├── Customer_Shopping_Behavior_Report.pdf       # Full project report
├── Business_Problem_Document.pdf               # Original business problem statement
├── requirements.txt
├── .env.example
├── .gitignore
└── README.md
```

## 📊 Dataset

3,900 customer transaction records, 18 original attributes: demographics (age, gender, location), product details (category, item, size, color, season), transaction data (purchase amount, payment method, shipping type, discount/promo usage), and engagement signals (review rating, subscription status, previous purchases, purchase frequency).

- Average purchase amount: **$59.76**
- Average review rating: **3.75 / 5**
- Discount usage: **43%** of purchases
- Subscribed customers: **27%**

## 🧹 Step 1 — Data Cleaning & Feature Engineering (Python)

Performed in `Customer_Shopping_Behavior_Analysis.ipynb`:

1. **Initial exploration** — `head()`, `info()`, `describe()` to understand structure and types.
2. **Missing values** — 37 nulls in `review_rating`, imputed using the median rating within each product `category` (not a single global median).
3. **Column standardization** — lowercased column names, replaced spaces with underscores, renamed `purchase_amount_(usd)` → `purchase_amount`.
4. **Feature engineering**:
   - `age_group` — quartile-based segments (Young Adult, Adult, Middle-aged, Senior) via `pd.qcut`.
   - `purchase_frequency_days` — mapped text frequency labels (e.g. "Weekly") to numeric day counts.
5. **Redundant column removal** — confirmed `discount_applied` and `promo_code_used` were identical, dropped the duplicate.
6. **Load to MySQL** — cleaned data pushed into a `customer_behavior_db.customer` table via SQLAlchemy, simulating a real data warehouse for the SQL phase. Credentials are read from environment variables (see Security Note below).

## 🗄️ Step 2 — Business Analysis (SQL)

`customer_behavior.sql` answers 10 business questions against the `customer` table, including:

- Revenue split by gender
- Customers who spent above average despite using a discount
- Top 5 products by average review rating
- Standard vs. Express shipping spend comparison
- Subscriber vs. non-subscriber spend and revenue
- Customer segmentation (New / Returning / Loyal) by purchase history
- Top 3 products per category
- Revenue contribution by age group

Key findings: male customers generated **$157,890** in revenue vs. **$75,191** for female customers (though the dataset skews 68% male); subscribers spend about the same per order as non-subscribers ($59.49 vs. $59.87); and the customer base is heavily **Loyal** (3,116) with very few **New** customers (83).

## 📈 Step 3 — Dashboard (Power BI)

`Customer_Behavior_Dashboard.pbix` gives stakeholders an interactive view with:

- KPI cards: number of customers, average purchase amount, average review rating
- Donut chart: % of customers by subscription status
- Revenue & sales by category
- Revenue & sales by age group
- Slicers: subscription status, gender, category, shipping type

The dashboard's KPIs reconcile exactly with the SQL query outputs, confirming consistency between the database and BI layers.

## 💡 Key Business Recommendations

1. **Grow the subscription program** — add tangible perks (free shipping, early access) since subscribers don't yet outspend non-subscribers.
2. **Re-balance discount strategy** — tier discounts by customer segment rather than applying them broadly (~43% of orders already discounted).
3. **Prioritize high-rated, low-discount products** (Gloves, Sandals, Boots, Hats, Skirts) in marketing.
4. **Invest in win-back campaigns** for the "Returning" segment rather than broad top-of-funnel acquisition, since the "New" segment is very small.
5. **Don't over-index on shipping type or age group alone** as standalone targeting levers — combine with category and discount-sensitivity for sharper segmentation.

## ▶️ How to Run

1. Clone the repo and install dependencies:
   ```
   git clone https://github.com/<Janhavitech>/customer-behavior-analysis.git
   cd customer-behavior-analysis
   pip install -r requirements.txt
   ```

2. Set your own MySQL credentials as environment variables (copy `.env.example` → `.env` and fill in your values, or export directly):
   ```
   export DB_USERNAME="your_username"
   export DB_PASSWORD="your_password"
   export DB_HOST="localhost"
   export DB_PORT="3306"
   export DB_NAME="customer_behavior_db"
   ```

3. Run the notebook to clean the data and load it into MySQL:
   ```
   jupyter notebook Customer_Shopping_Behavior_Analysis.ipynb
   ```

4. Run the queries in `customer_behavior.sql` against your MySQL instance.

5. Open `Customer_Behavior_Dashboard.pbix` in Power BI Desktop and point it to your local database.

## 🔒 Security Note

Database credentials are loaded from environment variables and are never hardcoded in the notebook. Make sure any real `.env` file you create is excluded via `.gitignore` and never committed.

## 👩‍💻 Author

Janhavi | MSc Graduate
