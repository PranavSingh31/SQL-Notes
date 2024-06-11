# SQL Notes -> Structured Query Language
It is a language

MySQL is a relational DBMS

CRUD - Create Read Update Delete

First SQL Code
--------------
```
CREATE DATABASE IF NOT EXISTS temp;
USE temp;
CREATE TABLE student(
	id INT PRIMARY KEY,
	name VARCHAR(255)
);
INSERT INTO student VALUES (1,'PRANAV')
SELECT * FROM student
```
`DROP TABLE student`
DataTypes in SQL
----------------
INT, FLOAT, CHAR, VARCHAR, TEXT, BLOB(for audio/video)

Sizes :
TINY, SMALL, MEDIUM, --, BIG

> ex. TINYINT, SMALLINT, MEDIUMINT, INT, BIGINT

DECIMAL, DATE, DATETIME, TIMESTAMP, TIME, ENUM, SET, BOOLEAN, BIT

Types of SQL Commands
---------------------
1. DDL (Data Definition Language)
   + CREATE
   + ALTERTABLE (to change property of a column)
   + DROP, TRUNCATE (to remove all rows)
   + RENAME... 
> keywords like WHERE doesn't work on these.

2. DRL/DQL (retrieval/query)
   + SELECT

3. DML (modification)
   + INSERT
   + UPDATE
   + DELETE

4. DCL (control)
   + GRANT
   + REVOKE

5. TCL (transmission control)
   + START TRANSACTION
   + COMMIT
   + ROLLBACK
   + SAVEPOINT

TABLE WITH FOREIGN KEY SYNTAX
-----------------------------
```
CREATE TABLE temp1(
 id INT,
 amount INT(10),
 date DATETIME,
 FOREIGN KEY (id)
	REFERENCES temp2(id2)
   	ON DELETE CASCADE
);
```

DUAL TABLE
----------
The DUAL table is a special one-row, one-column table that is present by default in some Oracle database systems. It's not a standard part of SQL; rather, it's a feature specific to Oracle databases. The DUAL table is often used for performing simple calculations, testing expressions, and retrieving system-level values without needing to select from an actual table. It's a convenient tool for querying or evaluating expressions when you don't need to interact with any real table data.

Ex

`SELECT 10 + 5 FROM DUAL;`

`SELECT SYSDATE FROM DUAL;`

`SELECT 'Hello' AS Greeting FROM DUAL;`

`SELECT CASE WHEN 5 > 3 THEN 'True' ELSE 'False' END FROM DUAL;`

SOME OTHER KEYWORDS
-------------
WHERE, BETWEEN, IN, AND, OR, NOT, IS NULL

Ex

`SELECT * FROM table1 WHERE age BETWEEN 0 AND 100;`

`SELECT * FROM table1 WHERE col_name NOT IN (1,2,3,4);`

> Notes : `IN` is a very useful keyword, we can reduce the statement length by using IN and providing a list rather than individual comparison

`DESC student` describes the table

PATTERN MATCHING
----------------
% matches any number of character from 0 to n just like * in regex

_ matches only one character

`SELECT * FROM customer WHERE name LIKE '%p_';`

> Notes: `LIKE` is used here for specific filtering

SORTING in SQL using ORDER BY
-----------------------------
Ex. 

`SELECT name FROM customer ORDER BY salary;` (ASC is by default)

`SELECT name FROM customer ORDER BY salary DESC;`

GROUPING in SQL using GROUP BY
------------------------------
** `DISTINCT` ** is used to get distinct values

Ex. 
`SELECT DISTINCT department FROM customer`

> GROUP BY is generally used with aggregate functions like COUNT, AVG, MIN, MAX, SUM

> GROUP BY and DISTINCT are literally same, MySQL uses DISTINCT as GROUP BY if no aggregate function is used

Ex.

`SELECT department FROM worker GROUP BY department`

`SELECT department, COUNT(department) FROM worker GROUP BY department`

`SELECT department, AVG(salary) FROM worker GROUP BY department`



** `HAVING`** Keyword is just like we use WHERE with SELECT, HAVING is used with GROUP BY

