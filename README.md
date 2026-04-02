
# 🛒 E-commerce Analytics - Olist Store

## 📊 Project Overview

Comprehensive data analysis project examining 100K+ orders from **Olist**, a Brazilian e-commerce marketplace. This project demonstrates end-to-end analytics capabilities using SQL, Power BI, Excel, and Tableau to derive actionable business insights.

## 🎯 Key Insights

- **Total Orders**: 99,441 orders analyzed
- **Total Revenue**: ₹16.01M (converted from BRL)
- **Total Customers**: 96.5K unique customers
- **Average Order Value**: ₹163.32
- **Delivery Performance**: 11.31 average delivery days for pet shop category
- **Payment Preference**: Credit card dominates with 76,795 transactions (73.92%)

## 🛠️ Technologies Used

- **Database**: MySQL 8.0
- **Visualization**: 
  - Microsoft Power BI
  - Microsoft Excel (Power Query, Pivot Tables)
  - Tableau Public
- **Query Language**: SQL (Advanced queries with JOINs, CTEs, Window Functions)

## 📈 Dashboard Features

### Power BI Dashboard
- Sales performance metrics (Total Price, Total Payment, Orders)
- Weekday vs Weekend analysis (77% weekday, 23% weekend)
- Payment type distribution
- Shipping days correlation with review scores
- Geographic sales analysis (São Paulo city analysis)

### Excel Dashboard
- Interactive slicers (Year, Quarter, Payment Type, Weekday/Weekend)
- KPI cards (Total Sales, Orders, Profit)
- Pet shop category delivery analysis
- State-wise customer distribution
- Review score impact on shipping days

### Tableau Dashboard
- Interactive performance cockpit
- Credit card review analysis
- Customer segmentation by state
- Multi-dimensional filtering

## 💾 Database Schema

The project uses the Olist e-commerce dataset with the following tables:
- `olist_orders` - Order information
- `olist_order_payments` - Payment details
- `olist_order_items` - Product items per order
- `olist_products` - Product catalog
- `olist_customers` - Customer information
- `olist_order_reviews` - Customer reviews
- `olist_sellers` - Seller information
- `olist_geolocation` - Geographic data

## 📝 Key SQL Queries

### Q1: Weekday vs Weekend Analysis
```sql
SELECT 
    CASE 
        WHEN DAYOFWEEK(order_purchase_timestamp) IN (1, 7) 
        THEN 'Weekend'
        ELSE 'Weekday'
    END AS day_type,
    COUNT(DISTINCT order_id) AS total_orders,
    ROUND(SUM(payment_value), 2) AS total_payment
FROM olist_orders o
JOIN olist_order_payments p ON o.order_id = p.order_id
GROUP BY day_type;
```

### Q2: Orders with Review Score 5 and Credit Card Payment
```sql
SELECT COUNT(DISTINCT o.order_id) AS total_orders
FROM olist_orders o
JOIN olist_order_reviews r ON o.order_id = r.order_id
JOIN olist_order_payments p ON o.order_id = p.order_id
WHERE r.review_score = 5 
AND p.payment_type = 'credit_card';
```

### Q3: Average Delivery Days for Pet Shop
```sql
SELECT 
    ROUND(AVG(DATEDIFF(order_delivered_customer_date, 
                       order_purchase_timestamp)), 2) AS avg_delivery_days
FROM olist_orders o
JOIN olist_order_items oi ON o.order_id = oi.order_id
JOIN olist_products p ON oi.product_id = p.product_id
WHERE p.product_category_name = 'pet_shop'
AND order_delivered_customer_date IS NOT NULL;
```

### Q4: Average Price and Payment for São Paulo
```sql
SELECT 
    ROUND(AVG(p.payment_value), 2) AS avg_payment
FROM olist_customers c
JOIN olist_orders o ON c.customer_id = o.customer_id
JOIN olist_order_payments p ON o.order_id = p.order_id
WHERE c.customer_city = 'sao paulo';
```

### Q5: Shipping Days vs Review Score Relationship
```sql
SELECT 
    r.review_score,
    ROUND(AVG(DATEDIFF(o.order_delivered_customer_date, 
                       o.order_purchase_timestamp)), 2) AS avg_shipping_days
FROM olist_orders o
JOIN olist_order_reviews r ON o.order_id = r.order_id
WHERE o.order_delivered_customer_date IS NOT NULL
GROUP BY r.review_score
ORDER BY r.review_score;
```

## 📸 Dashboard Preview

### Power BI Dashboard
![Dashboard Screenshot](Screenshot%202026-04-02%20104413.png)

### Tableau Dashboard
![Dashboard Screenshot](Screenshot%202026-03-31%20171450.png)

### Excel Dashboard
![Dashboard Screenshot](Screenshot%202026-04-01%20184036.png)



## 🔍 Key Findings

1. **Weekday Dominance**: 77% of orders occur on weekdays
2. **Credit Card Preference**: 74% customers prefer credit card payments
3. **Delivery Correlation**: Higher review scores correlate with faster delivery (21.25 days for score 1 vs 10.63 days for score 5)
4. **Geographic Concentration**: São Paulo leads with 15,540 orders
5. **Payment Distribution**: Credit card (₹12.54M), Boleto (₹2.87M), Voucher (₹0.38M)

## 👨‍💻 Author
**Abhishek Reddy**
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/abhishekreddy111)

---

⭐ If you found this project helpful, please give it a star!
