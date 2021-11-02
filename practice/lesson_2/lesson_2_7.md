# Prob 1
Load the file into your database.

What does the file do?
It checks if a table already exits, deletes it if it does, then creates a table with three columns to hold general film information. Then it loads data for three films into the table

What is the output of the command? What does each line in this output mean?
psql:/home/mabba/ls-ls180/practice/films1.sql:1: NOTICE:  table "films" does not exist, skipping 
DROP TABLE (this command is getting skipped)
CREATE TABLE (says that a new table was created)
INSERT 0 1 (inserted one row into a table)
INSERT 0 1 (inserted one row into a table)
INSERT 0 1 (inserted one row into a table)

Open up the file and look at its contents. What does the first line do?
Deletes a table if it already exists.

# Prob 2
Write a SQL statement that returns all rows in the films table.

SELECT * FROM films;

# Prob 3
Write a SQL statement that returns all rows in the films table with a title shorter than 12 letters.

SELECT * FROM films WHERE LENGTH(title) < 12;

# Prob 4
Write the SQL statements needed to add two additional columns to the films table: director, which will hold a director's full name, and duration, which will hold the length of the film in minutes.

ALTER TABLE films
ADD COLUMN director text,
ADD COLUMN duration integer
;

# Prob 5
Write SQL statements to update the existing rows in the database with values for the new columns:
title	director	duration
Die Hard	John McTiernan	132
Casablanca	Michael Curtiz	102
The Conversation	Francis Ford Coppola	113

UPDATE films 
SET director = 'John McTiernan', duration = 132
WHERE title='Die Hard';

UPDATE films 
SET director = 'Michael Curtiz', duration = 102
WHERE title='Casablanca';

UPDATE films 
SET director = 'Francis Ford Coppola', duration = 113
WHERE title='The Conversation';

# Prob 6
Write SQL statements to insert the following data into the films table:

title	year	genre	director	duration
1984	1956	scifi	Michael Anderson	90
Tinker Tailor Soldier Spy	2011	espionage	Tomas Alfredson	127
The Birdcage	1996	comedy	Mike Nichols	118

INSERT INTO films
(title, year, genre, director, duration)
VALUES('1984', 1956, 'scifi', 'Michael Anderson', 90),
('Tinker Tailor Soldier Spy', 2011, 'espionage', 'Tomas Alfredson', 127),
('The Birdcage', 1996, 'comedy', 'Mike Nichols', 118);

# Prob 7
Write a SQL statement that will return the title and age in years of each movie, with newest movies listed first:

SELECT title, (2021 - year) AS age FROM films ORDER BY year DESC;

(I see the proper way now, instead of my manually entering the date:
SELECT title, extract("year" from current_date) - "year" AS age
  FROM films
  ORDER BY age ASC;
)

# Prob 8
Write a SQL statement that will return the title and duration of each movie longer than two hours, with the longest movies first.

SELECT title, duration FROM films WHERE duration > 120 ORDER BY duration DESC;

# Prob 9
Write a SQL statement that returns the title of the longest film.

SELECT title FROM films ORDER BY duration DESC LIMIT 1;