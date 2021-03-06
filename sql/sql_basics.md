# SQL Overview
_SQL (Structured Query Language)_ is the standard language for communicating with relational database management systems.  There's a few variations including Microsoft's SQL Server, MySQL, PostgreSQL, SQLite.  Below instructions are with MySQL.  We'll setup a MySQL server, then use a client to connect to the server and query for data.

## Installation
On a mac, use homebrew to install mysql using `$brew install mysql` to install the MySQL as a server.  For a GUI client, you can download MySQL Workbench, which will allow you to visually see the data with a GUI (as opposed to only command line).

## Bash Commands

###  Useful MySQL commands (Start, Stop Server)
    $mysql.server start  # start the server
    $mysql.server stop  # stop the server
    $mysql.server status  # check if server is running
    $mysql.server help  # get help commands

### Run a text file (containing sql scripts) from command line
    $mysql < myscript.sql

### Switching from bash to shell
    $mysql  # enters into the shell from bash
    $mysql -u root -p  # enters into the shell as root user
    mysql>  # This is what you'll see when in shell

## Shell Commands

### Create, Drop, and Use a Database
    mysql> DROP DATABASE IF EXISTS my_database_name;
    mysql> CREATE DATABASE my_database_name;
    mysql> USE my_database_name;

### Basic Format of SQL Queries

Assuming data (say mytable from mydatabase) looks like:

    # last_name, first_name, suffix, city, state, birth, death
    Adams, John, , Braintree, MA, 1735-10-30, 1826-07-04
    Adams, John Quincy, , Braintree, MA, 1767-07-11, 1848-02-23
    Arthur, Chester A., , Fairfield, VT, 1829-10-05, 1886-11-18
    Buchanan, James, , Mercersburg, PA, 1791-04-23, 1868-06-01
    Bush, George H.W., , Milton, MA, 1924-06-12, 

We can do the following query:

    SELECT last_name, first_name, state
    FROM mydatabase.mytable;

*  a database holds a lot of tables
*  a table holds lots of rows and columns of data
*  a record is a particular instance of a row and column

### COUNT

    SELECT COUNT(*) FROM sampdb.president
    SELECT COUNT(first_name), COUNT(death) FROM sampdb.president  # Returns 42 and 38
    SELECT COUNT(DISTINCT state, first_name) FROM sampdb.president

*  Counts the not NULL values passed into it
*  This is an 'aggregating' function - takes a set of records or values and performs calculations on that set
*  For `Distinct`, we get 42 instead of 43 total presidents, means one duplicate state and first_name combo
*  Can also Subset and Count the data

### Other Functions (AVG, SUM, MIN, MAX)

    SELECT AVG(death) FROM sampdb.president
    SELECT SUM(age) FROM sampdb.president
    SELECT MIN(age) FROM sampdb.president
    SELECT MAX(age) FROM sampdb.president

### Functions (AS, DATEDIFF, CONCAT, etc.)

    SELECT ... score * 2 AS doubled_score
    SELECT ... DATEDIFF(death, birth)
    SELECT ... CONCAT(first_name, ' ', last_name)
    SELECT ... UNIQUE(last_name, middle_name, first_name)

*  Can create an alias with `AS` (required for subqueries)
*  Can run functions (e.g. `CONCAT` combines strings into one, `DATEDIFF` gets difference in days)
*  Can get unique values using `UNIQUE`

### NULL Values and Functions

*  __NULL__ values represent missing or unknown data (not the same as the value 0).
*  To test for _NULL_, do __IS NULL__ or __IS NOT NULL__.
    -  __IS NULL__ checks if the value is null.  For example, `SELECT LastName, FirstName FROM Persons WHERE LastName IS NULL`
    -  __IS NOT NULL__ checks if the value is not null.  For example, `SELECT LastName, FirstName FROM Persons WHERE LastName IS NOT NULL`
*  There are functions __ISNULL(), NVL(), IFNULL(), and COALESCE()__ that specify how we want to treat a _NULL_ value
    -  For MySQL, _IFNULL()_ tells us how to treat a _NULL_ value (in this case, replacing it with 0): `SELECT UnitPrice*(UnitsInStock+IFNULL(UnitsOnOrder,0)) FROM Products`

### WHERE

    SELECT COUNT(first_name) FROM sampdb.president WHERE last_name="Adams" 

*  Filters (before `GROUP BY`)
*  Can pass in lists (using `IN`) or tuples
    -  Filter by list: ... `WHERE state IN ('MA', 'VA')`
    -  Filter by tuple: ... `WHERE (last_name, state) IN ('Adams', 'MA')`

