# Prob 1
Write a SQL statement that makes a new sequence called "counter".

CREATE SEQUENCE counter;

# Prob 2
Write a SQL statement to retrieve the next value from the sequence created in #1.

SELECT nextval('counter');

# Prob 3
Write a SQL statement that removes a sequence called "counter".

DROP SEQUENCE counter;

# Prob 4
Is it possible to create a sequence that returns only even numbers? The documentation for sequence might be useful.

Yes I believe it is, maybe something alond the lines of:

CREATE SEQUENCE even_counter INCREMENT BY 2 START WITH 2;
Welp there you go, it worked just as expected...

# Prob 5
What will the name of the sequence created by the following SQL statement be?
CREATE TABLE regions (id serial PRIMARY KEY, name text, area integer);

regions_id_seq

It follows the tablename_id_seq naming convention.

# Prob 6
Write a SQL statement to add an auto-incrementing integer primary key column to the films table.

ALTER TABLE films ADD COLUMN id serial PRIMARY KEY;

# Prob 7
What error do you receive if you attempt to update a row to have a value for id that is used by another row?

INSERT INTO films
(title, year, genre, director, duration, id)
VALUES
('unknown', 2021, 'unknown', 'unknown name', 0, 3);
ERROR:  duplicate key value violates unique constraint "films_pkey"
DETAIL:  Key (id)=(3) already exists.

# Prob 8
What error do you receive if you attempt to add another primary key column to the films table?

psql_course=# ALTER TABLE films ADD CONSTRAINT primary_key_two PRIMARY KEY (title, year);
ERROR:  multiple primary keys for table "films" are not allowed


# Prob 9
Write a SQL statement that modifies the table films to remove its primary key while preserving the id column and the values it contains.

ALTER TABLE films DROP CONSTRAINT films_pkey;





