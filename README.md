# Restaurant_Order_Analysis_Using_MySQL
The project analyzes order data to identify the most and least popular menu items and types of cuisine in a restaurant.

## Objective 1
### Explore the items table
The first objective is to better understand the items table by finding the number of rows in the table, the least and most expensive items, and the item prices within each category.

### Task 

#### 1. View the menu_items table and write a query to find the number of items on the menu

#### Solution:-
```
           SELECT * FROM menu_items;

           SELECT 
                 COUNT(DISTINCT menu_item_id) AS Item_count
           FROM 
                 menu_items;
```
#### 2. What are the least and most expensive items on the menu?

#### Solution:-
```
    SELECT
          MAX(CASE WHEN price = (SELECT MAX(price) FROM menu_items) THEN item_name END) AS most_expensive,
          MIN(CASE WHEN price = (SELECT MIN(price) FROM menu_items) THEN item_name END) AS least_expensive
    FROM
          menu_items;
```
#### 3. How many Italian dishes are on the menu? What are the least and most expensive Italian dishes on the menu?

#### Solution:-
```
    SELECT
          COUNT(*) AS No_of_Itaian
    FROM
          menu_items
    WHERE
          category LIKE "Italian";
```
```
    SELECT
           MAX(CASE WHEN price = (SELECT MAX(price) FROM menu_items WHERE category LIKE 'Italian') THEN item_name END) AS most_expensive,
           MIN(CASE WHEN price = (SELECT MIN(price) FROM menu_items WHERE category LIKE 'Italian') THEN item_name END) AS least_expensive
    FROM
           menu_items;
```
#### 4. How many dishes are in each category? What is the average dish price within each category?

#### Solution:-
```
SELECT
          category,
          COUNT(item_name) AS no_of_dishes,
          ROUND(AVG(price),2) AS avg_dish_price
FROM
          menu_items
GROUP BY
          category;
```
---

## Objective 2
### Explore the orders table
Your second objective is to better understand the orders table by finding the date range, the number of items within each order, and the orders with the highest number of items.

### Task 

#### 1. View the order_details table. What is the date range of the table?

#### Solution:-
```
SELECT * FROM order_details;

SELECT
       MIN(order_date),
       MAX(order_date)
FROM
       order_details;
```

#### 2. How many orders were made within this date range? How many items were ordered within this date range?

#### Solution:-
```
SELECT
       COUNT(DISTINCT order_id) AS order_count,
       COUNT(DISTINCT item_id) AS item_count
FROM
       order_details;
```
#### 3. Which orders had the most number of items?

#### Solution:-
```
     SELECT
               order_id
     FROM
               order_details
     GROUP BY
               order_id
     HAVING  
               COUNT(item_id) = 
               (SELECT 
                       MAX(item_count)
                FROM (
                        SELECT 
                                   COUNT(item_id) AS item_count
                        FROM
                                   order_details
                         GROUP BY
                                   order_id
                     ) AS counting
                );
```

#### 4.How many orders had more than 12 items?

#### Solution:-
```
    SELECT
             COUNT(order_id) AS orders_with_more_than_12_items
    FROM (
             SELECT 
                       order_id, 
                       COUNT(item_id) AS item_count
             FROM 
                       order_details
             GROUP BY 
                       order_id
             HAVING 
                       item_count > 12
         ) AS subquery;
```
---

## Objective 3
### Analyze customer behavior
Your final objective is to combine the items and orders tables, find the least and most ordered categories, and dive into the details of the highest spend orders.

### Task

#### 1. Combine the menu_items and order_details tables into a single table

#### Solution:-
```
SELECT 
       *
FROM
       order_details JOIN menu_items
ON
       order_details.item_id=menu_items.menu_item_id;
```
#### 2. What were the least and most ordered items? What categories were they in?

#### Solution:-
```
WITH temp AS (
SELECT 
          category,
          item_name,
          COUNT(order_id) AS order_count,
          RANK() OVER (ORDER BY COUNT(order_id) DESC) AS max_to_min
FROM
          order_details JOIN menu_items
ON
          order_details.item_id=menu_items.menu_item_id
GROUP BY
          category,
          item_name
)
SELECT
        category,
		item_name AS most_ordered_item
FROM
		temp
WHERE 
        max_to_min=1;
```
```
WITH temp AS (
SELECT 
          category,
          item_name,
          COUNT(order_id) AS order_count,
          RANK() OVER (ORDER BY COUNT(order_id) ASC) AS min_to_max
FROM
          order_details JOIN menu_items
ON
          order_details.item_id=menu_items.menu_item_id
GROUP BY
          category,
          item_name
)
SELECT
        category,
		item_name AS least_ordered_item
FROM
		temp
WHERE 
        min_to_max=1;
```
#### 3. What were the top 5 orders that spent the most money?

#### Solution:-
```
SELECT
          order_id,
          SUM(price) AS Total_price
FROM
          order_details JOIN menu_items 
ON 
          order_details.item_id = menu_items.menu_item_id
GROUP BY
          order_id
ORDER BY 
          Total_price DESC
LIMIT
          5;
```
#### 4. View the details of the highest spend order. Which specific items were purchased?

#### Solution:-
```
```



     

