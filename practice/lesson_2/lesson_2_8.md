# Prob 1
Create a new database called residents using the PostgreSQL command line tools.

CREATE DATABASE residents;

# Prob 2
Load this file into the database created in #1.

psql_course=# \i ~/ls-ls180/practice/residents_with_data.sql

# Prob 3
Write a SQL query to list the ten states with the most rows in the people table in descending order.
SELECT state, count(id) AS count FROM people GROUP BY state ORDER BY count DESC LIMIT 10; 

# Prob 4
Write a SQL query that lists each domain used in an email address in the people table and how many people in the database have an email address containing that domain. Domains should be listed with the most popular first.

SELECT split_part(email, '@', 2) AS domain, count(id) AS count FROM people
GROUP BY split_part(email, '@', 2)
ORDER BY count DESC;

# Prob 5
Write a SQL statement that will delete the person with ID 3399 from the people table.

DELETE FROM people WHERE id=3399;

# Prob 6
Write a SQL statement that will delete all users that are located in the state of California (CA).
DELETE FROM people WHERE state='CA';

# Prob 7
Write a SQL statement that will update the given_name values to be all uppercase for all users with an email address that contains teleworm.us.

UPDATE people SET given_name = upper(given_name) WHERE split_part(email,'@', 2) = 'teleworm.us';

# Prob 8
Write a SQL statement that will delete all rows from the people table.

DELETE FROM people;