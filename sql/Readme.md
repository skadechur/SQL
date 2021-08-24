# MySql 

## Install mysql in Mac OS using brew
Homebrew is used for installing the services.
 
1. First install brew :
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. Installing MySQL

To install MySQL
- brew install mysql 
    - This will install the latest stable version
3. Install brew services first
- brew services 
4. To start a services like mysql
- brew services start mysql
- brew services
5. mysql -uroot

### CREATING NEW USERS

Creating single user.
```sql
CREATE USER 'user1'@'localhost' IDENTIFIED BY 'password_for_user1';
```

```sql
SHOW GRANTS for  u1; #ERROR 1141 (42000): There is no such grant defined for user 'u1' on host '%'
```

### You can change the password for any user using `ALTER USER`
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
```
### Login using password.
```sql
mysql -uroot -p
```

### Display all the users.
```sql
SELECT user FROM mysql.user;
```

# DATE, DATETIME and TIMESTAMP

DATE : 
- only date part
- for string with delimiter(any): 'YYYY-MM-DD' or 'YY-MM-DD'
- for string without delimiter : 'YYYYMMDD' or 'YYMMDD'
- for number :YYYYMMDD or YYMMDD

```sql
CREATE TABLE table_name(column_name DATE);

INSERT INTO table_name VALUES('YYYY/MM/DD')
```
DATETIME :
NOW(): function for getting current time. 

```sql
CREATE TABLE table_name(column_name DATETIME DEFAULT NOW());
```


## GRANTS PRIVILEGES to users.

- ALL PRIVILEGES- as we saw previously, this would allow a MySQL user full access to a designated database (or if no database is selected, global access across the system)
- CREATE- allows them to create new tables or databases
- DROP- allows them to them to delete tables or databases
- DELETE- allows them to delete rows from tables
- INSERT- allows them to insert rows into tables
- SELECT- allows them to use the SELECT command to read through databases
- UPDATE- allow them to update table rows
- GRANT OPTION- allows them to grant or remove other users’ privileges


### Syntax:
```sql
GRANT permission_type ON database.table TO 'username'@'localhost';
```

Examples:
1.  GRANT all privileges for all databases and tables.
```sql
GRANT ALL PRIVILEGES ON  *.* TO 'user1'@'localhost' ;
```
2. GRANT specified privileges for all tables in database_name database.
```sql
GRANT INSERT,SELECT,UPDATE ON database_name.* TO 'user2'@'localhost';
```
3. GRANT DELETE privilege for the table_name in database_name.
```sql
GRANT DELETE ON database_name.table_name TO 'user3'@'localhost';
```
4. GRANT privileges at column level to different users at once
```sql
GRANT SELECT (col1), INSERT (col1, col2) ON mydb.mytbl TO 'someuser'@'somehost', 'otheruser'@'otherhost';
```
Example:
```sql
GRANT SELECT (id), INSERT (name) ON  demo1.user_1  TO 'u1'@'localhost','u2'@'localhost';
```

NOT VALID:
```sql
GRANT SELECT (id), INSERT (name) ON demo1.user_1, demo1.user_2 TO 'u1'@'localhost','u2'@'localhost';
GRANT SELECT (id), INSERT (name) ON demo1.user\* TO 'u1','u2';
GRANT SELECT (id), INSERT (name) ON demo1.user\% TO 'u1','u2';
GRANT SELECT (id), INSERT (name) ON demo1.(user_1,user_2) TO 'u1'@'localhost','u2'@'localhost';
```
VALID :
```sql
*.*
db_name.*
db_name.tbl_name
```

### NOTE : 
- Always `FLUSH PRIVILEGES` after `GRANT` and `REVOKE
- In MySQL `GRANT` syntax only permits one object in the `priv_level position`, though it may use a * as a wildcard. i.e
- Quotation marks are necessary to specify a user_name, host_name string containing special characters  or wildcard characters such as % (for example, 'test-user'@'%.com'). Quote the user name and host name separately.
- The _ and % wildcards are permitted when specifying database names in GRANT statements that grant privileges at the database level (GRANT ... ON db_name.*).Example: a _ character as part of a database name, specify it using the \ escape character as \_ in the GRANT statement(for example, GRANT ... ON `foo\_bar`.* TO ...).