> basically a filtering tool

Ex.

`SELECT department, COUNT(department) FROM worker GROUP BY department HAVING COUNT(department) > 2;`

CONSTRAINTS (DLL)
-----------
**1. PRIMARY KEY (not null, unique, only one primary key per table)**
   
syntax : Two ways to define

either first one or second
```
CREATE TABLE student(
	id INT PRIMARY KEY,
	name VARCHAR(255)
);
```
```
CREATE TABLE student(
	id INT,
	name VARCHAR(255),
	PRIMARY KEY (id)
);
```

**2. FOREIGN KEY (refers primary key of other table)**
   
> syntax : write at last line

`FOREIGN KEY (cust_id) REFERENCES customer(id)`

**3. UNIQUE KEY (syntax is similar to previous ones)**

**4. CHECK (consistency constraint)**
   
Ex. 

`CONSTRAINT acc_balance_check CHECK(balance > 1000);`

**5. NOT NULL**
   
`Balance INT NOT NULL;`

**6. DEFAULT**
   
`Balance INT NOT NULL DEFAULT 0;`


ALTER OPERATIONS
----------------
`ADD` -> ```ALTER TABLE workers ADD new_col_name datatype ADD new_col_name2 datatype;```

adds new column with datatype

`MODIFY` -> ```ALTER TABLE workers MODIFY col-name col-datatype;```

changes datatype of an attribute

`CHANGE COLUMN` -> ```ALTER TABLE workers CHANGE COLUMN old-col-name new-col-name new-col-datatype;```

rename column name

`DROP COLUMN` -> ```ALTER TABLE workers DROP COLUMN col-name;```

drop a column completely

`RENAME` -> ```ALTER TABLE workers RENAME TO workers2;```

rename table name itself

DATA MODIFICATION LANGUAGE
--------------------------
`UPDATE`, `INSERT`, `DELETE`

Ex.

`INSERT INTO workers VALUES (1,'Hey'-----);`

`UPDATE workers SET address='Mumbai', gender='M' WHERE id = 121;`

> when we perform update, or delete operation over every row, MySQL prevents this for protection against viruses using SQL_SAFE_UPDATES, so to disable it

`SET SQL_SAFE_UPDATES = 0;
UPDATE workers SET pincode = 110000;`

Ex.

`DELETE FROM workers where id = 121;`

`DELETE FROM workers` (deletes the whole table but ofc SQL_SAFE_UPDATES would appear)

**referential constraints on delete:** `ON DELETE CASCADE` and `ON DELETE SET NULL`

> directly deleting a parent row that has child on an another table world throw an error. To maintain the integrity, we can apply these referential constraints

`ON DELETE CASCADE` deletes the parent row as well as referenced row on the child table.

`ON DELETE SET NULL` on the other hand, sets the attribute on the referenced row of child table to NULL.

Ex. 
```
CREATE TABLE Order_Details(
	Order_id INT PRIMARY_KEY,
	delivery_date DATE,
	cust_id INT,
	FOREIGN KEY(cust_id) references Customer(id) ON DELETE CASCASE;
);
```
```
CREATE TABLE Order_Details(
	Order_id INT PRIMARY_KEY,
	delivery_date DATE,
	cust_id INT,
	FOREIGN KEY(cust_id) references Customer(id) ON DELETE SET NULL;
);
```

### REPLACE

Cases:
1) If data is already present, it would replace the data.
2) If data is not present, it would work as INSERT.

Ex.
`REPLACE INTO Customer(id,city) VALUES(1251,'Colony);`

`REPLACE INTO Customer(id, cname,city) SELECT id ,cname, city FROM Customer WHERE id = 500;` 
> this would make everything NULL other than selected attributes

#### REPLACE vs UPDATE
if data is not present REPLACE would work as INSERT but UPDATE would throw an error

JOINS
------

+ **`INNER JOIN`**
returns a resultant table that has matching values from both the tables or all the tables.

> Suppose table A have 1,2,3 and B have 1,2,4,5 rows, then resultant rows would be (1-combined),(2-combined)