### GROUP BY

    SELECT state, COUNT(*) FROM sampdb.president GROUP BY state ORDER BY state

*  GROUP BY is a subset; divides records by unique tuples

### GROUP BY using WHERE and HAVING

    SELECT state, COUNT(*) FROM sampdb.president
       WHERE birth <= '1900-01-01'
       GROUP BY state
       HAVING COUNT(*) >=2
       ORDER BY COUNT(*);

Sample Output:
    
    state, count(*)
       MA, 2
       VT, 2
       NC, 2
       NY, 4
       OH, 7
       VA, 8

*  WHERE filters before the GROUP BY
*  HAVING filters after the GROUP BY

### Subqueries
*  Subqueries is a query inside another query
*  Usually this means an __outer query__ and an __inner query__
   `SELECT name FROM city WHERE pincode IN (SELECT pincode FROM pin WHERE zone = 'west')`
   -  __outer query__ is `SELECT name FROM city WHERE pincode IN ...`
   -  __inner query__ is `SELECT pincode FROM pin WHERE zone = 'west'`
*  Returned table has to have the fields and row expected of it by the calling query
*  Alias (`AS`) is required for subqueries
*  Two varieties of subqueries (__correlated__ and __non-correlated__)
    
    -  __Non-correlated subquery__ means that the _inner query_ doesn't depend on the _outer query_ and can run as a stand alone query
    
      *  `SELECT company FROM stock WHERE listed_on_exchange = (SELECT ric FROM market WHERE country='japan')`
        - The _inner query_ executes first, then the _outer query_.
        - _non-correlated subqueries_ usually use `IN` or `NOT IN`

    -  __Correlated subquery__ means that the _inner query_ depends on the _outer query_
    
      *  `SELECT student_id, score FROM sampdb.score AS scr WHERE event_id = 3 AND score > ( SELECT AVG(score) FROM sampdb.score WHERE event_id = scr.event_id GROUP BY event_id) )`
        - Requires use of alias (`AS`)
        - The _outer query_ executes first, then the _inner query_; this is because the _inner query_ depends on the output of the _outer query_
        - These queries are slower than _non-correlated queries_ (think about doing a _join_ instead)
        - _correlated subqueries_ usually use `EXISTS` or `NOT EXISTS`

### EXISTS

*  The __EXISTS__ and __NOT EXISTS__ conditions are used with a subquery (usually the _correlated subquery_).  Condition is met if the subquery returns at least one row.  It can be used with _SELECT, INSERT, UPDATE, or DELETE_ statements.
*  `SELECT * FROM mysuppliers WHERE NOT EXISTS (SELECT * FROM myorders WHERE mysuppliers.supplier_id = myorders.order_id);`
*  Note that these are very inefficient since the sub-query is re-run for every single row so use only if no other solution


### JOINS
Used to combine rows from two or more tables.  Types of joins include:

*  __INNER JOIN__ selects all rows from both tables that have a match
    - `SELECT column_name(s) FROM table1 INNER JOIN table2 ON table1.column_name=table2.column_name;`
*  __LEFT JOIN__ selects all rows from the left table (table1) and matching rows on the right table (table2)
    - `SELECT column_name(s) FROM table1 LEFT JOIN table2 ON table1.column_name=table2.column_name;`
*  __RIGHT JOIN__ selects all rows from the right table (table2) and matching rows on the left table (table1)
    - `SELECT column_name(s) FROM table1 RIGHT JOIN table2 ON table1.column_name=table2.column_name;`
*  __FULL JOIN__ selects all rows from the left table (table1) and all rows on the right table (table2)
    - `SELECT column_name(s) FROM table1 FULL OUTER JOIN table2 ON table1.column_name=table2.column_name;`
*  __UNION and UNION ALL__ combines the result of two or more SELECT statements (with each statement having the same number of columns, data types, and order).  _UNION_ selects distinct values only.  _UNION ALL_ allows duplicates.
    -  `SELECT column_name(s) FROM table1 UNION SELECT column_name(s) FROM table2`
    -  `SELECT column_name(s) FROM table1 UNION ALL SELECT column_name(s) FROM table2`

### Primary Key (PK) and Foreign Key (FK)
*  A __primary key__ constraint uniquiely identifies each record in a database table.  These values must be unique, cannot contain NULL values, and each table can only have one primary key.  Note that you can have multiple column values that make up the primary key (e.g. P_ID, LastName, SSN).
    -  Add a _primary key_ to a table with a single column (e.g. P_ID) as the constraint: `ALTER TABLE MyTable ADD PRIMARY KEY (P_ID)`
    -  Add a _primary key_ to a table with multiple columns (e.g. P_ID, LastName) as the constraint: `ALTER TABLE MyTable ADD PRIMARY KEY(P_ID, LastName)`
    -  Remove a _primary key_: `ALTER TABLE MyTable DROP PRIMARY KEY`