## REVOKE PRIVILEGES from users

Syntax:
```sql
REVOKE permission_type ON database.table FROM 'username'@'localhost';
```
Example:

1. Revoke the privileges given to the user all at once.
```sql
REVOKE SELECT,INSERT,UPDATE ON demo.* FROM  'poonam'@'localhost';
```
2. If we have granted privileges to user and we only revoke part of it . 
```sql
SHOW GRANTS for  u2@localhost;
```
Grants for u2@localhost:                                                      
```sql
GRANT USAGE ON *.* TO `u2`@`localhost`                                       
GRANT SELECT (`id`), INSERT (`name`) ON `demo1`.`user_1` TO `u2`@`localhost` 
GRANT SELECT (`id`), INSERT (`name`) ON `demo1`.`user_2` TO `u2`@`localhost` 
```
> Revoke only part of privileges.
>```sql
>REVOKE SELECT (`id`) ON `demo1`.`user_2` FROM `u2`@`localhost`;
>```
> ```sql
>SHOW GRANTS for  u2@localhost;
>```
>Grants for u2@localhost                                                    
>```sql
>GRANT USAGE ON *.* TO `u2`@`localhost`                                       
>GRANT SELECT (`id`), INSERT (`name`) ON `demo1`.`user_1` TO `u2`@`localhost` 
>GRANT INSERT (`name`) ON `demo1`.`user_2` TO `u2`@`localhost`  
>```              

### NOTE:
- MySQL does not automatically revoke any privileges when you drop a database or table.
- Always `FLUSH PRIVILEGES` after `GRANT` and `REVOKE


## Drop the user
- Revoke the privileges before you DROP the users.
- Always `FLUSH PRIVILEGES` after `GRANT` and `REVOKE`
```sql
DROP USER 'username'@'localhost';
```
## Using MySQL in terminal

### Display databases in the server SHOW statement 
Syntax:
```sql
SHOW DATABASES;
```
2. If the database exists, try to access it.
Syntax:
```sql
USE database_name;
```
3. Creating and Selecting a Database
Syntax:
```sql
CREATE DATABASE database_name;
USE database_name;
```
4. Display all the tables in the current database
```sql
SHOW TABLES;
```
5. To verify that your table was created the way you expected.
```sql  
DESCRIBE table_name;
```
You can use DESCRIBE any time, for example, if you forget the names of the columns in your table or what types they have.

# DDL Statments
## Create database
Syntax:
```sql
CREATE DATABASE databasename;
```
## Create Table 
Syntax:
```sql
CREATE TABLE table_name (
column1 datatype,
column2 datatype,
column3 datatype,
....
);
```
Example:
```sql
CREATE TABLE if not exists countries( 
country_id int not null, 
country_name varchar(255), 
region_id int 
); 
```

Create Table Using Another Table

Synatx:
```sql
CREATE TABLE new_table_name AS
SELECT column1, column2,...
FROM existing_table_name
WHERE ....;
```
Example:
Write a SQL statement to create the structure of a table using existing table structure.
```sql
CREATE TABLE  dup_countries 
LIKE countries; 
```
Write a SQL statement to create a copy of table including structure and data from another table.
```sql
CREATE TABLE IF NOT EXISTS dup_countries 
AS SELECT * FROM  countries; 
```

## ALTER Table

- The ALTER TABLE statement is used to add, delete, or modify columns in an existing table.
- The ALTER TABLE statement is also used to add and drop various constraints on an existing table.

Syntax:
```sql
ALTER TABLE table_name
ADD column_name datatype;
```

### RENAME
1.. Write a SQL statement to rename the table countries to country_new.
```sql
ALTER TABLE countries RENAME country_new;
```
### ADD
Example:
1. Write a SQL statement to add a column region_id to the table locations.

```sql
ALTER TABLE locations
ADD region_id int not null;
```
2. Write a SQL statement to add a columns ID as the first column of the table locations.
```sql
ALTER TABLE locations
ADD ID int not null FIRST;
```
3. Write a SQL statement to add a column region_id after state_province to the table locations.
```sql
ALTER TABLE locations
ADD region_id int NOT NULL
AFTER state_province;
```
### MODIFY
Example:
Write a SQL statement change the data type of the column country_id to integer in the table locations.
```sql
ALTER TABLE locations
MODIFY country_id int;
```
Write a SQL statement to change the name of the column state_province to state, keeping the data type and size same of locations table.
```sql
ALTER TABLE locations
CHANGE state_province state varchar(25);
```
Write a SQL statement to drop the existing primary from the table locations on a combination of columns location_id and country_id.
```sql
ALTER TABLE locations
DROP PRIMARY KEY;
```

DROP 
```sql
ALTER TABLE "table_name" DROP COLUMN "column_name";

