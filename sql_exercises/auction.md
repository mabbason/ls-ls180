# This set of exercises will focus on an auction. Create a new database called auction. In this database there will be three tables, bidders, items, and bids.

terminal: createdb auction
psql cnsl: 
  CREATE DATABASE auction;
  \c auction;

CREATE TABLE bidders (
  id serial PRIMARY KEY,
  name text NOT NULL
);

CREATE TABLE items (
  id serial PRIMARY KEY,
  name text NOT NULL,
  initial_price numeric(6,2) NOT NULL,
  sales_price numeric(6,2)
);

CREATE TABLE bids (
  id serial PRIMARY KEY,
  bidder_id int NOT NULL REFERENCES bidders(id) ON DELETE CASCADE,
  item_id int NOT NULL REFERENCES items(id) ON DELETE CASCADE,
  amount numeric(6,2) NOT NULL
);

CREATE INDEX ON bids (bidder_id, item_id);

# Write a SQL query that shows all items that have had bids put on them. Use the logical operator IN for this exercise, as well as a subquery.

SELECT name FROM items WHERE id IN (SELECT DISTINCT item_id FROM bids);

# Write a SQL query that shows all items that have not had bids put on them. Use the logical operator NOT IN for this exercise, as well as a subquery.

SELECT name FROM items WHERE id NOT IN (SELECT DISTINCT item_id FROM bids);

# Write a SELECT query that returns a list of names of everyone who has bid in the auction. While it is possible (and perhaps easier) to do this with a JOIN clause, we're going to do things differently: use a subquery with the EXISTS clause instead. Here is the expected output:

SELECT name FROM bidders WHERE EXISTS (SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);
or with a JOIN

SELECT DISTINCT bidders.name FROM
bidders INNER JOIN bids ON bidders.id = bids.bidder_id;

# For this exercise, we'll make a slight departure from how we've been using subqueries. We have so far used subqueries to filter our results using a WHERE clause. In this exercise, we will build that filtering into the table that we will query. Write an SQL query that finds the largest number of bids from an individual bidder.

# For this exercise, you must use a subquery to generate a result table (or virtual table), and then query that table for the largest number of bids.

SELECT max(subTable.count) FROM (SELECT count(bids.bidder_id) FROM bids GROUP BY bidder_id)AS subTable;

# For this exercise, use a scalar subquery to determine the number of bids on each item. The entire query should return a table that has the name of each item along with the number of bids on an item.

SELECT name, (SELECT count(item_id) FROM bids WHERE items.id = item_id) AS bid_count
FROM items;

Using a left outer join...

SELECT items.name, count(bids.item_id) FROM
items LEFT OUTER JOIN bids ON items.id = bids.item_id
GROUP BY items.name
ORDER BY count DESC;

# We want to check that a given item is in our database. There is one problem though: we have all of the data for the item, but we don't know the id number. Write an SQL query that will display the id for the item that matches all of the data that we know, but does not use the AND keyword. Here is the data we know:

'Painting', 100.00, 250.00

Response:
SELECT id FROM items WHERE ROW('Painting', 100.00, 250.00) = ROW(name, initial_price, sales_price);

# First use just EXPLAIN, then include the ANALYZE option as well. For your answer, list any SQL statements you used, along with the output you get back, and your thoughts on what is happening in both cases.

auction=# EXPLAIN SELECT name FROM bidders WHERE EXISTS
(SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);
                            QUERY PLAN                            
------------------------------------------------------------------
 Hash Semi Join  (cost=1.58..27.91 rows=26 width=32)
   Hash Cond: (bidders.id = bids.bidder_id)
   ->  Seq Scan on bidders  (cost=0.00..22.70 rows=1270 width=36)
   ->  Hash  (cost=1.26..1.26 rows=26 width=4)
         ->  Seq Scan on bids  (cost=0.00..1.26 rows=26 width=4)
(5 rows)


auction=# EXPLAIN ANALYZE SELECT name FROM bidders WHERE EXISTS
(SELECT 1 FROM bids WHERE bids.bidder_id = bidders.id);
QUERY PLAN                                                 
------------------------------------------------------------------------------------------------------------
 Hash Semi Join  (cost=1.58..27.91 rows=26 width=32) (actual time=0.023..0.026 rows=6 loops=1)
   Hash Cond: (bidders.id = bids.bidder_id)
   ->  Seq Scan on bidders  (cost=0.00..22.70 rows=1270 width=36) (actual time=0.007..0.008 rows=7 loops=1)
   ->  Hash  (cost=1.26..1.26 rows=26 width=4) (actual time=0.010..0.011 rows=26 loops=1)
         Buckets: 1024  Batches: 1  Memory Usage: 9kB
         ->  Seq Scan on bids  (cost=0.00..1.26 rows=26 width=4) (actual time=0.004..0.006 rows=26 loops=1)
 Planning Time: 0.119 ms
 Execution Time: 0.041 ms
(8 rows)

It looks like there are several main operations that postgres is taking to execute this SELECT.
  - Hash Semi Join, which is the overall main operation.
    - But in order to do that, it has to find where (bidders.id = bids.bidder_id) the Hash Cond.
      - Which it does by the Seq Scan on bidders and then the...
      - Hash

So each further nest on the outline, represents an action that needs to be completed in order for postgres to accomplish the major action that it is branched out from. It makes me think of a recursive function, where JS works all the way down until some condiitonal is triggered and then works all the way back up the stack. 

# Run EXPLAIN ANALYZE on the two statements. Compare the planning time, execution time, and the total time required to run these two statements. Also compare the total "costs". Which statement is more efficient and why?

```
SELECT MAX(bid_counts.count) FROM
  (SELECT COUNT(bidder_id) FROM bids GROUP BY bidder_id) AS bid_counts;
```
QUERY PLAN                                                 
-------------
 Aggregate  (cost=1.97..1.98 rows=1 width=8) (actual time=0.039..0.040 rows=1 loops=1)
   ->  HashAggregate  (cost=1.39..1.65 rows=26 width=12) (actual time=0.036..0.037 rows=6 loops=1)
         Group Key: bids.bidder_id
         ->  Seq Scan on bids  (cost=0.00..1.26 rows=26 width=4) (actual time=0.016..0.018 rows=26 loops=1)
 Planning Time: 0.066 ms
 Execution Time: 0.066 ms
(6 rows)

```
SELECT COUNT(bidder_id) AS max_bid FROM bids
  GROUP BY bidder_id
  ORDER BY max_bid DESC
  LIMIT 1;
```
QUERY PLAN                                                    
-------------
 Limit  (cost=1.78..1.78 rows=1 width=12) (actual time=0.025..0.025 rows=1 loops=1)
   ->  Sort  (cost=1.78..1.84 rows=26 width=12) (actual time=0.024..0.025 rows=1 loops=1)
         Sort Key: (count(bidder_id)) DESC
         Sort Method: top-N heapsort  Memory: 25kB
         ->  HashAggregate  (cost=1.39..1.65 rows=26 width=12) (actual time=0.019..0.020 rows=6 loops=1)
               Group Key: bidder_id
               ->  Seq Scan on bids  (cost=0.00..1.26 rows=26 width=4) (actual time=0.008..0.009 rows=26 loops=1)
 Planning Time: 0.074 ms
 Execution Time: 0.048 ms
(9 rows)

There must be something about the way my version of Postgres is handling things on my system that differs from the curriculum. But overall I see the point between comparing the way that a specific query is constructed.