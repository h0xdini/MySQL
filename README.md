# MySQL

> SQL is a relational database owned by Oracle, to manage it we can use the command line, phpMyAdmin and Oracle WorkBench.

<br/>

## Basic DataBase Creation and Deletion

### Creation

```sql
CREATE DATABASE db_name;
CREATE DATABASE IF NOT EXISTS db_name;
```

### Deletion

```sql
DROP DATABASE db_name;
DROP DATABASE IF EXISTS db_name;
```

### Switch between databases

```sql
USE db_name;
```

### Choose a database

```sql
SHOW DATABASES LIKE "db_name";
```

<br/>
<br/>

## Data Types

### Numeric Types

| Numeric type  |  Max chars |
| --- | --- |
| TinyInt | 4 |
| SmallInt | 6 |
| MediumInt | 9 |
| Int | 11 |
| BigInt | 20 |
| Serial | 20(BigInt) |
| Boolean | 1(TinyInt) |

> Note: Each type has a specific value range

<br/>

### Date & Time Types

| Date type | Format |
| --- | --- |
| Date | YYYY-MM-DD |
| Datetime | YYYY-MM-DD HH:MM:SS |
| Timestamp | YYYY-MM-DD HH:MM:SS |
| Time | HH:MM:SS |
| Year | YYYY |

> Max Date: 9999-12-31

> Year: 4 charachter or 2 characters

<br/>

### String Types

| String type | Max Length |
| --- | --- |
| Char | 255(must specify) |
| VarChar | 65.535 |
| TinyText | |
| Text |  |
| MediumText |  |
| LongText |   |
| Blob |  |
| Enum |   |
| Set |   |

> Char: stores a fixed value (ex. 10 charachters always), faster than VarChar because it uses static memory

> VarChar: stores a variable value, slower than Char because it uses dynamic memory

> Text: Stores long strings (Dealt with && compared depending on Charset)

> Blob: Binary large object (used for storing images and files, etc...), Dealt with && compared depending on numeriv value of the bytes

> Set: same as Enum but we can select multiple values 

<br/>
<br/>

## Tables

### Create a table

> Switch to the target database: USE db_name;

> Must specify number of columns

```sql
CREATE TABLE IF NOT EXISTS students (
  id INT(11),
  name VARCHAR(255),
  email VARCHAR(255)
);
```

<br/>

### Display a table

```sql
DESCRIBE table_name;
SHOW COLUMNS FROM table_name;
SHOW FIELDS FROM table_name;
```

<br/>

### Tables status

```sql
SHOW TABLE STATUS;
```

<br/>

### Table creation code

```sql
SHOW CREATE TABLE table_name;
```

<br/>

### Drop table

```sql
DROP TABLE IF EXISTS table_name;
```

<br/>

### Rename table

```sql
RENAME TABLE table_name TO new_table_name;
```

<br/>

#### Rename multiple tables

```sql
RENAME TABLE table_name TO new_table_name, table2 TO new table2_new;
```

<br/>

### Change the storage engine of the table 

```sql
ALTER TABLE table_name ENGINE = MYISAM;
```

<br/>
<br/>

## ALTER

> Edits the structure of the table

### Add new column

```sql
ALTER TABLE students ADD password VARCHAR(255); // adds it at the end
ALTER TABLE students ADD username VARCHAR(255) AFTER name; // adds it after the name field
ALTER TABLE students ADD role VARCHAR(255) FIRST; // adds it at the beginning
```

<br/>

### DROP new column

```sql
ALTER TABLE students DROP password;
```

<br/>

### Change fields order

```sql
ALTER TABLE students CHANGE password password VARCHAR(255) AFTER username;
```

> The data type is required

<br/>

### Change data type of a field

```sql
ALTER TABLE table_name CHANGE field field text(50); // we can also change the second name argument

ALTER TABLE table_name MODIFY field char(255);
```

<br/>

### Rename table

```sql
ALTER TABLE table_name RENAME new_name;
```

<br/>

### Change and Modify together

```sql
ALTER TABLE table_name MODIFY field VARCAHR(255), CHANGE table_name new_table;
```

<br/>

### Change the char set

```sql
ALTER TABLE table_name CONVERT TO CHAR SET utf8; // default charset: latin1
```

<br/>
<br/>

## Constraints

> SQL constraints are used to specify rules for the data in a table.

> Constraints can be specified when the table is created with the `CREATE TABLE` statement, or after the table is created with the `ALTER TABLE` statement.


#### Syntax with create table

```sql
CREATE TABLE table_name (
    column1 datatype constraint,
    column2 datatype constraint,
    column3 datatype constraint,
    ....
);
```

> Constraints are used to limit the type of data that can go into a table. This ensures the accuracy and reliability of the data in the table. If there is any violation between the constraint and the data action, the action is aborted.

> Constraints can be column level or table level. Column level constraints apply to a column, and table level constraints apply to the whole table.

<br/>

<p>The following constraints are commonly used in SQL:</p>

- **NOT NULL** : Ensures that a column cannot have a NULL value.
- **UNIQUE** : Ensures that all values in a column are diffrent.
- **PRIMARY KEY** : A combination of `NOT NULL` and `UNIQUE`. Uniquely identifies each row in a table.
- **FOREIGN KEY** : Prevents actions that would destroy links between tables
- **CHECK** : Ensures that the values in a column satisfies a specific condition
- **DEFAULT** : Sets a default value for a column if no value is specified
- **CREATE INDEX** : Used to create and retrieve data from the database very quickly

### NOT NULL

