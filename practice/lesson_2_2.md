#What kind of programming language is SQL?
A special purpose language, because it's only used for working with relational databases
unlike JS, Ruby, C#, etc which are general languages.

#What are the three sublanguages of SQL?
DDL, DML, DCL
Data Definition Language
Data Manipulation Language
Data Control Language

#Write the following values as quoted string values that could be used in a SQL query.
canoe: 'canoe'
a long road: 'a long road'
weren't: 'weren''t'
"No way!": '"No way!"'

#What operator is used to concatenate strings?
The || is used to concatenate, for example
SELECT first_name || ' ' || last_name AS full_name FROM tablename;

#What function returns a lowercased version of a string? Write a SQL statement using it.
lower(string) for examples
SELECT lower('MiXeD') FROM tablename;

#How does the psql console display true and false values?
true = t
false = f

#The surface area of a sphere is calculated using the formula A = 4π r2, where A is the surface area and r is the radius of the sphere.
Use SQL to compute the surface area of a sphere with a radius of 26.3cm, truncated to return an integer.
SELECT trunc(4 * pi() * 26.3 ^ 2);