```

## DROP Table

The DROP TABLE statement is used to drop an existing table in a database.

Syntax:
```sql
DROP TABLE table_name;
```

## DML Statements


### SELECT 
SELECT DISTINCT
    Select all the unique values from a column.
Syntax:-
```sql
SELECT DISTINCT column1,
column2, ...
FROM table_name;
```
Example:-
```sql
SELECT DISTINCT Country FROM Customers;
```
### INSERT
The INSERT INTO statement is used to insert new records in a table.
Syntax:
```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```
OR
```sql
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

Example:
```sql
INSERT INTO employees VALUES(510,'Alex','Hanes','CLERK',18000,201,60);
```

### UPDATE
The SQL UPDATE Query is used to modify the existing records in a table. You can use the WHERE clause with the UPDATE query to update the selected rows, otherwise all the rows would be affected.
### Syntax :
```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```
### Examples :
1. Write a SQL statement to change the email and commission_pct column of employees table with 'not available' and 1 for those employees whose employee_id is 1
```sql
UPDATE employees SET email='not available' ,commission_pct = 1  WHERE employee_id = 1;
```
2. Write a SQL statement to change salary of employee to 8000 whose ID is 3 if the existing salary is greater than 15000.
```sql
UPDATE employees
SET salary = 8000
WHERE employee_ID = 3  AND salary > 15000;
```
3. Write a query to append @example.com to email field.
```sql  
UPDATE employees SET email = CONCAT(email, '@example.com');
```
4. Write a SQL statement to change employee_name of employee which ID is 2 , and email starts with 'sh%'
```sql
UPDATE employees  SET employee_name = 'sam'  WHERE employee_ID= 2  AND email like 'sh%';
```
5. Write a query to update the portion of the phone_no in the employees table, within the phone no the substring '8233445' will be replaced by '4456677'.
```sql
UPDATE employees  SET phone_no = REPLACE(phone_no, '8233445', '4456677' ) WHERE phone_no LIKE '%8233445%';
```

## SQL OPERATIONS
1. TOP
     Used to specify the number of records to return.
Syntax:-
```sql
SELECT TOP no_rows/percent
column_name
FROM table_name
WHERE condition;
```
Example:-
```sql
SELECT TOP 3 * FROM Customers
WHERE Country = `India` ;
```
2. LIMIT
    To limit the number of rows in the query result.
Syntax:-
```sql
SELECT column_name
FROM table_name limit no_rows;
```
Example:-
```sql
SELECT * FROM Customers
WHERE Country='India'
LIMIT 3;
```
3. BETWEEN
    Selects values within a given range.
Syntax:-
```sql
SELECT column_name
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```
Example:-
```sql
SELECT * FROM Products
WHERE Price BETWEEN 10 AND 20;
```
4. IN
    It is a multi-value operator.
Syntax:-
```sql
SELECT column_name
FROM table_name
WHERE column_name IN (value1, value2, ...);
```
Example:-
```sql
SELECT * FROM student
WHERE sname IN (‘abc’,’xyz’’rst’)
```
5. LIKE
    search for a specified pattern in a column. There are two wildcards
    1. (%): For any number of characters
    2. (_) : For single character
Syntax:-
```sql
SELECT column1, column2, ...
FROM table_name
WHERE columnN LIKE pattern;
```
Example:-
```sql
SELECT * FROM student
WHERE sname LIKE a%;
SELECT * FROM student
WHERE sname LIKE %a;
SELECT * FROM Customers
WHERE CustomerName LIKE '_r%';
```
6. SQL Aliases
    Temporary Name.
