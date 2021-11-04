Practice Problems

# Import this file into a new database.

You may experience some errors while importing this database:

ERROR:  relation "public.orders" does not exist
ERROR:  relation "public.products" does not exist
ERROR:  relation "public.orders" does not exist
ERROR:  relation "public.products" does not exist
ERROR:  relation "public.orders" does not exist
ERROR:  sequence "products_id_seq" does not exist
ERROR:  table "products" does not exist
ERROR:  sequence "orders_id_seq" does not exist
ERROR:  table "orders" does not exist
You may ignore these errors.

psql -d psql_course < orders_products1.sql

# Update the orders table so that referential integrity will be preserved for the data between orders and products.
ALTER TABLE orders ADD CONSTRAINT foreign_key_orders FOREIGN KEY (product_id) REFERENCES products(id);

# Use psql to insert the data shown in the following table into the database:

Quantity	Product
10	small bolt
25	small bolt
15	large bolt

INSERT INTO products
(name)
VALUES
('small bolt'),
('large bolt');

INSERT INTO orders
(product_id, quantity)
VALUES                          
(1, 10),
(1, 25), 
(2, 15);

# Write a SQL statement that returns a result like this:

 quantity |    name
----------+------------
       10 | small bolt
       25 | small bolt
       15 | large bolt
(3 rows)

SELECT orders.quantity, products.name FROM
orders INNER JOIN products
ON products.id=orders.product_id;

# Can you insert a row into orders without a product_id? Write a SQL statement to prove your answer.
No, because of the Foreign Key constraint. If it merely referenced the products.id column, then that would be possible. Since it was set to reference with a FK constraint, it will error out as there would be no corresponding product.id to reference.

INSERT INTO orders
(quantity)
VALUES
(13);

Welp, nvm. I was wrong. 

# Write a SQL statement that will prevent NULL values from being stored in orders.product_id. What happens if you execute that statement?

ALTER TABLE orders ALTER COLUMN product_id SET NOT NULL;
It will error, because there is already a NULL value in the table.

# Make any changes needed to avoid the error message encountered in #6.

DELETE FROM orders WHERE id=4;
ALTER TABLE orders ALTER COLUMN product_id SET NOT NULL;

# Create a new table called reviews to store the data shown below. This table should include a primary key and a reference to the products table.
Product	Review
small bolt	a little small
small bolt	very round!
large bolt	could have been smaller

CREATE TABLE reviews (
id serial PRIMARY KEY,
product_id integer REFERENCES products(id) NOT NULL,
review text NOT NULL);

# Write SQL statements to insert the data shown in the table in #8.
INSERT INTO reviews
(product_id, review)
VALUES
(1, 'a little small'),
(1, 'very round!'),
(2, 'could have been smaller');

# True or false: A foreign key constraint prevents NULL values from being stored in a column.
False. I learned this one a few questions ago, it would make sense to use NOT NULL on every FOREIGN KEY column it seems.