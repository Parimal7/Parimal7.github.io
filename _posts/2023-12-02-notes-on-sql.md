---
layout: post
title: Notes on SQL
---
## Intro

These notes are taken from http://sqlbolt.com, a website with interactive SQL quizzes.

## Common SQL Queries

```sql
-- Select query for a specific columns
SELECT column, another_column, … FROM mytable;

-- Select query for all columns
SELECT * FROM mytable;

-- Select query with constraints
SELECT column, another_column, … FROM mytable WHERE _condition_ AND/OR _another_condition_ AND/OR …;

-- Select query with unique results
SELECT DISTINCT column, another_column, … FROM mytable WHERE _condition(s)_;

-- Select query with ordered results
SELECT column, another_column, … FROM mytable WHERE _condition(s)_ ORDER BY column ASC/DESC;

-- Select query with limited rows
SELECT column, another_column, … FROM mytable WHERE _condition(s)_ ORDER BY column ASC/DESC LIMIT num_limit OFFSET num_offset;

-- Select query with INNER JOIN on multiple tables
SELECT column, another_table_column, … FROM mytable INNER JOIN another_table ON mytable.id = another_table.id WHERE _condition(s)_ ORDER BY column, … ASC/DESC LIMIT num_limit OFFSET num_offset;

-- Select query with LEFT/RIGHT/FULL JOINs on multiple tables
SELECT column, another_column, … FROM mytable INNER/LEFT/RIGHT/FULL JOIN another_table ON mytable.id = another_table.matching_id WHERE _condition(s)_ ORDER BY column, … ASC/DESC LIMIT num_limit OFFSET num_offset;

-- Select query with constraints on NULL values
SELECT column, another_column, … FROM mytable WHERE column IS/IS NOT NULL AND/OR _another_condition_ AND/OR …;

-- Example query with expressions
SELECT particle_speed / 2.0 AS half_particle_speed FROM physics_data WHERE ABS(particle_position) * 10.0 > 500;

-- Example query with both column and table name aliases
SELECT column AS better_column_name, … FROM a_long_widgets_table_name AS mywidgets INNER JOIN widget_sales ON mywidgets.id = widget_sales.widget_id;

-- Select query with aggregate functions over all rows
SELECT AGG_FUNC(_column_or_expression_) AS aggregate_description, … FROM mytable WHERE _constraint_expression_;

-- Select query with aggregate functions over groups
SELECT AGG_FUNC(_column_or_expression_) AS aggregate_description, … FROM mytable WHERE _constraint_expression_ GROUP BY column;

-- Select query with HAVING constraint
SELECT group_by_column, AGG_FUNC(_column_expression_) AS aggregate_result_alias, … FROM mytable WHERE _condition_ GROUP BY column HAVING _group_condition_;

-- Complete SELECT query
SELECT DISTINCT column, AGG_FUNC(_column_or_expression_), … FROM mytable JOIN another_table ON mytable.column = another_table.column WHERE _constraint_expression_ GROUP BY column HAVING _constraint_expression_ ORDER BY _column_ ASC/DESC LIMIT _count_ OFFSET _COUNT_;

-- Insert statement with values for all columns
INSERT INTO mytable VALUES (value_or_expr, another_value_or_expr, …), (value_or_expr_2, another_value_or_expr_2, …), …;

-- Insert statement with specific columns (The number of values need to match the number of columns specified). Inserting values this way has the benefit of being forward compatible. For example, if you add a new column to the table with a default value, no hardcoded `INSERT` statements will have to change as a result to accommodate that change.
INSERT INTO mytable (column, another_column, …) VALUES (value_or_expr, another_value_or_expr, …), (value_or_expr_2, another_value_or_expr_2, …), …;

-- Update statement with values
UPDATE mytable SET column = value_or_expr, other_column = another_value_or_expr, … WHERE condition;

-- Delete statement with condition
DELETE FROM mytable WHERE condition;

-- Create table statement w/ optional table constraint and default value
CREATE TABLE IF NOT EXISTS mytable ( column _DataType_ _TableConstraint_ DEFAULT _default_value_, another_column _DataType_ _TableConstraint_ DEFAULT _default_value_, … );

-- Altering table to add new column(s)
ALTER TABLE mytable ADD column _DataType_ _OptionalTableConstraint_ DEFAULT default_value;

-- Altering table to remove column(s)
ALTER TABLE mytable DROP column_to_be_deleted;

-- Altering table name
ALTER TABLE mytable RENAME TO new_table_name;

-- Drop table statement
DROP TABLE IF EXISTS mytable;
```

## Common operators for numerical data:

|  Operator |  Condition |  SQL Example |
|---|---|---|
|=, !=, < <=, >, >=|Standard numerical operators|col_name != 4|
|BETWEEN … AND …|Number is within range of two values (inclusive)|col_name BETWEEN 1.5 AND 10.5|
|NOT BETWEEN … AND …|Number is not within range of two values (inclusive)|col_name NOT BETWEEN 1 AND10|
|IN (…)|Number exists in a list|col_name IN (2, 4, 6)|
|NOT IN (…)|Number does not exist in a list|col_name NOT IN (1, 3, 5)|

## Common operators for text data:

|Operator|Condition|Example|
|---|---|---|
|=|Case sensitive exact string comparison (_notice the single equals_)|col_name = "abc"|
|!= or <>|Case sensitive exact string inequality comparison|col_name != "abcd"|
|LIKE|Case insensitive exact string comparison|col_name LIKE "ABC"|
|NOT LIKE|Case insensitive exact string inequality comparison|col_name NOT LIKE "ABCD"|
|%|Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE)|col_name LIKE "%AT%"  <br>(matches "AT", "ATTIC", "CAT" or even "BATS")|
|_|Used anywhere in a string to match a single character (only with LIKE or NOT LIKE)|col_name LIKE "AN_"  <br>(matches "AND", but not "AN")|
|IN (…)|String exists in a list|col_name IN ("A", "B", "C")|
|NOT IN (…)|String does not exist in a list|col_name NOT IN ("D", "E", "F")|

Note: All strings must be quoted so that the query parser can distinguish words in the string from SQL keywords.

## Common aggregate functions

|Function|Description|
|---|---|
|COUNT(\*), COUNT(column)|A common function used to counts the number of rows in the group if no column name is specified. Otherwise, count the number of rows in the group with non-NULL values in the specified column.|
| MIN(column) | Finds the smallest numerical value in the specified column for all rows in the group. |
|MAX(column)|Finds the largest numerical value in the specified column for all rows in the group.|
|AVG(column)|Finds the average numerical value in the specified column for all rows in the group.|
|SUM(column)|Finds the sum of all numerical values in the specified column for the rows in the group.|


## Common data types

|Data type|Description|
|---|---|
|`INTEGER`, `BOOLEAN`|The integer datatypes can store whole integer values like the count of a number or an age. In some implementations, the boolean value is just represented as an integer value of just 0 or 1.|
|`FLOAT`, `DOUBLE`, `REAL`|The floating point datatypes can store more precise numerical data like measurements or fractional values. Different types can be used depending on the floating point precision required for that value.|
|`CHARACTER(num_chars)`, `VARCHAR(num_chars)`, `TEXT`|The text based datatypes can store strings and text in all sorts of locales. The distinction between the various types generally amount to underlaying efficiency of the database when working with these columns.<br><br>Both the CHARACTER and VARCHAR (variable character) types are specified with the max number of characters that they can store (longer values may be truncated), so can be more efficient to store and query with big tables.|
|`DATE`, `DATETIME`|SQL can also store date and time stamps to keep track of time series and event data. They can be tricky to work with especially when manipulating data across timezones.|
|`BLOB`|Finally, SQL can store binary data in blobs right in the database. These values are often opaque to the database, so you usually have to store them with the right metadata to requery them.|

## Common table constraints

|Constraint|Description|
|---|---|
|`PRIMARY KEY`|This means that the values in this column are unique, and each value can be used to identify a single row in this table.|
|`AUTOINCREMENT`|For integer values, this means that the value is automatically filled in and incremented with each row insertion. Not supported in all databases.|
|`UNIQUE`|This means that the values in this column have to be unique, so you can't insert another row with the same value in this column as another row in the table. Differs from the `PRIMARY KEY` in that it doesn't have to be a key for a row in the table.|
|`NOT NULL`|This means that the inserted value can not be `NULL`.|
|`CHECK (expression)`|This allows you to run a more complex expression to test whether the values inserted are valid. For example, you can check that values are positive, or greater than a specific size, or start with a certain prefix, etc.|
|`FOREIGN KEY`|This is a consistency check which ensures that each value in this column corresponds to another value in a column in another table.  <br>  <br>For example, if there are two tables, one listing all Employees by ID, and another listing their payroll information, the `FOREIGN KEY` can ensure that every row in the payroll table corresponds to a valid employee in the master Employee list.|

## Query order of execution

1. `FROM` and `JOIN`s
The `FROM` clause, and subsequent `JOIN`s are first executed to determine the total working set of data that is being queried. This includes subqueries in this clause, and can cause temporary tables to be created under the hood containing all the columns and rows of the tables being joined.

2. `WHERE`
Once we have the total working set of data, the first-pass `WHERE` constraints are applied to the individual rows, and rows that do not satisfy the constraint are discarded. Each of the constraints can only access columns directly from the tables requested in the `FROM` clause. Aliases in the `SELECT` part of the query are not accessible in most databases since they may include expressions dependent on parts of the query that have not yet executed.