Syntax:-
```sql
SELECT column_name AS alias_name
FROM table_name;
```
Example:-
```sql
SELECT CustomerID AS ID,
CustomerName AS Customer
FROM Customers;
```
7. IS NULL
IS NULL is used to test for empty values.
Syntax:-
```sql
SELECT column_names
FROM table_name
WHERE column_name IS NULL;
```
Example:-
```sql
SELECT CustomerName, ContactName, Address
FROM Customers
WHERE Address IS NULL;
```
8. IS NOT NULL
    Used to test for non-empty values.
Syntax:-
```sql
SELECT column_name
FROM table_name
WHERE column_name IS NOT NULL;
```
Example:-
```sql
SELECT CustomerName, ContactName, Address
FROM Customers
WHERE Address IS NOT NULL;
```
9. SQL AND
    AND operator displays a record if all the conditions separated by AND are TRUE.
Syntax:-
```sql
SELECT * FROM table_name
WHERE condition1 AND condition2 ;
```
Example:-
```sql
SELECT * FROM Customers
WHERE Country='Germany' AND    City='Berlin';
```
10. SQL OR
    OR  operator displays a record if any of the conditions separated by OR are TRUE.
Syntax:-
```sql
SELECT * FROM table_name
WHERE condition1 OR condition2 OR condition3 ...;
```
Example:-
```sql
SELECT * FROM Customers
WHERE City='Berlin' OR City='Mü
```
11. SQL NOT
    NOT operator displays a record if the condition is false.
Syntax:-
```sql
SELECT * FROM table_name
WHERE NOT column_name=value;
```
Example:-
```sql
SELECT * FROM Customers
WHERE NOT Country='Germany';
```

12. ANY or ALL
- ALL means that the condition will be true only if the operation is true for all values in the range.
    (Return value boolean)
-   ANY means that the condition will be true if the operation is true for any of the values in the range.
    (Return value boolean)
Syntax:-
```sql
SELECT column_name
FROM table_name
WHERE column_name (operator ANY or ALL)
(SELECT column_name
FROM table_name
WHERE condition);
```
Example:-
```sql
SELECT ProductName
FROM Products
WHERE ProductID = ANY
(SELECT ProductID
FROM OrderDetails
WHERE Quantity = 10);
```
13. EXIST
    Returns boolean value if subquery returns any value.
Syntax:-
```sql
SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);
```
Example:-
```sql
SELECT SupplierName
FROM Suppliers
WHERE EXISTS (SELECT ProductName
FROM Products
WHERE Products.SupplierID = Suppliers.supplierID AND Price < 20);
```

### ORDER BY
Used to sort the result in ascending or descending order. It sorts the result ascending order by default.
DESC : used to sort the result in descending order.
### Syntax :
```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;
```
### Examples :
1. ASC
```sql
SELECT * FROM countries ORDER BY country_name ASC;
```
2. DESC
```sql
SELECT * FROM countries ORDER BY country_name DESC;
```
### GROUP BY
- `GROUP BY` and `HAVING`
`GROUP BY` clause,  used with aggregate functions  (`COUNT()`, `MAX()`, `MIN()`, `SUM()`, `AVG()`) are applied to groups of rows that have the same values. 
- `HAVING` clause allows us to filter the groups which are displayed. The WHERE clause filters rows before the aggregation, the HAVING clause filters after the aggregation.

### Synatx:
```sql
SELECT column_name
FROM table_name
WHERE condition
GROUP BY column_name;
```
### Example:
When you specify GROUP BY Country the result is that you get only one row for each different value of Country. All the other columns must be "aggregated" by one of SUM, COUNT ...
```sql
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country;
```
 Write a query to display number of emploees getting same salary 
```sql
SELECT COUNT(*), sal FROM emp GROUP BY sal HAVING COUNT(sal) >1;
```
## Functions
1. MAX():-
    MAX() functions return the largest value of the selected column.
Syntax:-
```sql
SELECT MAX(column_name)
FROM table_name
WHERE condition;
```
Example:-
```sql
SELECT MAX(Price)
AS  LargestPrice
FROM Products;
```
2. MIN():-
    MIN() functions return the smallest value of the selected column.