`SELECT column-list FROM table1 INNER JOIN table2 ON condition1`

Ex. 

`SELECT c.*, o.* FROM customer AS c INNER JOIN orders AS o ON c.id = o.cust_id`

+ **`OUTER JOIN`**
  
  - `LEFT JOIN` : Resulting table has matching values of both left and right table, and all the table from left table.

     > Suppose table A have 1,2,3 and B have 1,2,4,5 rows, then resultant rows would be (1-combined),(2-combined),3

  - `RIGHT JOIN` : Resulting table has matching values of both left and right table, and all the table from right table.

     > Suppose table A have 1,2,3 and B have 1,2,4,5 rows, then resultant rows would be (1-combined),(2-combined),4,5

     SYNTAX is pretty much same as INNER JOIN 

  - `FULL JOIN` : Resulting tables just combines data from both tables. We use this to remove redundant rows upon combining.

    > Suppose table A have 1,2,3 and B have 1,2,4,5 rows, then resultant rows would be (1-combined),(2-combined),3,4,5

> **There is no keyword as FULL JOIN in MySQL, therefore, we use LEFT JOIN U RIGHT JOIN.**

Ex.
```
SELECT * FROM customers LEFT JOIN orders ON customers.id = orders.customer_id
UNION
SELECT * FROM customers RIGHT JOIN orders ON customers.id = orders.customer_id;
```

+ **`CROSS JOIN`**

returns all the cartesian products of the rows present in both tables. Hence, all possible variations are reflected in the output.

rarely used in practical purpose.

> Suppose table A have 1,2,3 and B have 1,2 rows, then resultant rows would be (1+1),(1+2),(2+1),(2+2),(3+1),(3+2)

Ex.

`SELECT column-lists FROM table1 CROSS JOIN table2`

+ **`SELF JOIN`**
  
joins a table to itself

> **MySQL doesn't have INNER JOIN, so we emulate it like this:**

Ex.

`SELECT * FROM table1 AS t1 INNER JOIN table1 AS t2 ON t1.column_name = t2.column_name;`

**Can we join two tables without using JOIN keyword?**

Yes,

Ex. (INNER JOIN)

`SELECT * FROM lefttable, righttable WHERE lefttable.id = righttable.id`

SET OPERATIONS
--------------

Tables should be of same type, the datatypes and number of columns

> Suppose A have A-1,B-1,C-2 and B have B-2,D-3 then their UNION would have A-1,B-1,C-2,B-2,D-3

> Suppose if it was FULL JOIN on first column, it would generate (A-1-NULL),(B-1-2),(C-2-NULL),(D-NULL-3)

+ UNION
  
Ex.

`SELECT * FROM table1 UNION SELECT * FROM table2;`

+ INTERSECT
  
> **MySQL doesn't provide such keyword so we need to emulate it.**

Ex.

`SELECT DISTINCT id FROM t1 INNER JOIN t2 USING(id);`

+ MINUS
  
> **Again, MySQL doesn't provide such keyword so we need to emulate it.**

Ex.

`SELECT column_list FROM table1 LEFT JOIN table2 ON table1.column_name = table2.column_name WHERE table2.column_name IS NULL;`

SUB-QUERIES
-----------

Nested Queries (Generally, outer queries depends on inner queries)

Alternative to join

Ex,

`SELECT max(age) FROM (SELECT * FROM employee WHERE fname LIKE '%A%') as TEMP;`

+ Corelated subquery :

With a normal nested subquery, the inner SELECT query runs first and executes once, returning values to be used by the main query. A correlated subquery, however executes once for each candidate row considered by the outer query. In other words, the inner query is driven by the outer query.

### Differenece between JOIN & SUBQUERIES

VIEWS
-----

used to hide irrelevant info and make a custom view

`CREATE VIEW custom_view AS SELECT fname, age FROM employee;`

`SELECT * FROM custom_view;`

`ALTER VIEW custom_view AS SELECT fname, lname, age FROM employee;`

`DROP VIEW OF EXISTS custom_view`




