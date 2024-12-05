This case is sourced from: [Link](https://8weeksqlchallenge.com/case-study-1/).

***

# 🍣 Case Study #1: Danny's Diner 
<img src="https://user-images.githubusercontent.com/81607668/127727503-9d9e7a25-93cb-4f95-8bd0-20b87cb4b459.png" alt="Image" width="450" height="480">

***

## Summary
Danny wants to analyze customer data to understand their spending habits and favorite menu items to improve personalization and loyalty programs.

***

## Data recieved: 

**Table 1: Sales**  
<img src=https://github.com/user-attachments/assets/7b2365d7-cea1-4e47-929c-ca17f156d867 alt="Image" width="350" height="550">


**Table 2: Menu**  
![image](https://github.com/user-attachments/assets/a6f68029-fafc-49a8-ad64-38cf80013c0a)


**Table 3: Members**  
![image](https://github.com/user-attachments/assets/148223f7-1882-4871-8edc-d465283c3ff2)

***

## Entity Relationship Diagram

![image](https://user-images.githubusercontent.com/81607668/127271130-dca9aedd-4ca9-4ed8-b6ec-1e1920dca4a8.png)

## Question and Solution
**1. What is the total amount each customer spent at the restaurant?**

````
SELECT 
  customer_id, 
  SUM(price) AS total_spent
FROM dannys_diner.sales
JOIN dannys_diner.menu USING (product_id)
GROUP BY customer_id
ORDER BY customer_id;
````

#### Steps:
- Use **JOIN** to merge `dannys_diner.sales` and `dannys_diner.menu` tables as `sales.customer_id` and `menu.price` are from both tables.
- Use **SUM** to calculate the total sales contributed by each customer.
- Group the aggregated results by `sales.customer_id`. 

#### Answer:
| customer_id | total_sales |
| ----------- | ----------- |
| A           | 76          |
| B           | 74          |
| C           | 36          |

- Customer A spent $76.
- Customer B spent $74.
- Customer C spent $36.

***
