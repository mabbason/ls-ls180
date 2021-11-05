# For this set of exercises we'll want to create a new database and some tables to work with. The theme for the exercise is that of a workshop. Create a database to store information and tables related to this workshop.

# One table should be called devices. This table should have columns that meet the following specifications:

# Includes a primary key called id that is auto-incrementing.
# A column called name, that can contain a String. It cannot be NULL.
# A column called created_at that lists the date this device was created. It should be of type timestamp and it should also have a default value related to when a device is created.
# In the workshop, we have several devices, and each device should have many different parts. These parts are unique to each device, so one device can have various parts, but those parts won't be used in any other device. Make a table called parts that reflects the information listed above. Table parts should have the following columns that meet the following specifications:

# An id which auto-increments and acts as the primary key
# A part_number. This column should be unique and not null.
# A foreign key column called device_id. This will be used to associate various parts with a device.

psql_course=# CREATE DATABASE workshop;
CREATE DATABASE
psql_course=# \c workshop;
You are now connected to database "workshop" as user "mabba".
workshop=# CREATE TABLE devices (
workshop(# id serial PRIMARY KEY,
workshop(# name text NOT NULL,
workshop(# created_at timestamp DEFAULT now()
workshop(# );
CREATE TABLE
workshop=# CREATE TABLE parts (
workshop(# id serial PRIMARY KEY,
workshop(# part_number int UNIQUE NOT NULL,
workshop(# device_id int NOT NULL REFERENCES devices(id)
workshop(# );
CREATE TABLE

# Now that we have the infrastructure for our workshop set up, let's start adding in some data. Add in two different devices. One should be named, "Accelerometer". The other should be named, "Gyroscope".

# The first device should have 3 parts (this is grossly simplified). The second device should have 5 parts. The part numbers may be any number between 1 and 10000. There should also be 3 parts that don't belong to any device yet.

workshop=# INSERT INTO parts
(part_number, device_id)
VALUES
(113, 1),
(114, 1),
(115, 1),
(201, 2),
(202, 2),
(203, 2),
(204, 2),
(205, 2),
(301, DEFAULT),
(302, DEFAULT),
(303, DEFAULT);

# Write an SQL query to display all devices along with all the parts that make them up. We only want to display the name of the devices. Its parts should only display the part_number.

SELECT devices.name, parts.part_number FROM
devices INNER JOIN parts
ON devices.id = parts.device_id;

# We want to grab all parts that have a part_number that starts with 3. Write a SELECT query to get this information. This table may show all attributes of the parts table.

workshop=# SELECT * FROM parts WHERE part_number::text LIKE '3%';
 id | part_number | device_id 
----+-------------+-----------
 10 |         301 |          
 11 |         302 |          
 12 |         303 |          
(3 rows)

# Write an SQL query that returns a result table with the name of each device in our database together with the number of parts for that device.

workshop=# SELECT devices.name, count(parts.id) AS no_of_parts FROM
workshop-# devices LEFT OUTER JOIN parts
workshop-# ON devices.id = parts.device_id
workshop-# GROUP BY devices.name;
     name      | no_of_parts 
---------------+-------------
 Accelerometer |           3
 Gyroscope     |           5
(2 rows)

# In the previous exercise, we had to use a `GROUP BY` clause to obtain the expected output. In that exercise, we used an SQL query like:
```
SELECT devices.name AS name, COUNT(parts.device_id)
FROM devices
JOIN parts ON devices.id = parts.device_id
GROUP BY devices.name;

```

# Now, we want to work with the same query, but we want to guarantee that the devices and the count of their parts are listed in descending alphabetical order. Alter the SQL query above so that we get a result table that looks like the following.
```
name          | count
--------------+-------
Gyroscope     |     5
Accelerometer |     3
(2 rows)
```
Response:

workshop=# SELECT devices.name, count(parts.id) AS no_of_parts FROM
devices LEFT OUTER JOIN parts
ON devices.id = parts.device_id
GROUP BY devices.name
ORDER BY devices.name DESC;
     name      | no_of_parts 
---------------+-------------
 Gyroscope     |           5
 Accelerometer |           3
(2 rows)

# Write two SQL queries:

# One that generates a listing of parts that currently belong to a device.
# One that generates a listing of parts that don't belong to a device.
# Do not include the id column in your queries.

workshop=# SELECT part_number, device_id FROM parts WHERE device_id IS NOT NULL;
 part_number | device_id 
-------------+-----------
         113 |         1
         114 |         1
         115 |         1
         201 |         2
         202 |         2
         203 |         2
         204 |         2
         205 |         2
(8 rows)


workshop=# SELECT part_number, device_id FROM parts WHERE device_id IS NULL;
 part_number | device_id 
-------------+-----------
         301 |          
         302 |          
         303 |          
(3 rows)


# Assuming nothing about the existing order of the records in the database, write an SQL statement that will return the name of the oldest device from our devices table.

SELECT name FROM devices ORDER BY created_at LIMIT 1;


