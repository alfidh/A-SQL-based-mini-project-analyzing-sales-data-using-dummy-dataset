# A-SQL-based-mini-project-analyzing-sales-data-using-dummy-dataset
Designed and analyzed a dummy sales dataset to calculate revenue, profit, margin per product, and top customers, showcasing skills in SQL, data aggregation, and business insight generation.

## Description
This mini project demonstrates the ability to design, manage, and analyze sales data using SQL. It uses a dummy dataset representing products, customers, and sales transactions. Analyses focus on generating actionable business insights, including revenue, profit, margin per product, and top customers.

## Dataset / Tables
1. **Products**  
   - `product_id` (Primary Key)  
   - `product_name`  
   - `category`  
   - `product_cost`  
   - `price`   

2. **Customers**  
   - `customer_id` (Primary Key)  
   - `city`  
   - `customer_name`  

3. **Sales**  
   - `sales_id` (Primary Key)  
   - `sale_date`  
   - `product_id` (Foreign Key → Products)  
   - `customer_id` (Foreign Key → Customers)  
   - `quantity`
  
## Key Analyses / Queries
### 1. Total Quantity & Revenue by Product
```sql
SELECT p.product_name, SUM(s.quantity) AS total_quantity, SUM(p.price * s.quantity) AS total_revenue
FROM sales s
JOIN products p ON p.product_id = s.product_id
GROUP BY p.product_name
ORDER BY total_quantity DESC, total_revenue DESC;

Output:
| product_name        | total_quantity | total_revenue |
| ------------------- | -------------- | ------------- |
| Internet Package B  | 21             | 12180000.000  |
| Internet Package A  | 17             | 8500000.000   |
| Cloud Storage Pro   | 14             | 4200000.000   |
| Cloud Storage Basic | 14             | 3500000.000   |

---
```
### 2. Profit Margin by Product
```sql
SELECT p.product_name, 
       SUM((p.price - p.product_cost) * s.quantity) AS total_profit, 
       ROUND(AVG((p.price - p.product_cost) / p.price * 100), 2) AS avg_margin_percent
FROM sales s
JOIN products p ON p.product_id = s.product_id
GROUP BY p.product_name
ORDER BY total_profit DESC, avg_margin_percent DESC;


Output:
| product_name        | total_profit | avg_margin_percent |
| ------------------- | ------------ | ------------------ |
| Internet Package B  | 6930000.000  | 56.90              |
| Internet Package A  | 5100000.000  | 60.00              |
| Cloud Storage Basic | 2100000.000  | 60.00              |
| Cloud Storage Pro   | 1400000.000  | 33.33              |

---
````
### 3. Top 5 Customer by Profit
```sql
SELECT c.customer_name, SUM((p.price - p.product_cost) * s.quantity) AS total_profit
FROM sales s
JOIN customers c ON c.customer_id = s.customer_id
JOIN products p ON p.product_id = s.product_id
GROUP BY c.customer_name
ORDER BY total_profit DESC
LIMIT 5;

Output: 
| customer_name | total_profit |
| ------------- | ------------ |
| PT Maju Jaya  | 4320000.000  |
| CV Makmur     | 4160000.000  |
| CV Sinar Jaya | 3300000.000  |
| PT Sejahtera  | 2550000.000  |
| PT Abadi      | 1200000.000  |
---
```
### 4. Customer with >1 Order
```sql
SELECT c.customer_name, COUNT(s.sales_id) AS num_order
FROM sales s
JOIN customers c ON c.customer_id = s.customer_id
GROUP BY c.customer_name
HAVING COUNT(s.sales_id) > 1
ORDER BY num_order DESC;

Output:
| customer_name | num_order |
| ------------- | --------- |
| PT Maju Jaya  | 2         |
| PT Sejahtera  | 2         |
| CV Makmur     | 2         |
| PT Abadi      | 2         |
| CV Sinar Jaya | 2         |

---
```
### 5. Number of Product Order by Customer
```sql
SELECT c.customer_name, p.product_name, SUM(s.quantity) AS total_quantity
FROM sales s
JOIN customers c ON c.customer_id = s.customer_id
JOIN products p ON p.product_id = s.product_id
GROUP BY c.customer_name, p.product_name
ORDER BY c.customer_name, total_quantity DESC;

Output:

| customer_name | product_name        | total_quantity |
| ------------- | ------------------- | -------------- |
| CV Makmur     | Internet Package B  | 12             |
| CV Makmur     | Cloud Storage Pro   | 2              |
| CV Sinar Jaya | Cloud Storage Basic | 8              |
| CV Sinar Jaya | Internet Package A  | 7              |
| PT Abadi      | Cloud Storage Pro   | 12             |
| PT Maju Jaya  | Internet Package A  | 10             |
| PT Maju Jaya  | Internet Package B  | 4              |
| PT Sejahtera  | Cloud Storage Basic | 6              |
| PT Sejahtera  | Internet Package B  | 5              |

---
