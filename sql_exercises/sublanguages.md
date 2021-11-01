# Part 1
SQL consists of 3 different sublanguages. For example, one of these sublanguages is called the Data Control Language (DCL) component of SQL. It is used to control access to a database; it is responsible for defining the rights and roles granted to individual users. Common SQL DCL statements include:
```
GRANT
REVOKE
```
Name and define the remaining 2 sublanguages, and give at least 2 examples of each.

Response:
The two remaining sublanguages are DDL/Data Definition Language and DML/Data Manipulation Language.

Data Definition involves defining how the data is structured, so CREATE tables and databases,
or ALTER
where Data Manipulation involves CRUD-dy things to create/read/update/delete which in SQL
is SELECT/INSERT/UPDATE/DELETE

# Part 2
Does the following statement use the Data Definition Language (DDL) or the Data Manipulation Language (DML) component of SQL?
```
SELECT column_name FROM my_table;
```
This is DML. Working with the data itself.

# Part 3
Does the following statement use the DDL or DML component of SQL?

```
CREATE TABLE things (
  id serial PRIMARY KEY,
  item text NOT NULL UNIQUE,
  material text NOT NULL
);
```
This is defining how the data will be structured, not manipulating any data itself, so this is DDL.

# Part 4
Does the following statement use the DDL or DML component of SQL?
```
ALTER TABLE things
DROP CONSTRAINT things_item_key;
```
This involves the data structure again, so DDL.

# Part 5
Does the following statement use the DDL or DML component of SQL?
```
INSERT INTO things VALUES (3, 'Scissors', 'Metal');
```
This is one of the CRUD actions, so DML.

# Part 6
Does the following statement use the DDL or DML component of SQL?
```
UPDATE things
SET material = 'plastic'
WHERE item = 'Cup';
```
This is manipulating data itself, so DML.

# Part 7
Does the following statement use the DDL, DML, or DCL component of SQL?
```
\d things
```
This is a meta-command, which doesn't actually change anything it is only looking at
the schema of the table 'things' so I am going to go with DDL. It doesn't look at 
who has access or who owns the table so DDL feels more correct.
Yeah, it felt like a trick question but I figured that it might be lumped in, in some
weird way.

# Part 8
Does the following statement use the DDL or DML component of SQL?
```
DELETE FROM things WHERE item = 'Cup';
```
This is part of CRUD, so DML.

# Part 9
Does the following statement use the DDL or DML component of SQL?
```
DROP DATABASE xyzzy;
```
This is dealing with how a database is defined, or in this case kind of "un-defined" so DDL.

# Part 10
Does the following statement use the DDL or DML component of SQL?
```
CREATE SEQUENCE part_number_sequence;
```
This is part of DDL, deals with the schema with the data as a by-product.