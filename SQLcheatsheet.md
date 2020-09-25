# SQL CheatSheet
*Pau Tarragó Navarra*

## Data Types
| Name | Parameters | Comments |
| ---- |:---------- | :------- |
| `CHAR` | `(n)` | fixed length string, n being the string length, max 255 |
| `VARCHAR` | `(n)` | variable length string, n being the max string length, if n > 255 it is converted into `TEXT` variable |
| `TEXT` | | up to 65 535 characters |
| `BLOB` | | up to 65 535 bytes of data |
| `LONGTEXT` | | up to 4 294 967 295 characters |
| `LONGBLOB` | | up to 4 294 967 295 bytes of data |
| `ENUM` | `(x, y, z, ...)` | list of possible values, if a value is inserted that is not in the list a blank value will be inserted, sorted in the insertion order |
| `SET` | | similar to `ENUM` but can store more than one choice |
| `INT` | `(n)` | intiger, n is optional to limit a maximum size, `UNSIGNED*` for unsigned values |
| `BIGINT` | `(n)` | intiger, n is optional to limit a maximum size, `UNSIGNED*` for unsigned values |
| `FLOAT` | `(n, d)` | floating decimal, n, d are optional and limit the maximum size of the intiger and decimal part respectively |
| `DOUBLE` | `(n, d)` | floating decimal, n, d are optional and limit the maximum size of the intiger and decimal part respectively |
| `DECIMAL` | `(n, d)` | `DOUBLE` stored as a string, n, d are optional and limit the maximum size of the intiger and decimal part respectively |
| `DATE` | `()` | YYYY-MM-DD format |
| `TIME` |`()`| HH:MM:SS format |
| `DATETIME` | `()` | YYYY-MM-DD HH:MM:SS format |
| `TIMESTAMP` | `()` | number of seconds since the Unix epoch ('1970-01-01 00:00:00' UTC) |
| `YEAR` | `()` | can be in a 2-digit format or a 4-digit format |

## Condition Operators
| Type | Operators | Comments |
| ---- |:---------- | :------- |
| Arithmetic | `+, -, *, / ` | |
| Comparation | `=, >, <, >=, <=, <>` | `<>` operator means `!=`|
| Bitwise | `&, \|, ^` | AND, OR, XOR respectively |
| Logical | `ALL, ANY, NOT, AND, OR, BETWEEN, IN, EXISTS, LIKE, SOME` | |

For more details see [this](https://www.w3schools.com/sql/sql_operators.asp).

## Table Creation
```sql
create table table_name
    (column_name data_type [column_restriction] [default_value])
    [, column_name data_type [column_restriction] [default_value], ...]
    table_restrictions);
```
### Column Restrictions
| Name | Parameters | Comments |
| ---- |:---------- | :------- |
| `UNIQUE` | | column can't have repeated values |
| `PRIMARY KEY` | | this column is the primary key for the table |
| `REFERENCE` | `table [column]` | this column is a foreign key referencing said table/column |
| `CHECK` | `(condition ...)` | conditions can only reference self column |
| `NOT NULL` | | this column can't contain null values |
### Table Restrictions
| Name | Parameters | Comments |
| ---- |:---------- | :------- |
| `UNIQUE` | `column ...` | this set of columns must have unique tuples |
| `PRIMARY KEY` | `column ...` | this set of columns are the table primary keys |
| `FOREIGN KEY`<br>`REFERENCE` | `column ...`<br>`table [column]`| this set of columns are foreign keys to the table/column referenced |
| `CHECK` | `(condition ...)` | conditions can only reference columns from self table |

## Tuple Insertion
```sql
insert into table_name [column, ...]
(values {val | NULL} [, ...] | query);
```
Values must be the same data types as indicated in `create table` and in the same order.

## Tuple Deletion
```sql
delete from table_name
where condition ...;
```

## Tuple Modification
```sql
update table_name
set column_name = {expression | NULL} [, ...]
where condition ...;
```

## Query
### Basic format
```sql
select column_name [, ...] | *
from table_name
[where condition ...];
```
\* means select all columns from said table.

### Ordered
```sql
select column_name [, ...] | *
from table_name
[where condition ...]
order by column_name [DESC | ASC] [, ...];
```
If not specified, it defaults to `ASC`.

### Without repetitions
```sql
select [DISTINCT | ALL] column_name [, ...] | *
from table_name
[where condition ...];
```
If not specified, it defaults to `ALL`.

### Grouping
```sql
select column_name [, ...] | *
from table_name
[where condition ...]
group by column_name [, ...]
[having condition ...];
```

### Agregation functions
| Name | Parameters | Comments |
| ---- |:---------- | :------- |
| `COUNT` | `(* \| DISTINCT column_name \| column_name)` | null values are not counted |
| `SUM` | `expression` | |
| `MIN` | `expression` | |
| `MAX` | `expression` | |
| `AVG` | `expression` | |

### More than one table
```sql
select k.column_name [, ...] | *
from table_name k [[, | INNER JOIN | NATURAL INNER JOIN] ...]
[where condition ...]
group by k.column_name [, ...]
[having condition ...];
```

`k` is the key that indicates from which table the column is referencing to.

### Query union
```sql
select k.column_name [, ...] | *
from table_name k [[, | INNER JOIN | NATURAL INNER JOIN] ...]
[where condition ...]
UNION
select k.column_name [, ...] | *
from table_name k [[, | INNER JOIN | NATURAL INNER JOIN] ...]
[where condition ...]
[order by k.column_name [DESC | ASC] [, ...]];
```
The two selects must be compatible, and the `order by` command afects both queries.