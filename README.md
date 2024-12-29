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
```

#### 4.How many orders had more than 12 items?

#### Solution:-
```
```



     

