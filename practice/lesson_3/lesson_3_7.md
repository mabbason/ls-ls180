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


# Write a SQL statement that returns a result like this:

 quantity |    name
----------+------------
       10 | small bolt
       25 | small bolt
       15 | large bolt
(3 rows)


# Can you insert a row into orders without a product_id? Write a SQL statement to prove your answer.

# Write a SQL statement that will prevent NULL values from being stored in orders.product_id. What happens if you execute that statement?


# Make any changes needed to avoid the error message encountered in #6.

# Create a new table called reviews to store the data shown below. This table should include a primary key and a reference to the products table.
Product	Review
small bolt	a little small
small bolt	very round!
large bolt	could have been smaller


# Write SQL statements to insert the data shown in the table in #8.

# True or false: A foreign key constraint prevents NULL values from being stored in a column.