Syntax:-
```sql
SELECT MIN(column_name)
FROM table_name
WHERE condition;
```
Example:-
```sql
SELECT MIN(Price)
AS SmallestPrice
FROM Products;
```
3. AVG():-
    AVG()  functions return the average value of the selected column.
Syntax:-
```sql
SELECT AVG(column_name)
FROM table_name
WHERE condition;
```
Example:-
```sql
SELECT AVG(Price)
FROM Products;
```
4. SUM():-
    SUM() functions return the total sum of a numeric column.
Syntax:-
```sql
SELECT SUM(column_name)
FROM table_name
WHERE condition;
```
Example:-
```sql
SELECT SUM(Quantity)
FROM OrderDetails;
```
5. COUNT():-
    COUNT() functions return the number of rows that match a specified condition.
Syntax:-
```sql
SELECT COUNT(column_name)
FROM table_name
WHERE condition;
```
Example:-
```sql
SELECT COUNT(ProductID)
FROM Products
WHERE  price =18;
```

# JSON Data Type
- Json columns cannot have a not-null default value.
- JSON values are written as strings.
- Mysql parses any string used in a context that requires a JSON value, and produces an error if it is not a valid JSON.
### Creating JSON values : 
Syntax : 
```sql
CREATE TABLE TABLE_NAME (COLUMN_NAME JSON);
```
```sql
INSERT INTO TABLE_NAME VALUES('{"Key1": "value1","Key2": "value2","Key3": "value3"});
```
### Accessing the keys :
- A json array contains a list of values separated by commas and enclose within [ ] 
```sql
[ "abv", false, 10]
```
- A json array contains a set of key-value pairs separated by commas and enclosed within { } characters;
```sql          
{"key1" : "value", "key2" :10}
```
- If we try to insert a value which is not a valid JSON value, it fails to insert.
```sql
INSERT INTO t1 VALUES ('[1,2,');
```
> ERROR 3140 (22032) : Invalid JSON text : "Invalid value" at position 6 in value for column 't1,jdoc'.


To display the desired value using key_name as the key, but without including the surrounding quote marks or any escapes, use the inline path operator ->>, like this:

```sql
 SELECT column_name->>"$.key_name" FROM table_name;
```

# TRIGGER 
### What is a trigger ?
- A DB trigger is a code or programs that automatically execute with response to some event on a table or view in a database.
- Mainly, triggers help to maintain the integrity of the database.
- To create a trigger
    - CREATE TRIGGER TRIGGER_NAME;
- To drop a trigger
    - DROP TRIGGER TRIGGER_NAME;
- If you drop a table, any triggers for the table are also dropped.

Synatx:
```sql
CREATE
    [DEFINER = user]
    TRIGGER trigger_name
    trigger_time trigger_event
    ON tbl_name FOR EACH ROW
    [trigger_order]
    trigger_body

trigger_time: { BEFORE | AFTER }

trigger_event: { INSERT | UPDATE | DELETE }

trigger_order: { FOLLOWS | PRECEDES } other_trigger_name
```
## STEPS :
### 1. Create table_1 and insert values into table_1.
```sql
CREATE TABLE countries(
country_id int not null auto_increment primary key,
country_name varchar(35) default " ",
Region_id int,
created_at Datetime, 
updated_at Datetime
);
```
```sql
INSERT INTO countries(country_id,country_name) VALUES(1,"INDIA");
INSERT INTO countries(country_id,country_name) VALUES(2,"USA");
```
### 2. Create table_2
```sql
CREATE TABLE countries_hist(
hist_country_id int not null auto_increment primary key,
hist_country_name varchar(35) default " ",
Region_id int,
hist_created_at Datetime, 
hist_updated_at Datetime
);
```
### 3. Create a trigger on table_1.

Change the delimiter to tell mysql to treat the statements, stored procedures or triggers as an entire statement.
Syntax:
``` 
delimiter <delimiter>
```

