# 🍔 Swiggy Sales Analysis

> **End-to-end SQL-based data warehousing and analysis of 200k+ Swiggy order records, uncovering revenue patterns, customer behavior, restaurant performance, and regional trends across India.**


---

## ⭐ Project Overview

### 🔴 Situation
Swiggy, one of India's leading food delivery platforms, generates millions of transactional records daily spanning multiple cities, states, restaurants, and food categories. Raw transactional data in this form is difficult to query efficiently and offers limited analytical value without proper structuring.

The dataset contained **197,430 raw order records** (pre-cleaning) across dimensions including location, restaurant, dish, category, price, rating, and date — stored as a flat CSV file with no relational structure, mixed date formats, and duplicate entries.

---

### 🎯 Task
The goal was to transform this raw, unstructured dataset into a **production-grade analytical system** capable of answering key business questions across six domains:

- 📈 Revenue generation trends
- 👤 Customer spending behavior
- 🍽️ Restaurant & food category performance
- 🗺️ Regional demand patterns
- ⭐ Customer satisfaction analysis
- 📅 Time-based demand forecasting

---

### ⚙️ Action

#### 1. Database Setup & Data Loading
- Created `swiggy_db` in MySQL with proper data types and constraints
- Loaded raw CSV using `LOAD DATA LOCAL INFILE` with `UTF8MB4` character encoding
- Handled date format conversion using `STR_TO_DATE()` during ingestion

#### 2. Data Quality Assurance
- Ran a **comprehensive null matrix scan** across all 10 columns
- Detected and flagged **blank/empty string entries** in categorical fields
- Identified **27 duplicate rows** using full-row `GROUP BY` analysis
- Removed duplicates safely using `ROW_NUMBER()` window function with `AUTO_INCREMENT` ID partitioning

#### 3. Star Schema Data Warehouse Design
Restructured the flat table into a **Star Schema** with:

```
Fact Table: fact_swiggy_orders
    ├── dim_date        (date_id, full_date, year, month, quarter, week)
    ├── dim_location    (location_id, state, city, location)
    ├── dim_restaurant  (restaurant_id, restaurant_name)
    ├── dim_category    (category_id, category)
    └── dim_dish        (dish_id, dish_name)
```

- Populated all dimension tables using `SELECT DISTINCT` from staging
- Linked fact table via multi-table `INNER JOIN` surrogate key resolution
- Enforced referential integrity using `FOREIGN KEY` constraints

#### 4. KPI Computation
Computed four core business KPIs directly from the fact table:
- Total Orders, Total Revenue (INR Millions), Average Dish Price, Average Rating

#### 5. Deep-Dive Analytical Queries
Built a full library of BI queries covering:
- Monthly & day-of-week order volume trends
- Top 10 cities and states by order volume and revenue
- Restaurant rankings by orders, revenue, and rating
- Food category revenue share analysis
- Customer spending segmentation by price buckets
- Revenue vs. Rating correlation analysis
- Price vs. Satisfaction relationship

---

### 📊 Result

#### Key Performance Indicators

| Metric | Value |
|--------|-------|
| ✅ Total Orders | 197,403 |
| 💰 Total Revenue | ₹53 Million |
| 🧾 Avg Dish Price | ₹268 |
| ⭐ Avg Customer Rating | 4.34 / 5 |

#### Top Findings

**🕐 Time Trends**
- Q2 was the strongest quarter with **74,155 orders**
- January, August & May led in monthly revenue
- Saturday and Friday see peak orders; Monday is the slowest day

**🏙️ Location Insights**
- **Bengaluru, Mumbai, Hyderabad, Jaipur, Lucknow** are the top cities by volume
- **Karnataka, Uttar Pradesh, Telangana** lead in revenue
- **Kerala (4.44)** ranks highest in customer satisfaction

**🍽️ Restaurant Performance**
- McDonald's leads in order volume; **KFC leads in revenue** despite fewer orders — indicating a higher average order value
- Regional restaurants (Sakana, Vijay Dairy, Radhey Lal's Parampara Sweets) outperform national chains in customer ratings

**💸 Customer Spending**
- ₹100–₹299 is the dominant spending band, accounting for **110,757 combined orders**
- Top dishes: Choco Lava Cake, Veg Fried Rice, Paneer Butter Masala

**⭐ Rating vs Revenue**
- Mid-rated dishes (4.0–4.49) generate **₹33.37M** vs. ₹6.64M for top-rated (4.8+) — proving volume matters more than perfection
- Price has minimal impact on satisfaction (ratings range only 4.33–4.39 across all price brackets)

---

## 📌 Strategic Recommendations

1. **Expand in High-Volume Cities** — Deepen partnerships in Bengaluru and top metros
2. **Target ₹100–₹299 Segment** — Bundle offers and promotions where demand is strongest
3. **Boost High-Rated Dish Visibility** — Feature underutilized top-rated dishes prominently
4. **Invest in Main Course & Burgers** — Top revenue-generating categories
5. **Replicate Kerala's Success Model** — Study and scale high-satisfaction regional strategies
6. **Support Regional Restaurants** — Feature local brands through promoted listings
