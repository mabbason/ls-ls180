# Prob 1
What is the result of using an operator on a NULL value?

It results in an error because NULL isn't any type of value, so the computer doesn't really know how to handle it with an operator.
(Wrong. I was right about what NULL is but not about what it would result in)

# Prob 2
Set the default value of column department to "unassigned". Then set the value of the department column to "unassigned" for any rows where it has a NULL value. Finally, add a NOT NULL constraint to the department column.

ALTER TABLE employees ALTER COLUMN department SET DEFAULT 'unassigned';
UPDATE employees SET department = 'unassigned' WHERE department IS NULL;
ALTER TABLE employees ALTER COLUMN department SET NOT NULL;

# Prob 3
Write the SQL statement to create a table called temperatures to hold the following data:
    date    | low | high
------------+-----+------
 2016-03-01 | 34  | 43
 2016-03-02 | 32  | 44
 2016-03-03 | 31  | 47
 2016-03-04 | 33  | 42
 2016-03-05 | 39  | 46
 2016-03-06 | 32  | 43
 2016-03-07 | 29  | 32
 2016-03-08 | 23  | 31
 2016-03-09 | 17  | 28
 Keep in mind that all rows in the table should always contain all three values.

CREATE TABLE temperatures (
date date NOT NULL,
low integer NOT NULL,
high integer NOT NULL
);

# Prob 4
Write the SQL statements needed to insert the data shown in #3 into the temperatures table.

# Prob 5
Write a SQL statement to determine the average (mean) temperature -- divide the sum of the high and low temperatures by two) for each day from March 2, 2016 through March 8, 2016. Make sure to round each average value to one decimal place.

SELECT round((high + low) / 2.0, 1) AS avg_temp FROM temperatures WHERE date BETWEEN
'2016-03-02' AND '2016-03-08';

# Prob 6
Write a SQL statement to add a new column, rainfall, to the temperatures table. It should store millimeters of rain as integers and have a default value of 0.

ADD COLUMN rainfall integer DEFAULT 0 NOT NULL;

# Prob 7
Each day, it rains one millimeter for every degree the average temperature goes above 35. Write a SQL statement to update the data in the table temperatures to reflect this.



# Prob 8
A decision has been made to store rainfall data in inches. Write the SQL statements required to modify the rainfall column to reflect these new requirements.

ALTER TABLE temperatures ALTER COLUMN rainfall TYPE decimal(4,3);
UPDATE temperatures SET rainfall = rainfall * 0.0393701;

# Prob 9
Write a SQL statement that renames the temperatures table to weather.

ALTER TABLE temperatures RENAME TO weather;
# Prob 10
What psql meta command shows the structure of a table? Use it to describe the structure of weather.

\d weather;

# Prob 11
What PostgreSQL program can be used to create a SQL file that contains the SQL commands needed to recreate the current structure and data of the weather table?

I didn't know this one... but that seems like it could be a pretty useful command.