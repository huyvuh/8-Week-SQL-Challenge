This case is sourced from: [here](https://8weeksqlchallenge.com/case-study-1/).

***

# 🍣 Case Study #1: Danny's Diner 
<img src="https://user-images.githubusercontent.com/81607668/127727503-9d9e7a25-93cb-4f95-8bd0-20b87cb4b459.png" alt="Image" width="450" height="480">

***

## Summary
Danny wants to analyze customer data to understand their spending habits and favorite menu items to improve personalization and loyalty programs.

***

## We were given 

- **Sales**  
<img src=https://github.com/user-attachments/assets/7b2365d7-cea1-4e47-929c-ca17f156d867 alt="Image" width="350" height="550">


- **Menu**  
![image](https://github.com/user-attachments/assets/a6f68029-fafc-49a8-ad64-38cf80013c0a)


- **Customers**  
![image](https://github.com/user-attachments/assets/148223f7-1882-4871-8edc-d465283c3ff2)

***

## Entity Relationship Diagram

![image](https://github.com/user-attachments/assets/c6edadea-188a-495c-b44c-b5e3f1a26093)


## Question and Solution
**1️⃣. What is the total amount each customer spent at the restaurant?**

```sql
SELECT 
  customer_id, 
  SUM(price) AS total_spent
FROM dannys_diner.sales
JOIN dannys_diner.menu USING (product_id)
GROUP BY customer_id
ORDER BY customer_id;
```

#### Reason:
- **JOIN** can merge the `sales` and `menu` tables into a new table with two collumns: `customer_id` and `price`.
- **SUM** can calculate the total sales contributed by each customer.
- **GROUP** the aggregated results by `customer_id`. 

#### Result:
| customer_id | total_spent |
| ----------- | ----------- |
| A           | 76          |
| B           | 74          |
| C           | 36          |

- Customer A spent $76.
- Customer B spent $74.
- Customer C spent $36.

***

**2️⃣. How many days has each customer visited the restaurant?**

```sql
SELECT 
  customer_id, 
  COUNT(DISTINCT order_date) AS visit_count
FROM sales
GROUP BY customer_id;
```

#### Reason:
- **COUNT(DISTINCT)** is used to remove double counted

#### Result:
| customer_id | visit_count |
| ----------- | ----------- |
| A           | 4           |
| B           | 6           |
| C           | 2           |

- Customer A visited 4 times.
- Customer B visited 6 times.
- Customer C visited 2 times.

***

**3️⃣. What was the first item from the menu purchased by each customer?**

```sql
WITH first_order_sale AS 
	(
      SELECT
      	sales.customer_id, 
      	sales.order_date, 
      	menu.product_name, 
      	DENSE_RANK() OVER ( PARTITION BY  sales.customer_id  ORDER BY sales.order_date) AS rank
      	FROM dannys_diner.sales 
      	INNER JOIN dannys_diner.menu 
      		ON sales.product_id = menu.product_id
      )
      	
 	SELECT
    	first_order_sale.customer_id, 
        first_order_sale.product_name
        FROM first_order_sale
        where rank = 1
```
#### Result:
| customer_id | product_name |
| ----------- | ----------- |
| A           | curry       |
| A           | sushi       |
| B           | curry       |
| C           | ramen       |
| C           | ramen       |

