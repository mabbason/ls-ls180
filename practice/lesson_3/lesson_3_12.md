# The books_categories table from this database was created with foreign keys that don't have the NOT NULL and ON DELETE CASCADE constraints. Go ahead and add them now.

ALTER TABLE books_categories
DROP CONSTRAINT books_categories_book_id_fkey,
DROP CONSTRAINT books_categories_category_id_fkey,
ADD FOREIGN KEY (book_id) REFERENCES books(id) ON DELETE CASCADE,
ADD FOREIGN KEY (category_id) REFERENCES categories(id) ON DELETE CASCADE,
ALTER COLUMN book_id SET NOT NULL,
ALTER COLUMN category_id SET NOT NULL;

# Write a SQL statement that will return the following result:
 id |     author      |           categories
----+-----------------+--------------------------------
  1 | Charles Dickens | Fiction, Classics
  2 | J. K. Rowling   | Fiction, Fantasy
  3 | Walter Isaacson | Nonfiction, Biography, Physics
(3 rows)

Response:
SELECT books.id, books.author, string_agg(categories.name, ', ') AS categories FROM
books INNER JOIN books_categories ON books.id = books_categories.book_id
INNER JOIN categories ON categories.id = books_categories.category_id
GROUP BY books.id
ORDER BY books.id;