*  A __foreign key__ constraint in one table is just a _primary key_ in another table.
    -  Add a _foreign key_ on a single column as the constraint: `ALTER TABLE MyTable ADD FOREIGN KEY (P_ID) REFERENCES OtherTable(P_ID)`
    -  Add a _foreign key_ on multiple columns (e.g. P_ID) as the constraint: `ALTER TABLE MyTable ADD CONSTRAINT my_fk FOREIGN KEY (P_ID) REFERENCES OtherTable(P_ID)`
    -  Remove a _foreign key_ with: `ALTER TABLE MyTable DROP FOREIGN KEY my_fk`

### DROP and TRUNCATE
*  To delete an entire database, be extra sure you want to and can spell correctly, then do: `DROP DATABASE MyDatabase`.  This deletes everything in the database.
*  To delete an entire table (including the table itself), be extra sure you want to and can spell correctly, then do: `DROP TABLE MyTable`
*  To delete the data inside an entire table, do: `TRUNCATE TABLE MyTable`

### ALTER
*  To add, delete, or modify columns in an existing table, use __ALTER__.
*  To add a column in a table, use: `ALTER TABLE MyTable MODIFY COLUMN MyColumn <datatype>`
*  To delete a column in a table, use: `ALTER TABLE MyTable DROP COLUMN MyColumn`
*  To modify a column in a table, use: `ALTER TABLE MyTable MODIFY COLUMN MyColumn <datatype>`

### INSERT
*  To insert data by field: `INSERT INTO MyDatabase.MyTable SET ThisField = ThisData`
*  To insert data by row: `INSERT INTO MyDatabase.MyTable VALUES ('field1', 'field2', 'field3'`

### UPDATE
    UPDATE MyDatabase.MyTable SET ThisField = ThisData

### DELETE
    DELETE FROM MyDatabase.MyTable  # Careful, no criteria deletes the entire database!

### IF
    IF(2>1, 'OK', 'NOT)

### CASE
    CASE
        WHEN 2 > 1
            THEN 'OK'
        WHEN 1==1
            THEN 'YEP'
        ELSE
            'NOT OK'
    END

You can combine all of the above into something like this (Note: From SQL Server):
    
    SELECT IVRName, 
    SUM(CASE WHEN Disconnection in ('Answer') THEN 1 END) AS Answered, 
    SUM(CASE WHEN Disconnection in ('Abandon') THEN 1 END) AS Abandoned
    FROM LifeNetDW.dbo.QueueMetrics
    WHERE CallTime > '2015-1-31' AND IVRName IS NOT NULL
    GROUP BY IVRName


## Other

### Schemas
*  Command: SHOW DATABASES;
    -  Shows all databases available on the server
*  Command: SHOW TABLES IN books;
    - Shows all tables available in a particular database
*  Command: DESCRIBE books.authors;
    - Shows columns, data types, keys, in a particular table
*  Command: SHOW CREATE TABLE books.authors\G;
    - Shows the command for creating a table

### Indexing
*  Makes processing things much faster
*  An index is a pre-ordered sort of a particular field
*  Done and stored by the database server
*  Indexing has storage and performance costs
*  For every index on a table, the database must reorder the index for a new entry
*  Each index takes up space in storage and memory
*  Do 'profiling' to see where things are slow
*  To create an index, do: `CREATE INDEX MyIndex on MyTable (colToIndex)`
*  To create an index on a combination of volumns, do: `CREATE INDEX MyIndex on MyTable(colToIndex1, colToIndex2, colToIndex3)`
*  To drop an index, do: `ALTER TABLE MyTable DROP INDEX MyIndex`

### Normalization (Nth normal form)
*  Principles of Modeling Data 'norms'
*  Rules for making sure data is where we expect it and that it isn't duplicated
*  __First Normal Form (1NF)__
    - Data should be atomic (i.e. values should not be combined)
    - E.g. George Washington shouldn't be a value (because it combines first and last name)
    - No repeating columns
    - E.g. Columns like Author1, Author2, Author3
*  __Second Normal Form (2NF)__
    - No 'partial dependencies'
    - Cannot determine a nonkey column value with only a portion of the primary key
    - Basically, unnecessary duplication
*  __Third Normal Form (3NF)__
    - Second Normal Form plus no nonkey column depends on the value of another nonkey column
    - Values are driven by the primary key fields; these values should be removed to a table of their own