```sql
delimiter |
CREATE TRIGGER countries_trig BEFORE UPDATE ON countries
FOR EACH ROW
BEGIN
INSERT INTO countries_hist SET  hist_country_id = OLD.country_id, hist_country_name = OLD.country_name, hist_region_id = OLD.region_id, hist_created_at = OLD.created_at, hist_updated_at = OLD.updated_at;
END;
|
delimiter ;
```
### 4. To Show trigger
```sql
SHOW TRIGGER;
```
### 5. Update Table_1
```sql
UPDATE countries SET  country_id=1 WHERE country_name="INDIA";
```
> NOTE: Table_2 must be updated 
### READ THIS :
- The keyword BEFORE indicates the trigger action time. 
In this case, the trigger activates before each row is inserted into the table. 
- The other permitted keyword here is AFTER.
- INSERT operations cause trigger activation. In an INSERT trigger, only NEW.col_name can be used, there is no old row.
- We can also create triggers for DELETE and UPDATE operations.
- In a DELETE trigger, only OLD.col_name can be used, there is no new row. 
- In an UPDATE trigger, you can use OLD.col_name to refer to the columns of a row before it is updated and NEW.col_name to refer to the columns of the row after it is updated.
- The statement following FOR EACH ROW defines the trigger body; that is, the statement to execute each time the trigger activates, which occurs once for each row affected by the triggering event.
- We can use a trigger to modify the values to be inserted into a new row or used to update a row.
- By using the BEGIN ... END construct, you can define a trigger that executes multiple statements.
# CASCADE
It is used in conjunction with ON DELETE or ON UPDATE. It means that the child data is either deleted or updated when the parent data is deleted or updated.
### ON DELETE / ON UPDATE CASCADE 
-   MySQL provides a more effective way called ON DELETE CASCADE referential action for a foreign key that allows you to delete data from child tables automatically when you delete the data from the parent table.
-   When the row from the parent table is deleted.

Syntax:
```sql
CREATE TABLE child_table
(
  column1 datatype [ NULL | NOT NULL ],
  column2 datatype [ NULL | NOT NULL ],
  ...

  CONSTRAINT fk_name
    FOREIGN KEY (child_col1, child_col2, ... child_col_n)
    REFERENCES parent_table (parent_col1, parent_col2, ... parent_col_n)
    ON DELETE CASCADE
    [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ] 
);
```
### Example 
1. CREATING PARENT TABLE 
```sql
CREATE TABLE buildings (
building_no INT PRIMARY KEY AUTO_INCREMENT,
building_name VARCHAR(255) NOT NULL,
address VARCHAR(255) NOT NULL
);
```
2. CREATING CHILD TABLE
 ```sql
CREATE TABLE rooms (
room_no INT PRIMARY KEY AUTO_INCREMENT,
room_name VARCHAR(255) NOT NULL,
building_no INT NOT NULL,
FOREIGN KEY (building_no)
REFERENCES buildings (building_no)
ON DELETE CASCADE
);
```
3. INSERTING INTO PARENT TABLE
```sql
INSERT INTO buildings(building_name,address) VALUES('HYBE','Dacehi-dong Seoul South Korea');
```
4. INSERTING INTO CHILD TABLE
```sql
INSERT INTO rooms(room_name,building_no)VALUES('Amazon',1);
```
5. Deleting row from parent.
```sql
DELETE FROM buildings WHERE building_no=1;
```


## Index

- improves the speed of data retrieval on a table
- The query optimizer may use indexes to quickly locate data without having to scan every row in a table for a given query.
- case the columns are the string columns, it consume  memory.

Solution :- column_name(length)
- When you create a secondary index for a column, MySQL stores the values of the columns in a separate data structure

1. Single Index
Syntax:
```sql
CREATE INDEX index_name ON table_name (column_name);
```
Example:
```sql
CREATE INDEX phone_index ON customer(phone);
```

Example:
```sql
CREATE INDEX name_index ON customer (name(10));
```
2. composite /multiple-column index

At time of creating table.
 Syntax:
 ```sql
 CREATE TABLE table_name (
    c1 data_type PRIMARY KEY,
    c2 data_type,
    c3 data_type,
    c4 data_type,
    INDEX index_name (c2,c3,c4) #composite index consisting of three column c2,c3 and c4
);
 ```
 Adding to an existing table.
 ```sql
 CREATE INDEX index_name 
ON table_name(c2,c3,c4);
 ```

## Show indexes of a table
```sql
SHOW INDEXES FROM table_name;
```