3. `GROUP BY`
The remaining rows after the `WHERE` constraints are applied are then grouped based on common values in the column specified in the `GROUP BY` clause. As a result of the grouping, there will only be as many rows as there are unique values in that column. Implicitly, this means that you should only need to use this when you have aggregate functions in your query.

4. `HAVING`
If the query has a `GROUP BY` clause, then the constraints in the `HAVING` clause are then applied to the grouped rows, discard the grouped rows that don't satisfy the constraint. Like the `WHERE` clause, aliases are also not accessible from this step in most databases.

5. `SELECT`
Any expressions in the `SELECT` part of the query are finally computed.

6. `DISTINCT`
Of the remaining rows, rows with duplicate values in the column marked as `DISTINCT` will be discarded.

7. `ORDER BY`
If an order is specified by the `ORDER BY` clause, the rows are then sorted by the specified data in either ascending or descending order. Since all the expressions in the `SELECT` part of the query have been computed, you can reference aliases in this clause.

8. `LIMIT` / `OFFSET`
Finally, the rows that fall outside the range specified by the `LIMIT` and `OFFSET` are discarded, leaving the final set of rows to be returned from the query.

## Subqueries

You might have noticed that even with a complete query, there are many questions that we can't answer about our data without additional post, or pre, processing. In these cases, you can either make multiple queries and process the data yourself, or you can build a more complex query using SQL subqueries.

```sql
-- Example: General subquery
/*
Lets say your company has a list of all Sales Associates, with data on the revenue that each Associate brings in, and their individual salary. Times are tight, and you now want to find out which of your Associates are costing the company more than the average revenue brought per Associate.

First, you would need to calculate the average revenue all the Associates are generating:
*/
SELECT AVG(revenue_generated) FROM sales_associates;
/*
And then using that result, we can then compare the costs of each of the Associates against that value. To use it as a subquery, we can just write it straight into the `WHERE` clause of the query:
*/
SELECT * FROM sales_associates WHERE salary > (SELECT AVG(revenue_generated) FROM sales_associates);
```

A subquery can be referenced anywhere a normal table can be referenced. Inside a `FROM` clause, you can `JOIN` subqueries with other tables, inside a `WHERE` or `HAVING` constraint, you can test expressions against the results of the subquery, and even in expressions in the `SELECT` clause, which allow you to return data directly from the subquery.

## Correlated subqueries

A more powerful type of subquery is the _correlated subquery_ in which the inner query references, and is dependent on, a column or alias from the outer query. Unlike the subqueries above, each of these inner queries need to be run for each of the rows in the outer query, since the inner query is dependent on the current outer query row.

```sql
/*
Example: Correlated subquery

Instead of the list of just Sales Associates above, imagine if you have a general list of Employees, their departments (engineering, sales, etc.), revenue, and salary. This time, you are now looking across the company to find the employees who perform worse than average in their department.

For each employee, you would need to calculate their cost relative to the average revenue generated by all people in their department. To take the average for the department, the subquery will need to know what department each employee is in:
*/
`SELECT * FROM employees WHERE salary > (SELECT AVG(revenue_generated) FROM employees AS dept_employees **WHERE dept_employees.department = employees.department**);`
```

## Unions, Intersections and Exceptions

When working with multiple tables, the `UNION` and `UNION ALL` operator allows you to append the results of one query to another assuming that they have the same column count, order and data type. If you use the `UNION` without the `ALL`, duplicate rows between the tables will be removed from the result.

```sql
-- Select query with set operators
SELECT column, another_column FROM mytable **UNION / UNION ALL / INTERSECT / EXCEPT SELECT other_column, yet_another_column FROM another_table** ORDER BY column DESC LIMIT _n_;
```

In the order of operations as defined in [Lesson 12: Order of execution](https://sqlbolt.com/lesson/select_queries_order_of_execution "SQL Lesson 12: Order of execution"), the `UNION` happens before the `ORDER BY` and `LIMIT`. It's not common to use `UNION`s, but if you have data in different tables that can't be joined and processed, it can be an alternative to making multiple queries on the database.

Similar to the `UNION`, the `INTERSECT` operator will ensure that only rows that are identical in both result sets are returned, and the `EXCEPT` operator will ensure that only rows in the first result set that aren't in the second are returned. This means that the `EXCEPT` operator is query order-sensitive, like the `LEFT JOIN` and `RIGHT JOIN`.

Both `INTERSECT` and `EXCEPT` also discard duplicate rows after their respective operations, though some databases also support `INTERSECT ALL` and `EXCEPT ALL` to allow duplicates to be retained and returned.