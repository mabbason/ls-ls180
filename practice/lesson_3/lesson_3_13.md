# Write the SQL statement needed to create a join table that will allow a film to have multiple directors, and directors to have multiple films. Include an id column in this table, and add foreign key constraints to the other columns.

CREATE TABLE directors_films (
  id serial PRIMARY KEY,
  film_id int REFERENCES films(id),
  director_id REFERENCES directors(id)
);

# Write the SQL statements needed to insert data into the new join table to represent the existing one-to-many relationships.

INSERT INTO directors_films
(film_id)
VALUES
(1),
(2),
(3),
(4),
(5),
(6),
(7),
(8),
(9),
(10);

UPDATE directors_films
SET director_id = films.director_id
FROM films
WHERE films.id = directors_films.film_id;

# Write a SQL statement to remove any unneeded columns from films.

lesson_3_13=# ALTER TABLE films
lesson_3_13-# DROP COLUMN director_id;

# Write a SQL statement that will return the following result:
           title           |         name
---------------------------+----------------------
 12 Angry Men              | Sidney Lumet
 1984                      | Michael Anderson
 Casablanca                | Michael Curtiz
 Die Hard                  | John McTiernan
 Let the Right One In      | Michael Anderson
 The Birdcage              | Mike Nichols
 The Conversation          | Francis Ford Coppola
 The Godfather             | Francis Ford Coppola
 Tinker Tailor Soldier Spy | Tomas Alfredson
 Wayne's World             | Penelope Spheeris
(10 rows)

Response:
lesson_3_13=# SELECT films.title, directors.name FROM
lesson_3_13-# films INNER JOIN directors_films ON films.id = directors_films.film_id
lesson_3_13-# INNER JOIN directors ON directors.id = directors_films.director_id
lesson_3_13-# ORDER BY films.title;

# Write SQL statements to insert data for the following films into the database:
Film	Year	Genre	Duration	Directors
Fargo	1996	comedy	98	Joel Coen
No Country for Old Men	2007	western	122	Joel Coen, Ethan Coen
Sin City	2005	crime	124	Frank Miller, Robert Rodriguez
Spy Kids	2001	scifi	88	Robert Rodriguez

Response:
lesson_3_13=# INSERT INTO films
(title, year, genre, duration)
VALUES
('Fargo', 1996, 'comedy', 98),
('No Country for Old Men', 2007, 'western', 122),
('Sin City', 2005, 'crime', 124),
('Spy Kids', 2001, 'scifi', 88);

lesson_3_13=# INSERT INTO directors
(name)                        
VALUES
('Joel Cohen'),
('Ethan Cohen'),                                 
('Frank Miller'),
('Robert Rodriguez');

INSERT INTO directors_films
(film_id, director_id)
VALUES
(11, 9),
(12, 9),
(12, 10),
(13, 11),
(13, 12),
(14, 12);

# Write a SQL statement that determines how many films each director in the database has directed. Sort the results by number of films (greatest first) and then name (in alphabetical order).

lesson_3_13=# SELECT directors.name AS director, count(films.id) AS films_directed FROM
lesson_3_13-# directors INNER JOIN directors_films ON directors.id = directors_films.director_id
lesson_3_13-# INNER JOIN films ON films.id = directors_films.film_id
lesson_3_13-# GROUP BY directors.name
lesson_3_13-# ORDER BY count(films.id) DESC,
lesson_3_13-# directors.name;
