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

DataTypes in SQL
----------------
INT, CHAR, VARCHAR, TEXT, BLOB(for audio/video)

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

3. DRL/DQL (retrieval/query)
   + SELECT

4. DML (modification)
   + INSERT
   + UPDATE
   + DELETE

5. DCL (control)
   + GRANT
   + REVOKE

6. TCL (transmission control)
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

**5. DEFAULT**
   
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




