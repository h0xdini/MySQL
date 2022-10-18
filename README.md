# MySQL

> SQL is a relational database owned by Oracle, to manage it we can use the command line, phpMyAdmin and Oracle WorkBench.

<br/>

## Basic Creation and Deletion

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

