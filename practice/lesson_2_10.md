# Prob 1
Import this file into a PostgreSQL database.

\i ~/ls-ls180/practice/films2.sql

# Prob 2
Modify all of the columns to be NOT NULL.

ALTER TABLE films
ALTER COLUMN title SET NOT NULL,
ALTER COLUMN year SET NOT NULL,
ALTER COLUMN genre SET NOT NULL,
ALTER COLUMN director SET NOT NULL,
ALTER COLUMN duration SET NOT NULL;

# Prob 3
How does modifying a column to be NOT NULL affect how it is displayed by \d films?

It will set the value in displayed in the 'Nullable' column to 'not null' for each row.

# Prob 4
Add a constraint to the table films that ensures that all films have a unique title.

ALTER TABLE films
ADD CONSTRAINT unique_title UNIQUE (title);

# Prob 5
How is the constraint added in #4 displayed by \d films?

It is added under the 'Indexes' property for the table, UNIQUE CONSTRAINT, btree (title).

# Prob 6
Write a SQL statement to remove the constraint added in #4.

ALTER TABLE films DROP CONSTRAINT unique_title CASCADE;

# Prob 7
Add a constraint to films that requires all rows to have a value for title that is at least 1 character long.

ALTER TABLE films ADD CONSTRAINT min_title_length CHECK (length(title) >= 1);

# Prob 8
What error is shown if the constraint created in #7 is violated? Write a SQL INSERT statement that demonstrates this.

INSERT INTO films
(title, year, genre, director, duration)
VALUES
('', 2001, 'unknown', 'unknown', 0);
ERROR:  new row for relation "films" violates check constraint "min_title_length"
DETAIL:  Failing row contains (, 2001, unknown, unknown, 0).

# Prob 9
How is the constraint added in #7 displayed by \d films?

Check constraints:
    "min_title_length" CHECK (length(title::text) >= 1)

# Prob 10
Write a SQL statement to remove the constraint added in #7.

ALTER TABLE films DROP CONSTRAINT min_title_length;

# Prob 11
Add a constraint to the table films that ensures that all films have a year between 1900 and 2100.

ALTER TABLE films
ADD CONSTRAINT year_range
CHECK (year BETWEEN 1900 AND 2100);

# Prob 12
How is the constraint added in #11 displayed by \d films?

Check constraints:
    "year_range" CHECK (year >= 1900 AND year <= 2100)

# Prob 13
Add a constraint to films that requires all rows to have a value for director that is at least 3 characters long and contains at least one space character ().

ALTER TABLE films
ADD CONSTRAINT director_name
CHECK (length(director) >= 3 AND director LIKE '% %');

# Prob 14
How does the constraint created in #13 appear in \d films?

Check constraints:
    "director_name" CHECK (length(director::text) >= 3 AND director::text ~~ '% %'::text)

# Prob 15
Write an UPDATE statement that attempts to change the director for "Die Hard" to "Johnny". Show the error that occurs when this statement is executed.

UPDATE films SET director = 'Johnny' WHERE title = 'Die Hard';
ERROR:  new row for relation "films" violates check constraint "director_name"
DETAIL:  Failing row contains (Die Hard, 1988, action, Johnny, 132).

# Prob 16
List three ways to use the schema to restrict what values can be stored in a column.
We can restrict by NOT NULL, and adding a CONSTRAINT.
Ah yes, and by the data TYPE.

# Prob 17
Is it possible to define a default value for a column that will be considered invalid by a constraint? Create a table that tests this.
It looks like the table will be created, so no error in defining the default or the constraint, but if data is added and Postgres goes to add the default value, the constraint will still block the default value from being added if it violates the constraint.

# Prob 18
How can you see a list of all of the constraints on a table?
Just use the describe meta-command \d with the tablename.