#### Modify NULL property

```sql
ALTER TABLE table_name MODIFY field_name field__data_type NOT NULL;
```

#### Add a new field with NOT NULL activated

```sql
ALTER TABLE table_name ADD isActive BOOLEAN NOT NULL;
```

<br/>

### UNIQUE

> The UNIQUE constraint ensures that all values in a column are different.

> Both the `UNIQUE` and `PRIMARY KEY` constraints provide a guarantee for uniqueness for a column or set of columns.

> A `PRIMARY KEY` constraint automatically has a UNIQUE constraint.

> However, you can have many `UNIQUE` constraints per table, but only one `PRIMARY KEY` constraint per table.

#### Syntax

 ```sql
 CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    UNIQUE (ID)
);
```

To name a UNIQUE constraint, and to define a UNIQUE constraint on multiple columns, use the following SQL syntax:

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CONSTRAINT UC_Person UNIQUE (ID,LastName)
);
```

To create a UNIQUE constraint on the "ID" column when the table is already created, use the following SQL:

```sql
ALTER TABLE table_name ADD UNIQUE(field_name);
```

To name a UNIQUE constraint, and to define a UNIQUE constraint on multiple columns, use the following SQL syntax:

```sql
ALTER TABLE Persons
ADD CONSTRAINT UC_Person UNIQUE (ID,LastName);
```

To drop a UNIQUE constraint, use the following SQL:

```sql
ALTER TABLE Persons
DROP INDEX field; // we can replace field by a constraint
```

Make the field NOT NULL and UNIQUE

```sql
ALTER TABLE table_name ADD field_name VARCHAR(255) NOT NULL UNIQUE;
```

<br/>

### PRIMARY KEY

> The PRIMARY KEY constraint uniquely identifies each record in a table.

> Primary keys must contain `UNIQUE` values, and cannot contain `NULL` values.

> A table can have only ONE primary key; and in the table, this primary key can consist of single or multiple columns (fields).

#### SQL PRIMARY KEY on CREATE TABLE

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    PRIMARY KEY (ID)
);

Second method--

CREATE TABLE Persons (
    ID int NOT NULL PRIMARY KEY,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int
);
```

To allow naming of a PRIMARY KEY constraint, and for defining a PRIMARY KEY constraint on multiple columns, use the following SQL syntax:

```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CONSTRAINT PK_Person PRIMARY KEY (ID,LastName)
);
```

⚠️ **Note:** In the example above there is only ONE PRIMARY KEY (PK_Person). However, the VALUE of the primary key is made up of TWO COLUMNS (ID + LastName).

#### SQL PRIMARY KEY on ALTER TABLE

```sql
ALTER TABLE table_name ADD PRIMARY KEY (column_name);
```

To allow naming of a PRIMARY KEY constraint, and for defining a PRIMARY KEY constraint on multiple columns, use the following SQL syntax:

```sql
ALTER TABLE table_name ADD CONSTRAINT constraint_name PRIMARY_KEY (field1, field2...);
```

⚠️ **Note:** If you use `ALTER TABLE` to add a primary key, the primary key column(s) must have been declared to not contain NULL values (when the table was first created).

#### DROP a PRIMARY KEY Constraint

```sql
ALTER TABLE table_name DROP PRIMARY KEY;
```

```sql
ALTER TABLE table_name DROP CONSTRAINT constraint_name;
```

<br/>

### FOREIGN KEY Constraint

> A `FOREIGN KEY` is a field (or collection of fields) in one table, that refers to the `PRIMARY KEY` in another table.

> The table with the foreign key is called the child table, and the table with the primary key is called the referenced or parent table.


#### Persons Table

| personeId | lastName | firstName | age |
| --- | --- | --- | --- |
| 1 | Hansen | Ola | 30 |
| 2 | Svendson | Tove | 23 |
| 3 | Petterson | Kari | 20 |

#### Orders Table

| orderId | orderNumber | personId |
| --- | --- | --- |
| 1 | 77895 | 3 |
| 2 | 44678 | 3 |
| 3 | 22456 | 2 |
| 4 | 24562 | 1 |

> Notice that the "PersonID" column in the "Orders" table points to the "PersonID" column in the "Persons" table.

> The "PersonID" column in the "Persons" table is the PRIMARY KEY in the "Persons" table.

> The "PersonID" column in the "Orders" table is a FOREIGN KEY in the "Orders" table.

⚠️ **Note:** The FOREIGN KEY constraint prevents invalid data from being inserted into the foreign key column, because it has to be one of the values contained in the parent table.

#### SQL FOREIGN KEY on CREATE TABLE

```sql
CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);
```

for defining a `FOREIGN KEY` constraint on multiple columns, use the following SQL syntax:

```sql
CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID)
    REFERENCES Persons(PersonID)
);
```

#### SQL FOREIGN KEY on ALTER TABLE

```sql
ALTER TABLE table_name ADD FOREIGN KEY (clientId) REFRENCES clients(id)
ON UPDATE CASCADE 
ON DELETE CASCADE;
```

for defining a FOREIGN KEY constraint on multiple columns, use the following SQL syntax:

```sql
ALTER TABLE table_name
ADD CONSTRAINT constraint_name
FOREIGN KEY (other_table_primary_key_in_here) REFRENCES other_table(other_table_primary_key);
```

#### DROP a FOREIGN KEY constraint

```sql
ALTER TABLE table_name DROP FOREIGN KEY fk_name;
```

```sql
ALTER TABLE table_name DROP CONSTRAINT fk_constraint_name;
```
