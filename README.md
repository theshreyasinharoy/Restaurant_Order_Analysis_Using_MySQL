# Restaurant_Order_Analysis_Using_MySQL
The project analyzes order data to identify the most and least popular menu items and types of cuisine in a restaurant.

## Objective 1
### Explore the items table
The first objective is to better understand the items table by finding the number of rows in the table, the least and most expensive items, and the item prices within each category.

### Task 

#### 1. View the menu_items table and write a query to find the number of items on the menu

#### Solution:-
```
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


     

