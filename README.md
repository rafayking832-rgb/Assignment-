*1.1 Calculated column:*

SELECT 
    product_name, 
    list_price, 
    list_price / 1000.0 AS price_in_thousands
FROM production.products;

*1.2 Distinct states:*

SELECT DISTINCT state 
FROM sales.customers;

*1.3 5 cheapest products:*

SELECT TOP 5 product_name, list_price
FROM production.products
ORDER BY list_price ASC;

*1.4 BETWEEN + IN together:*
SELECT product_name, list_price, category_id
FROM production.products
WHERE list_price BETWEEN 200 AND 600
AND category_id IN (1, 6);

*1.5 Bug Identification and Fix:*

*Bug:* `AND` has higher priority than `OR`. So your query is read as:
`category_id = 1 OR (category_id = 6 AND list_price < 1000)`
It will return ALL bikes of category 1 even if price is $5000.

*Correct version:*

SELECT product_name, category_id, list_price
FROM production.products
WHERE (category_id = 1 OR category_id = 6)
AND list_price < 1000;

-- Better way with IN:
SELECT product_name, category_id, list_price
FROM production.products
WHERE category_id IN (1, 6)
AND list_price < 1000;

*1.6 LIKE patterns:*

SELECT *
FROM sales.customers
WHERE email LIKE '%.com'
AND last_name LIKE 'M%';

*1.7 OFFSET FETCH (11th to 20th most expensive):*

SELECT product_name, list_price
FROM production.products
ORDER BY list_price DESC
OFFSET 10 ROWS
FETCH NEXT 10 ROWS ONLY;

*1.8 Think About It:*

*Why `WHERE discount <> NULL` returns 0 rows:*
In SQL there is Three-Valued Logic: `TRUE, FALSE, UNKNOWN`.
Any comparison with `NULL` using `=, <>, <, >` does NOT return TRUE/FALSE, it returns `UNKNOWN`. And `WHERE` only keeps rows where result is `TRUE`.

So `discount <> NULL` = `UNKNOWN` for every row, even if discount is 10.

*Correct version:*
-- Find order items WITH a discount defined
SELECT * FROM sales.order_items
WHERE discount IS NOT NULL;

-- Find order items WITHOUT discount
SELECT * FROM sales.order_items
WHERE discount IS NULL;
Want me to make these into one file you can run on mobile?
