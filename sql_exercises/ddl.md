# first create a postgresql database named "extrasolar", and then create two tables in the database as follows:

`stars` table

-   `id`: a unique serial number that auto-increments and serves as a primary key for this table.
-   `name`: the name of the star,e,g., "Alpha Centauri A". Allow room for 25 characters. Must be unique and non-null.
-   `distance`: the distance in light years from Earth. Define this as a whole number (e.g., 1, 2, 3, etc) that must be non-null and greater than 0.
-   `spectral_type`: the spectral type of the star: O, B, A, F, G, K, and M. Use a one character string.
-   `companions`: how many companion stars does the star have? A whole number will do. Must be non-null and non-negative.

`planets` table

-   `id`: a unique serial number that auto-increments and serves as a primary key for this table.
-   `designation`: a single alphabetic character that uniquely identifies the planet in its star system ('a', 'b', 'c', etc.)
-   `mass`: estimated mass in terms of Jupiter masses; use an integer for this value.
CREATE DATABASE extrasolar;

PSQL COMMANDS:

CREATE DATABASE extrasolar;

\c extrasolar

CREATE TABLE stars (
  id serial PRIMARY KEY,
  name varchar(25) UNIQUE NOT NULL,
  distance integer NOT NULL CHECK (distance > 0),
  spectral_type char(1),
  companions integer NOT NULL CHECK (companions >= 0)
);

CREATE TABLE planets (
  id serial PRIMARY KEY,
  designation char(1) UNIQUE,
  mass integer
);

# You may have noticed that, when we created the stars and planets tables, we did not do anything to establish a relationship between the two tables. They are simply standalone tables that are related only by the fact that they both belong to the extrasolar database. However, there is no relationship between the rows of each table. 
# To correct this problem, add a star_id column to the planets table; this column will be used to relate each planet in the planets table to its home star in the stars table. Make sure the column is defined in such a way that it is required and must have a value that is present as an id in the stars table.

ALTER TABLE planets ADD COLUMN star_id int REFERENCES stars(id) NOT NULL;

# Hmm... it turns out that 25 characters isn't enough room to store a star's complete name. For instance, the star "Epsilon Trianguli Australis A" requires 30 characters. Modify the column so that it allows star names as long as 50 characters.

ALTER TABLE stars ALTER COLUMN name TYPE varchar(50);

# For many of the closest stars, we know the distance from Earth fairly accurately; for instance, Proxima Centauri is roughly 4.3 light years away. Our database, as currently defined, only allows integer distances, so the most accurate value we can enter is 4. Modify the distance column in the stars table so that it allows fractional light years to any degree of precision required.

ALTER TABLE stars ALTER COLUMN distance TYPE real;

# The spectral_type column in the stars table is currently defined as a one-character string that contains one of the following 7 values: 'O', 'B', 'A', 'F', 'G', 'K', and 'M'. However, there is currently no enforcement on the values that may be entered. Add a constraint to the table stars that will enforce the requirement that a row must hold one of the 7 listed values above. Also, make sure that a value is required for this column.

ALTER TABLE stars ADD CONSTRAINT spectral_value_check
CHECK(spectral_type IN ('O', 'B', 'A', 'F', 'G', 'K', 'M'));

ALTER TABLE stars ALTER COLUMN spectral_type SET NOT NULL;