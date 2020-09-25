# SQL CheatSheet
*Pau Tarragó Navarra*

## Data Types
| Name | Parameters | Comments |
| ---- |:---------- | :------- |
| `CHAR` | `(n)` | fixed length string, $n \in [0, 255]$ being the string length |
| `VARCHAR` | `(n)` | variable length string, $n \in [0, 255]$ being the max string length, if $n > 255$ it is converted into `TEXT` variable |
| `TEXT` | | up to $65535$ characters |
| `BLOB` | | up to $65535$ bytes of data |
| `LONGTEXT` | | up to $4294967295$ characters |
| `LONGBLOB` | | up to $4294967295$ bytes of data |
| `ENUM` | `(x, y, z, ...)` | list of possible values, if a value is inserted that is not in the list a blank value will be inserted, sorted in the insertion order |
| `SET` | | similar to `ENUM` but can store more than one choice |
| `INT` | `(n)` | intiger, $n$ is optional to limit a maximum size, `UNSIGNED*` for unsigned values |
| `BIGINT` | `(n)` | intiger, $n$ is optional to limit a maximum size, `UNSIGNED*` for unsigned values |
| `FLOAT` | `(n, d)` | floating decimal, $n, d$ are optional and limit the maximum size of the intiger and decimal part respectively |
| `DOUBLE` | `(n, d)` | floating decimal, $n, d$ are optional and limit the maximum size of the intiger and decimal part respectively |
| `DECIMAL` | `(n, d)` | `DOUBLE` stored as a string, $n, d$ are optional and limit the maximum size of the intiger and decimal part respectively |
| `DATE` | `()` | YYYY-MM-DD format |
| `TIME` |`()`| HH:MM:SS format |
| `DATETIME` | `()` | YYYY-MM-DD HH:MM:SS format |
| `TIMESTAMP` | `()` | number of seconds since the Unix epoch ('1970-01-01 00:00:00' UTC) |
| `YEAR` | `()` | can be in a 2-digit format or a 4-digit format |

## Table Creation
```sql
create table <table_name>
    
```