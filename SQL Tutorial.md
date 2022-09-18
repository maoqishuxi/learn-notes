# SQL Tutorial

## SQL Dialects

The following are the most popular dialects of SQL:

- [PL/SQL](http://www.plsqltutorial.com/) stands for procedural language/SQL. It is developed by Oracle for the [Oracle Database](http://www.oracletutorial.com/).
- [Transact-SQL](http://www.sqlservertutorial.net/) or T-SQL is developed by Microsoft for [Microsoft SQL Server](http://www.sqlservertutorial.net/).
- [PL/pgSQL](http://www.postgresqltutorial.com/postgresql-stored-procedures/) stands for Procedural Language/PostgreSQL that consists of SQL dialect and extensions implemented in PostgreSQL
- [MySQL](http://www.mysqltutorial.org/) has its own [procedural language](http://www.mysqltutorial.org/mysql-stored-procedure-tutorial.aspx) since version 5. Note that MySQL was acquired by Oracle.

## SQL SELECT

### Introduction to `SQL SELECT` statement

The SQL `SELECT` statement selects data from one or more tables. The following shows the basic syntax of the `SELECT` statement that selects data from a single table.

```sql
SELECT
	select_list
FROM
	table_name;
```

In this syntax:

- First, specify a list of comma-separated columns from the table in the `SELECT` clause.

- Then, specify the table name in the `FROM` clause.

If you want to query data from all the columns of the table, you can use the asterisk (*) operator instead if specifying all the column names:

```sql
SELECT * FROM table_name;
```

To assign an expression or a column an alias, you specify the `AS` keyword followed by the column alias as follows:

```sql
expression AS column_alias
```

```sql
SELECT 
    first_name, 
    last_name, 
    salary, 
    salary * 1.05 AS new_salary
FROM
    employees;
```

## SQL ORDER BY

### Introduction to SQL `ORDER BY` clause

The `ORDER BY` is an optional clause of the `SELECT` statement. The `ORDER BY` clause allows you to sort the rows returned by the `SELECT` clause by one or more sort expressions in ascending or descending order.

```sql
SELECT 
    select_list
FROM
    table_name
ORDER BY 
    sort_expression [ASC | DESC];
```

## SQL DISTINCT

### Introduction to SQL `DISTINCT` operator

To remove duplicate rows from a result set, you use the `DISTINCT` operator in the `SELECT` clause as follows:

```sql
SELECT DISTINCT
    column1, column2, ...
FROM
    table1;
```

## SQL LIMIT

### Introduction to SQL LIMIT clause

To limit the number of rows returned by a [select](https://www.sqltutorial.org/sql-select/) statement, you use the `LIMIT` and `OFFSET` clauses.

```sql
SELECT 
    column_list
FROM
    table1
ORDER BY column_list
LIMIT row_count OFFSET offset;
```

- The `LIMIT row_count` determines the number of rows (`row_count`) returned by the query.
- The `OFFSET offset` clause skips the `offset` rows before beginning to return the rows.

## SQL FETCH

### Introduction to SQL FETCH clause

To limit the number of rows returned by a query, you use the `LIMIT` clause. The `LIMIT` clause is widely supported by many database systems such as MySQL, H2, and HSQLDB. However, the `LIMIT` clause is not in SQL standard.

SQL:2008 introduced the `OFFSET FETCH` clause which has a similar function to the `LIMIT` clause. The `OFFSET FETCH` clause allows you to skip `N` first rows in a result set before starting to return any rows.

```
OFFSET offset_rows { ROW | ROWS }
FETCH { FIRST | NEXT } [ fetch_rows ] { ROW | ROWS } ONLY
```



- The `ROW` and `ROWS`, `FIRST` and `NEXT` are the synonyms. Therefore, you can use them interchangeably.
- The `offset_rows` is an integer number which must be zero or positive. In case the `offset_rows` is greater than the number of rows in the result set, no rows will be returned.
- The `fetch_rows` is also an integer number that determines the number of rows to be returned. The value of `fetch_rows` is equal to or greater than one.

```
SELECT 
    employee_id, 
    first_name, 
    last_name, 
    salary
FROM employees
ORDER BY 
    salary DESC
OFFSET 0 ROWS
FETCH NEXT 1 ROWS ONLY;
```

## SQL WHERE

### Introduction to SQL `WHERE` clause

To select specific rows from a table, you use a `WHERE` clause in the `SELECT` statement. The following illustrates the syntax of the `WHERE` clause in the `SELECT` statement:

```
SELECT 
    column1, column2, ...
FROM
    table_name
WHERE
    condition;
```

| Operator | Meaning               |
| :------- | :-------------------- |
| =        | Equal to              |
| <> (!=)  | Not equal to          |
| <        | Less than             |
| >        | Greater than          |
| <=       | Less than or equal    |
| >=       | Greater than or equal |

The literal values that you use in an expression can be numbers, characters, dates, and times, depending on the format you use:

- Number: use a number that can be an integer or a decimal without any formatting e.g., 100, 200.5
- Character: use characters surrounded by either single or double quotes e.g., “100”, “John Doe”.
- Date: use the format that the database stores. It depends on the database system e.g., MySQL uses `'yyyy-mm-dd'` format to store the [date](http://www.mysqltutorial.org/mysql-date/) data.
- Time: use the format that the database system uses to store the time. For example, MySQL uses `'HH:MM:SS'` to store time data.

### SQL Comparison Operators

The SQL comparison operators allow you to test if two expressions are the same. The following table illustrates the comparison operators in SQL:

| Operator | Meaning                  |
| :------- | :----------------------- |
| =        | Equal                    |
| <>       | Not equal to             |
| >        | Greater than             |
| >=       | Greater than or equal to |
| <        | Less than                |
| <=       | Less than or equal to    |

## SQL Logical Operators

A logical operator allows you to test for the truth of a condition. Similar to a [comparison operator](https://www.sqltutorial.org/sql-comparison-operators/), a logical operator returns a value of true, false, or unknown.

| **Operator**                                        | **Meaning**                                                  |
| :-------------------------------------------------- | :----------------------------------------------------------- |
| [ALL](https://www.sqltutorial.org/sql-all/)         | Return true if all comparisons are true                      |
| [AND](https://www.sqltutorial.org/sql-and/)         | Return true if both expressions are true                     |
| [ANY](https://www.sqltutorial.org/sql-any/)         | Return true if any one of the comparisons is true.           |
| [BETWEEN](https://www.sqltutorial.org/sql-between/) | Return true if the operand is within a range                 |
| [EXISTS](https://www.sqltutorial.org/sql-exists/)   | Return true if a subquery contains any rows                  |
| [IN](https://www.sqltutorial.org/sql-in/)           | Return true if the operand is equal to one of the value in a list |
| [LIKE](https://www.sqltutorial.org/sql-like/)       | Return true if the operand matches a pattern                 |
| [NOT](https://www.sqltutorial.org/sql-not/)         | Reverse the result of any other Boolean operator.            |
| [OR](https://www.sqltutorial.org/sql-or/)           | Return true if either expression is true                     |
| [SOME](https://www.sqltutorial.org/sql-any/)        | Return true if some of the expressions are true              |

### AND

The `AND` operator allows you to construct multiple conditions in the `WHERE` clause of an SQL statement such as `SELECT`, `UPDATE`, and `DELETE`:

```
expression1 AND expression2
```

```
SELECT 
    first_name, last_name, salary
FROM
    employees
WHERE
    salary > 5000 AND salary < 7000
ORDER BY salary;
```

### OR

Similar to the `AND` operator, the `OR` operator combines multiple conditions in an SQL statement’s `WHERE` clause:

```
expression1 OR expression2
```

```
SELECT 
    first_name, last_name, salary
FROM
    employees
WHERE
    salary = 7000 OR salary = 8000
ORDER BY salary;
```

### IS NULL

The `IS NULL` operator compares a value with a null value and returns true if the compared value is null; otherwise, it returns false.

```
SELECT 
    first_name, last_name, phone_number
FROM
    employees
WHERE
    phone_number IS NULL
ORDER BY first_name , last_name;
```

### BETWEEN

The `BETWEEN` operator searches for values that are within a set of values, given the minimum value and maximum value. Note that the minimum and maximum values are included as part of the conditional set.

```
SELECT 
    first_name, last_name, salary
FROM
    employees
WHERE
    salary BETWEEN 9000 AND 12000
ORDER BY salary;    
```

### IN

The `IN` operator compares a value to a list of specified values. The `IN` operator returns true if the compared value matches at least one value in the list; otherwise, it returns false.

```
SELECT 
    first_name, last_name, department_id
FROM
    employees
WHERE
    department_id IN (8, 9)
ORDER BY department_id;
```

### LIKE

The `LIKE` operator compares a value to similar values using a wildcard operator. SQL provides two wildcards used in conjunction with the `LIKE` operator:

- The percent sign ( `%`) represents zero, one, or multiple characters.
- The underscore sign ( `_`) represents a single character.

```
SELECT 
    employee_id, first_name, last_name
FROM
    employees
WHERE
    first_name LIKE 'jo%'
ORDER BY first_name;
```

### ALL

The `ALL` operator compares a value to all values in another value set. The `ALL` operator must be preceded by a [comparison operator](https://www.sqltutorial.org/sql-comparison-operators/) and followed by a [subquery](https://www.sqltutorial.org/sql-subquery/).

```
SELECT 
    first_name, last_name, salary
FROM
    employees
WHERE
    salary >= ALL (SELECT 
            salary
        FROM
            employees
        WHERE
            department_id = 8)
ORDER BY salary DESC;
```

### ANY

The `ANY` operator compares a value to any value in a set according to the condition as shown below:

```
comparison_operator ANY(subquery)
```

Similar to the ALL operator, the `ANY` operator must be preceded by a [comparison operator](https://www.sqltutorial.org/sql-comparison-operators/) and followed by a [subquery](https://www.sqltutorial.org/sql-subquery/).

```
SELECT 
    first_name, last_name, salary
FROM
    employees
WHERE
    salary > ANY(SELECT 
            AVG(salary)
        FROM
            employees
        GROUP BY department_id)
ORDER BY first_name , last_name; 
```

### EXISTS

```
EXISTS (subquery)
```

If the subquery returns one or more rows, the result of the `EXISTS` is true; otherwise, the result is false.

```
SELECT 
    first_name, last_name
FROM
    employees e
WHERE
    EXISTS( SELECT 
            1
        FROM
            dependents d
        WHERE
            d.employee_id = e.employee_id);
```

### NOT

```
NOT [Boolean_expression]
```

```
SELECT
	employee_id,
	first_name,
	last_name,
	salary
FROM
	employees
WHERE
	department_id = 5
AND NOT salary > 5000
ORDER BY
	salary;
```

```
SELECT
	employee_id,
	first_name,
	last_name,
	department_id
FROM
	employees
WHERE
	department_id NOT IN (1, 2, 3)
ORDER BY
	first_name;
```

## SQL Alias

SQL alias allows you to assign a table or a column a temporary name during the execution of a [query](https://www.sqltutorial.org/sql-select/). SQL has two types of aliases: table and column aliases.

```
column_name AS alias_name
```

In this syntax, you specify the column alias after the `AS` keyword followed by the column name. The `AS` keyword is optional. So you can omit it like this:

```
column_name alias_name
```

```
SELECT
	inv_no AS invoice_no,
	amount,
	due_date AS 'Due date',
	cust_no 'Customer No'
FROM
	invoices;
```

## SQL INNER JOIN

### Introduction to the SQL INNER JOIN clause.

When table A joins with table B using the inner join, you have the result set (3,4) that is the intersection of table A and table B.

![SQL INNER JOIN](https://www.sqltutorial.org/wp-content/uploads/2016/03/SQL-INNER-JOIN.png)

For each row in table A, the inner join clause finds the matching rows in table B. If a row is matched, it is included in the final result set.

```
SELECT a
FROM A
INNER JOIN B ON b = a;
```

The INNER JOIN clause appears after the FROM clause. The condition to match between table A and table B is specified after the ON keyword. This condition is called *join condition* i.e., *`B.n = A.n`*

The INNER JOIN clause can join three or more tables as long as they have relationships, typically foreign key relationships.

```
SELECT
  A.n
FROM A
INNER JOIN B ON B.n = A.n
INNER JOIN C ON C.n = A.n;
```

## SQL LEFT JOIN

### Introduction to SQL LEFT JOIN clause

The left join, however, returns all rows from the left table whether or not there is a matching row in the right table.

When we join table A with table B, all the rows in table A (the left table) are included in the result set whether there is a matching row in the table B or not.

![SQL LEFT JOIN](https://www.sqltutorial.org/wp-content/uploads/2016/03/SQL-LEFT-JOIN.png)

```
SELECT
	A.n
FROM
	A
LEFT JOIN B ON B.n = A.n;
```

```
SELECT
	country_id,
	country_name
FROM
	countries
WHERE
	country_id IN ('US', 'UK', 'CN');
```

## SQL SELF JOIN

### Introduction to SQL self-join

Sometimes, it is useful to join a table to itself. This type of join is known as the self-join.

We join a table to itself to evaluate the rows with other rows in the same table. To perform the self-join, we use either an [inner join](https://www.sqltutorial.org/sql-inner-join/) or [left join](https://www.sqltutorial.org/sql-left-join/) clause.

Because the same table appears twice in a single query, we have to use the table aliases. The following statement illustrates how to join a table to itself.

```
SELECT
	column1,
	column2,
	column3,
        ...
FROM
	table1 A
INNER JOIN table1 B ON B.column1 = A.column2;
```

In this statement joins the table1 to itself using an [INNER JOIN](https://www.sqltutorial.org/sql-inner-join/) clause. A and B are the table aliases of the table1. The `B.column1 = A.column2` is the join condition.

Besides the INNER JOIN clause, you can use the [LEFT JOIN](https://www.sqltutorial.org/sql-left-join/) clause.

## SQL FULL OUTER JOIN

### Introduction to SQL FULL OUTER JOIN clause

In theory, a full outer join is the combination of a [left join](https://www.sqltutorial.org/sql-left-join/) and a right join. The full outer join includes all rows from the joined tables whether or not the other table has the matching row.

If the rows in the joined tables do not match, the result set of the full outer join contains NULL values for every column of the table that lacks a matching row. For the matching rows, a single row that has the columns populated from the joined table is included in the result set.

```
SELECT column_list
FROM A
FULL OUTER JOIN B ON B.n = A.n;
```

## SQL CROSS JOIN

### Introduction to SQL CROSS JOIN clause

A cross join is a join operation that produces the Cartesian product of two or more tables.

In Math, a Cartesian product is a mathematical operation that returns a product set of multiple sets.

For example, with two sets A {x,y,z} and B {1,2,3}, the Cartesian product of A x B is the set of all ordered pairs (x,1), (x,2), (x,3), (y,1) (y,2), (y,3), (z,1), (z,2), (z,3).

The following picture illustrates the Cartesian product of A and B:

![img](https://www.sqltutorial.org/wp-content/uploads/2018/01/SQL-CROSS-JOIN-Cartesian_Product.png)

Similarly, in SQL, a Cartesian product of two tables A and B is a result set in which each row in the first table (A) is paired with each row in the second table (B). Suppose the A table has n rows and the B table has m rows, the result of the cross join of the A and B tables have n x m rows.

```
SELECT column_list
FROM A
CROSS JOIN B;
```

The following picture illustrates the result of the cross join between the table A and table B. In this illustration, the table A has three rows 1, 2 and 3 and the table B also has three rows x, y and z. As the result, the Cartesian product has nine rows:

![SQL CROSS JOIN](https://www.sqltutorial.org/wp-content/uploads/2018/01/SQL-CROSS-JOIN.png)

```
SELECT 
    column_list
FROM
    A,
    B;
```

## SQL GROUP BY

The `GROUP BY` is an optional clause of the `SELECT` statement. The `GROUP BY` clause allows you to group rows based on values of one or more columns. It returns one row for each group.

```
SELECT
	column1,
	column2,
	aggregate_function(column3)
FROM
	table_name
GROUP BY
	column1,
	column2;
```

The following picture illustrates shows how the GROUP BY clause works:

![img](https://www.sqltutorial.org/wp-content/uploads/2022/05/sql-group-by.svg)

## SQL HAVING

### Introduction to SQL HAVING clause

To specify a condition for groups, you use the HAVING clause.

The HAVING clause is often used with the GROUP BY clause in the [SELECT statement](https://www.sqltutorial.org/sql-select/). If you use a HAVING clause without a GROUP BY clause, the HAVING clause behaves like the [WHERE clause](https://www.sqltutorial.org/sql-where/).

```
SELECT
	column1,
	column2,
	AGGREGATE_FUNCTION (column3)
FROM
	table1
GROUP BY
	column1,
	column2
HAVING
	group_condition;
```

### HAVING vs. WHERE

The [WHERE](https://www.sqltutorial.org/sql-where/) clause applies the condition to individual rows before the rows are summarized into groups by the GROUP BY clause. However, the HAVING clause applies the condition to the groups after the rows are grouped into groups.

## SQL GROUPING SETS

### Introduction to SQL `GROUPING SETS`

A grouping set is a set of columns by which you group using the `GROUP BY` clause. Normally, a single aggregate query defines a single grouping set.

The `GROUPING SETS` is an option of the `GROUP BY` clause. The `GROUPING SETS` defines multiple grouping sets within the same query.

```
SELECT
    c1,
    c2,
    aggregate (c3)
FROM
    table
GROUP BY
    GROUPING SETS (
        (c1, c2),
        (c1),
        (c2),
        ()
);
```

## SQL ROLLUP

### Introduction to SQL ROLLUP

The `ROLLUP` is an extension of the `GROUP BY` clause. The `ROLLUP` option allows you to include extra rows that represent the subtotals, which are commonly referred to as super-aggregate rows, along with the grand total row. By using the `ROLLUP` option, you can use a single query to generate multiple [grouping sets](https://www.sqltutorial.org/sql-grouping-sets/).

```
SELECT 
    c1, c2, aggregate_function(c3)
FROM
    table
GROUP BY ROLLUP (c1, c2);
```

## SQL CUBE

### Introduction to SQL CUBE

Similar to the `ROLLUP`, `CUBE` is an extension of the `GROUP BY` clause. `CUBE` allows you to generate subtotals like the `ROLLUP` extension. In addition, the `CUBE` extension will generate subtotals for all combinations of grouping columns specified in the `GROUP BY` clause.

```
SELECT 
    c1, c2, AGGREGATE_FUNCTION(c3)
FROM
    table_name
GROUP BY CUBE(c1 , c2);
```

## SQL Subquery

```
SELECT 
    employee_id, first_name, last_name
FROM
    employees
WHERE
    department_id IN (SELECT 
            department_id
        FROM
            departments
        WHERE
            location_id = 1700)
ORDER BY first_name , last_name;
```

You can use a subquery in many places such as:

- With the `IN` or `NOT IN` operator
- With [comparison operators](https://www.sqltutorial.org/sql-comparison-operators/)
- With the `EXISTS` or `NOT EXISTS` operator
- With the `ANY` or `ALL` operator
- In the `FROM` clause
- In the `SELECT` clause

### SQL subquery with the comparison operator

```
comparison_operator (subquery)
```

where the [comparison operator](https://www.sqltutorial.org/sql-comparison-operators/) is one of these operators:

- Equal (=)
- Greater than (>)
- Less than (<)
- Greater than or equal ( >=)
- Less than or equal (<=)
- Not equal ( !=) or (<>)

## SQL UNION

### Introduction to SQL UNION operator

The UNION operator combines result sets of two or more [SELECT](https://www.sqltutorial.org/sql-select/) statements into a single result set. The following statement illustrates how to use the UNION operator to combine result sets of two queries:

```
SELECT 
    column1, column2
FROM
    table1 
UNION [ALL]
SELECT 
    column3, column4
FROM
    table2;
```

The columns returned by the SELECT statements must have the same or convertible data type, size, and be the same order.

The database system processes the query by executing two SELECT statements first. Then, it combines two individual result sets into one and eliminates duplicate rows. To eliminate the duplicate rows, the database system sorts the combined result set by every column and scans it for the matching rows located next to one another.

To retain the duplicate rows in the result set, you use the UNION ALL operator.

![SQL UNION](https://www.sqltutorial.org/wp-content/uploads/2016/03/SQL-UNION.png)

And the following picture illustrates A UNION ALL B

![SQL UNION ALL](https://www.sqltutorial.org/wp-content/uploads/2016/03/SQL-UNION-ALL.png)

## SQL INTERSECT

### Introduction to SQL INTERSECT operator

The INTERSECT operator is a set operator that returns distinct rows of two or more result sets from [SELECT](https://www.sqltutorial.org/sql-select/) statements.

![SQL-INTERSECT-Operator](https://www.sqltutorial.org/wp-content/uploads/2016/03/SQL-INTERSECT-Operator.png)

Like the [UNION operator](https://www.sqltutorial.org/sql-union/), the INTERSECT operator removes the duplicate rows from the final result set.

```
SELECT
	id
FROM
	a 
INTERSECT
SELECT
	id
FROM
	b;
```

To use the INTERSECT operator, the columns of the SELECT statements must follow the rules:

- The data types of columns must be compatible.
- The number of columns and their orders in the SELECT statements must be the same.

## SQL MINUS

### Introduction to SQL MINUS operator

Besides the `UNION`, `UNION ALL`, and `INTERSECT` operators, SQL provides us with the `MINUS` operator that allows you to subtract one result set from another result set.

```
SELECT
	id
FROM
	A 
MINUS 
SELECT
	id
FROM
	B;
```

![SQL MINUS](https://www.sqltutorial.org/wp-content/uploads/2016/03/SQL-MINUS.png)

In order to use the `MINUS` operator, the columns in the `SELECT` clauses must match in number and must have the same or, at least, convertible data type.

## SQL INSERT

### Introduction to the SQL INSERT statement

SQL provides the `INSERT` statement that allows you to insert one or more rows into a table. The `INSERT` statement allows you to:

1. Insert a single row into a table
2. Insert multiple rows into a table
3. Copy rows from a table to another table.

### Insert one row into a table

To insert one row into a table, you use the following syntax of the `INSERT` statement.

```
INSERT INTO table1 (column1, column2,...)
VALUES
	(value1, value2,...);
```

There are some points that you should pay attention to when you insert a new row into a table:

- First, the number of values must be the same as the number of columns. In addition, the columns and values must be the correspondent because the database system will match them by their relative positions in the lists.
- Second, before adding a new row, the database system checks for all integrity constraints e.g., [foreign key constraint](https://www.sqltutorial.org/sql-foreign-key/), [primary key constraint](https://www.sqltutorial.org/sql-primary-key/), [check constraint](https://www.sqltutorial.org/sql-check-constraint/) and [not null constraint](https://www.sqltutorial.org/sql-not-null-constraint/). If one of these constraints is violated, the database system will issue an error and terminate the statement without inserting any new row into the table.

It is not necessary to specify the columns if the sequence of values matches the order of the columns in the table. See the following `INSERT` statement that omits the column list in the `INSERT INTO` clause.

```
INSERT INTO table1
VALUES
	(value1, value2,...);
```

If you don’t specify a column and its value in the `INSERT` statement when you insert a new row, that column will take a default value specified in the table structure. The default value could be 0, a next integer value in a sequence, the current time, a NULL value, etc. See the following statement:

```
INSERT INTO (column1, column3)
VALUES
	(column1, column3);
```

## SQL UPDATE

### Introduction to the SQL UPDATE statement

```
UPDATE table_name
SET column1 = value1,
 column2 = value2
WHERE
	condition;
```

In this syntax:

- First, indicate the table that you want to update in the `UPDATE` clause.
- Second, specify the columns that you want to modify in the `SET` clause. The columns that are not listed in the `SET` clause will retain their original values.
- Third, specify which rows to update in the `WHERE` clause.

The `UPDATE` statement affects one or more rows in a table based on the condition in the `WHERE` clause. For example, if the `WHERE` clause contains a [primary key](https://www.sqltutorial.org/sql-primary-key/) expression, the `UPDATE` statement changes one row only.

## SQL DELETE

### Introduction to SQL DELETE statement

To remove one or more rows from a table, you use the `DELETE` statement. The general syntax for the `DELETE` statement is as follows:

```
DELETE
FROM
	table_name
WHERE
	condition;
```

First, provide the name of the table where you want to remove rows.

Second, specify the condition in the `WHERE` clause to identify the rows that need to be deleted. If you omit the `WHERE` clause all rows in the table will be deleted. Therefore, you should always use the `DELETE` statement with caution.

### SQL DELETE multiple rows example

```
DELETE FROM dependents 
WHERE
    employee_id IN (100 , 101, 102);
```

## SQL CASE

### Introduction to SQL CASE expression

The SQL CASE expression allows you to evaluate a list of conditions and returns one of the possible results. The CASE expression has two formats: simple CASE and searched CASE.

You can use the CASE expression in a clause or statement that allows a valid expression. For example, you can use the CASE expression in statements such as [SELECT](https://www.sqltutorial.org/sql-select/), [DELETE](https://www.sqltutorial.org/sql-delete/), and [UPDATE ](https://www.sqltutorial.org/sql-update/)or in clauses such as [SELECT](https://www.sqltutorial.org/sql-select/), [ORDER BY](https://www.sqltutorial.org/sql-order-by/), and [HAVING](https://www.sqltutorial.org/sql-having/).

### Simple CASE expression

```
CASE expression
WHEN when_expression_1 THEN
	result_1
WHEN when_expression_2 THEN
	result_2
WHEN when_expression_3 THEN
	result_3
...
ELSE
	else_result
END
```

```
SELECT 
    first_name,
    last_name,
    hire_date,
    CASE (2000 - YEAR(hire_date))
        WHEN 1 THEN '1 year'
        WHEN 3 THEN '3 years'
        WHEN 5 THEN '5 years'
        WHEN 10 THEN '10 years'
        WHEN 15 THEN '15 years'
        WHEN 20 THEN '20 years'
        WHEN 25 THEN '25 years'
        WHEN 30 THEN '30 years'
    END aniversary
FROM
    employees
ORDER BY first_name;
```

### Searched CASE expression

```
CASE
WHEN boolean_expression_1 THEN
	result_1
WHEN boolean_expression_2 THEN
	result_2
WHEN boolean_expression_3 THEN
	result_3
ELSE
	else_result
END;
```

```
SELECT 
    first_name,
    last_name,
    CASE
        WHEN salary < 3000 THEN 'Low'
        WHEN salary >= 3000 AND salary <= 5000 THEN 'Average'
        WHEN salary > 5000 THEN 'High'
    END evaluation
FROM
    employees;
```

## SQL Data Types

### Character string data type

The character string data type represents the character data type including fixed-length and varying-length character types.

### Numeric Types

Numeric values are stored in the columns with the type of numbers, typically referred to as `NUMBER`, `INTEGER`, `REAL`, and `DECIMAL`.

The following are the SQL numeric data types:

- BIT(n)
- BIT VARYING (n)
- DECIMAL (p,s)
- INTEGER
- SMALLINT
- BIGINT
- FLOAT(p,s)
- DOUBLE PRECISION (p,s)
- REAL(s)

### Decimal types

The `DECIMAL` data type is used to store exact numeric values in the database e.g., money values.

```
column_name DECIMAL (p,s)
```

In this syntax:

- p is the precision that represents the number of significant digits.
- s is the scale which represents the number of digits after the decimal point.

### Integer

Integer data type stores whole numbers, both positive and negative. The examples of integers are 10, 0, -10, and 2010.

### Floating-point data types

The floating-point data types represent approximate numeric values. The precision and scale of the floating point decimals are variable in lengths and virtually without limit.

### Date and Time types

The date and time data types are used to store information related to dates and times. SQL supports the following date and time data types:

- DATE
- TIME
- TIMESTAMP

#### DATE data type

The `DATE` data type represents date values that include three parts: year, month, and day. Typically, the range of the `DATE` data type is from `0001-01-01` to `9999-12-31`.

The date value generally is specified in the form:

```
'YYYY-DD-MM'
```

#### TIME data type

The `TIME` data type store values representing a time of day in hours, minutes, and seconds.

The `TIME` values should be specified in the following form:

```
'HH:MM:SS'
```

### TIMESTAMP data type

The `TIMESTAMP` data type represents timestamp values which include both `DATE` and `TIME` values.

The `TIMESTAMP` values are specified in the following form:

```
TIMESTAMP 'YYYY-MM-DD HH:MM:SS'
```

## SQL CREATE TABLE

### Introduction to SQL CREATE TABLE statement

A table is a collection of data stored in a database. A table consists of columns and rows. To create a new table, you use the `CREATE TABLE` statement with the following syntax:

```
CREATE TABLE table_name(
     column_name_1 data_type default value column_constraint,
     column_name_2 data_type default value column_constraint,
     ...,
     table_constraint
);
```

In the `CREATE TABLE` statement, you specify a comma-separated list of column definitions. Each column definition is composed of a column name, column’s [data type](https://www.sqltutorial.org/sql-data-types/), a default value, and one or more column constraints.

```
CREATE TABLE trainings (
    employee_id INT,
    course_id INT,
    taken_date DATE,
    PRIMARY KEY (employee_id , course_id)
);
```

## SQL Identity

### Introduction to SQL identity column

SQL identity column is a column whose values are automatically generated when you [add a new row to the table](https://www.sqltutorial.org/sql-insert/). To define an identity column, you use the `GENERATED AS IDENTITY` property as follows:

```
column_name data_type GENERATED { ALWAYS | BY DEFAULT } AS IDENTITY[ ( sequence_option ) ]
```

In this syntax:

- The `data_type` can be any integer [data type](https://www.sqltutorial.org/sql-data-types/).
- The `GENERATED ALWAYS` generates sequential integers for the identity column. If you attempt to [insert](https://www.sqltutorial.org/sql-insert/) (or [update](https://www.sqltutorial.org/sql-update/)) a value into the `GENERATED ALWAYS AS IDENTITY` column, the database system will raise an error.
- The `GENERATED BY DEFAULT` generates sequential integers for the identity column. However, if you provide a value for insert or update, the database system will use that value for insert instead of using the auto-generated value.

```
CREATE TABLE ranks (
    rank_id INT GENERATED ALWAYS AS IDENTITY,
    rank_name CHAR
);
```

## SQL Auto Increment

### SQL auto increment column in MySQL

MySQL uses `AUTO_INCREMENT` property to define an auto-increment column. See the following example:

```
CREATE TABLE leave_requests (
    request_id INT AUTO_INCREMENT,
    employee_id INT NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    leave_type INT NOT NULL,
    PRIMARY KEY(request_id)
);
```

### SQL auto increment column in Oracle

Oracle uses the [identity column](http://www.oracletutorial.com/oracle-basics/oracle-identity-column/) for creating an auto increment column as follows:

```
CREATE TABLE leave_requests (
    request_id NUMBER GENERATED BY DEFAULT AS IDENTITY,
    employee_id INT NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    leave_type INT NOT NULL,
    PRIMARY KEY(request_id)
);
```

### SQL auto increment column in PostgreSQL

Similar to Oracle, PostgreSQL also uses the [identity column](http://www.postgresqltutorial.com/postgresql-identity-column/) for defining auto increment columns:

```
CREATE TABLE leave_requests (
    request_id INT GENERATED BY DEFAULT AS IDENTITY,
    employee_id INT NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    leave_type INT NOT NULL,
    PRIMARY KEY(request_id)
);
```

### SQL auto increment column in SQL Server

SQL Server uses `IDENTITY` property to define an auto increment column as shown in the following query:

```
CREATE TABLE leave_requests (
    request_id INT IDENTITY(1,1),
    employee_id INT NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    leave_type INT NOT NULL,
    PRIMARY KEY(request_id)
);
```

### SQL auto increment column in DB2

Like Oracle, DB2 uses the identity column for defining the auto-increment column:

```
CREATE TABLE leave_requests (
    request_id INT GENERATED BY DEFAULT AS IDENTITY,
    employee_id INT NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    leave_type INT NOT NULL,
    PRIMARY KEY(request_id)
);
```

## SQL ALTER TABLE

Once you [create a new table](https://www.sqltutorial.org/sql-create-table/), you may want to change its structure because business requirements change. To modify the structure of a table, you use the `ALTER TABLE` statement. The `ALTER TABLE` statement allows you to perform the following operations on an existing table:

- Add a new column using the `ADD` clause.
- Modify attribute of a column such as constraint, default value, etc. using the `MODIFY` clause.
- Remove columns using the `DROP` clause.

### SQL ALTER TABLE ADD column

The following statement illustrates the `ALTER TABLE` with the `ADD` clause that allows you to add one or more columns to a table.

```
ALTER TABLE table_name
ADD new_colum data_type column_constraint [AFTER existing_column];
```

To add one or more columns to a table, you need to perform the following steps:

- First, specify the table that you want to add column denoted by the `table_name` after the `ALTER TABLE` clause.
- Second, place the new column definition after the `ADD` clause. If you want to specify the order of the new column in the table, you can use the optional clause `AFTER existing_column`.

You can add multiple columns to a table using a single `ALTER TABLE` statement. For example, The following statement adds the `fee` and `max_limit` columns to the `courses` table and places these columns after the `course_name` column.

```
ALTER TABLE courses 
ADD fee NUMERIC (10, 2) AFTER course_name,
ADD max_limit INT AFTER course_name;
```

### SQL ALTER TABLE MODIFY column

The `MODIFY` clause allows you to change some attributes of the existing column e.g., `NOT NULL` ,`UNIQUE`, and [data type](https://www.sqltutorial.org/sql-data-types/).

```
ALTER TABLE table_name
MODIFY column_definition;
```

Notice that you should modify the attributes of columns of a table that has no data. Because changing the attributes of a column in a table that already has data may result in permanent data loss.

```
ALTER TABLE courses 
MODIFY fee NUMERIC (10, 2) NOT NULL;
```

### SQL ALTER TABLE DROP columns

When a column of a table is obsolete and not used by any other database objects such as [triggers](https://www.sqltutorial.org/sql-triggers/), [views](https://www.sqltutorial.org/sql-views/), stored and stored procedures, you need to remove it from the table.

To remove one or more columns, you use the following syntax:

```
ALTER TABLE table_name
DROP column_name,
DROP colum_name,
...
```

To remove more than one column at the same time, you use multiple `DROP COLUMN` clauses separated by a comma (,).

```
ALTER TABLE courses 
DROP COLUMN max_limit,
DROP COLUMN credit_hours;
```

## SQL ADD COLUMN

```
ALTER TABLE table_name
ADD [COLUMN] column_definition;
```

In this statement,

- First, specify the table to which you want to add the new column.
- Second, specify the column definition after the `ADD COLUMN` clause.

```
ALTER TABLE candidates
ADD COLUMN home_address VARCHAR(255),
ADD COLUMN dob DATE,
ADD COLUMN linkedin_account VARCHAR(255);
```

### PostgreSQL

[Add one column to a table in PostgreSQL](http://www.postgresqltutorial.com/postgresql-add-column/):

```
ALTER TABLE table_name
ADD COLUMN column_definition;
```

Add multiple columns to a table in PostgreSQL:

```
    ALTER TABLE table_name
    ADD COLUMN column_definition,
    ADD COLUMN column_definition,
    ...
    ADD COLUMN column_definition;
```

### MySQL

[Add one column to a table in MySQL](http://www.mysqltutorial.org/mysql-add-column/):

```
    ALTER TABLE table_name
    ADD [COLUMN] column_definition;
```

Add multiple columns to a table in MySQL:

```
    ALTER TABLE table_name
    ADD [COLUMN] column_definition,
    ADD [COLUMN] column_definition,
    ...
    ADD [COLUMN] column_definition;
```

### Oracle

[Add one column to a table in Oracle](http://www.oracletutorial.com/oracle-basics/oracle-alter-table-add-column/):

```
ALTER TABLE table_name
ADD column_definition;
```

Add multiple columns to a table in Oracle:

```
ALTER TABLE table_name 
ADD (
    column_definition,
    column_definition,
    ...
);
```

### SQL Server

[Add one column to a table in SQL Server](http://www.sqlservertutorial.net/sql-server-basics/sql-server-alter-table-add-column/):

```
ALTER TABLE table_name
ADD column_definition;
```

Add multiple columns to a table in SQL Server:

```
ALTER TABLE table_name
ADD
    column_definition,
    column_definition,
    ...;
```

### SQLite

Add one column to a table in SQLite:

```
ALTER TABLE table_name
ADD COLUMN column_definition;
```

SQLite does not support adding multiple columns to a table using a single statement. To add multiple columns to a table, you must execute multiple `ALTER TABLE ADD COLUMN` statements.

### DB2

Add one column to a table in DB2

```
ALTER TABLE table_name
ADD column_definition;
```

Add multiple columns to a table in DB2:

```
ALTER TABLE table_name
ADD
    column_definition
    column_definition
    ...;
```

## SQL DROP COLUMN

### Introduction to SQL `DROP COLUMN` statement

```
ALTER TABLE table_name
DROP COLUMN column_name1,
[DROP COLUMN column_name2];
```

In this syntax:

- `table_name` is the name of the table which contains the columns that you are removing.
- `column_name1`, `column_name2` are the columns that you are dropping.

## SQL DROP TABLE

### Introduction to SQL DROP TABLE statement

As the database evolves, we will need to remove the obsolete and redundant tables from the database. To delete a table, we use the `DROP TABLE` statement.

```
DROP TABLE [IF EXISTS] table_name;
```

## SQL TRUNCATE TABLE

### Introduction to the SQL TRUNCATE TABLE statement

To delete all data from a table, you use the `DELETE` statement without a `WHERE` clause. For a big table that has few million rows, the `DELETE` statement is slow and not efficient.

To delete all rows from a big table fast, you use the following `TRUNCATE TABLE` statement:

```
TRUNCATE TABLE table_name;
```

### SQL TRUNCATE TABLE vs. DELETE

Logically the `TRUNCATE TABLE` statement and the `DELETE` statement without the `WHERE` clause gives the same effect that removes all data from a table. However, they do have some differences:

- When you use the `DELETE` statement, the database system logs the operations. And with some efforts, you can roll back the data that was deleted. However, when you use the `TRUNCATE TABLE` statement, you have no chance to roll back except you use it in a transaction that has not been committed.
- To delete data from a table referenced by a foreign key constraint, you cannot use the `TRUNCATE TABLE` statement. In this case, you must use the `DELETE` statement instead.
- The `TRUNCATE TABLE` statement does not fire the delete trigger if the table has the triggers associated with it.
- Some database systems reset the value of an auto-increment column (or identity, sequence, etc.) to its starting value after you execute the `TRUNCATE TABLE` statement. It is not the case for the `DELETE` statement.
- The `DELETE` statement with a `WHERE` clause deletes partial data from a table while the `TRUNCATE TABLE` statement always removes all data from the table.

## SQL Primary Key

A table consists of columns and rows. Typically, a table has a column or set of columns whose values uniquely identify each row in the table. This column or the set of columns is called the **primary key**.

The primary key that consists of two or more columns is also known as the **composite primary key**.

### Creating table with primary key

Generally, you define the primary key when creating the table. If the primary key consists of one column, you can use the PRIMARY KEY constraint as a column or table constraint. If the primary key consists of two or more columns, you must use the PRIMARY KEY constraint as the table constraint.

```
CREATE TABLE projects (
    project_id INT PRIMARY KEY,
    project_name VARCHAR(255),
    start_date DATE NOT NULL,
    end_date DATE NOT NULL
);
```

### Adding the primary key with ALTER TABLE statement

```
ALTER TABLE project_milestones
ADD CONSTRAINT pk_milestone_id PRIMARY KEY (milestone_id);
```

### Removing the primary key constraint

```
ALTER TABLE table_name
DROP CONSTRAINT primary_key_constraint;
```

## SQL Foreign Key Constraint

### Introduction to SQL foreign key constraint

A foreign key is a column or a group of columns that enforces a link between the data in two tables. In a foreign key reference, the [primary key](https://www.sqltutorial.org/sql-primary-key/) column (or columns) of the first table is referenced by the column (or columns) of the second table. The column (or columns) of the second table becomes the foreign key.

```
CREATE TABLE project_milestones (
    milestone_id INT AUTO_INCREMENT PRIMARY KEY,
    project_id INT,
    milestone_name VARCHAR(100),
    FOREIGN KEY (project_id)
        REFERENCES projects (project_id)
);
```

The FOREIGN KEY clause promotes the project_id of the project_milestones table to become the foreign key that is referenced to the project_id of the projects table.

```
FOREIGN KEY (project_id)
        REFERENCES projects (project_id)
```

### Adding FOREIGN KEY contraints to existing tables

To add a FOREIGN KEY constraint to existing table, you use the [ALTER TABLE](https://www.sqltutorial.org/sql-alter-table/) statement.

```
ALTER TABLE table_1
ADD CONSTRAINT fk_name FOREIGN KEY (fk_key_column)
   REFERENCES table_2(pk_key_column)
```

### Removing foreign key constraints

To remove a foreign key constraint, you also use the ALTER TABLE statement as follows:

```
ALTER TABLE table_name
DROP CONSTRAINT fk_name;
```

## SQL UNIQUE Constraint

By definition, an SQL UNIQUE constraint defines a rule that prevents duplicate values stored in specific columns that do not participate a primary key.

### UNIQUE vs. PRIMARY KEY constraints

You can have at most one [PRIMARY KEY constraint](https://www.sqltutorial.org/sql-primary-key/) whereas you can have multiple UNIQUE constraints in a table. In case you have multiple UNIQUE constraints in a table, all UNIQUE constraints must have a different set of columns.

Different from the PRIMARY KEY constraint, the UNIQUE constraint allows [NULL](https://www.sqltutorial.org/sql-is-null/) values. It depends on the RDBMS to consider NULL values are unique or not.

Not Allowed

|                           | **PRIMARY KEY constraint** | **UNIQUE constraint** |
| ------------------------- | -------------------------- | --------------------- |
| The number of constraints | One                        | Many                  |
| NULL values               | Do not allow               | Allow                 |

### Creating UNIQUE constraints

```
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL
);
```

### Adding UNIQUE constraints to existing table

In case the table already exists, you can add a UNIQUE constraint for columns with the prerequisite that the column or the combination of columns which participates in the UNIQUE constraint must contain unique values.

```
ALTER TABLE users
ADD CONSTRAINT uc_username UNIQUE(username);
```

If you want to add a new column and create a UNIQUE constraint for it, you use the following form of the ALTER TABLE statement.

```
ALTER TABLE users
ADD new_column data_type UNIQUE;
```

### Removing UNIQUE constraint

To remove a UNIQUE constraint, you use the ALTER TABLE statement as follows:

```
ALTER TABLE table_name
DROP CONSTRAINT unique_constraint_name;
```

## SQL CHECK Constraint

### Introduction to SQL CHECK constraint

A CHECK constraint is an integrity constraint in SQL that allows you to specify that a value in a column or set of columns must satisfy a Boolean expression.

You can define a CHECK constraint on a single column or the whole table. If you define the CHECK constraint on a single column, the CHECK constraint checks value for this column only. However, if you define a CHECK constraint on a table, it limits value in a column based on values in other columns of the same row.

```
CHECK(Boolean_expression)
```

To assign a CHECK constraint a name, you use the following syntax:

```
CONSTRAINT constraint_name CHECK(Boolean_expression)
```

```
CREATE TABLE products (
	product_id INT PRIMARY KEY,
	product_name VARCHAR (255) NOT NULL,
	selling_price NUMERIC (10, 2) CHECK (selling_price > 0),
	cost NUMERIC (10, 2) CHECK (cost > 0),
	CHECK (selling_price > cost)
);
```

## SQL NOT NULL Constraint

### Introduction to SQL NOT NULL constraint

The NOT NULL constraint is a column constraint that defines the rule which constrains a column to have non-NULL values only.

```
CREATE TABLE table_name(
   ...
   column_name data_type NOT NULL,
   ...
);
```

Logically, an NOT NULL constraint is equivalent to a CHECK constraint, therefore, the above statement is equivalent to the following statement.

```
CREATE TABLE table_name ( 
   ...
   column_name data_type,
   ...
   CHECK (column_name IS NOT NULL)
);
```

### ALTER TABLE NOT NULL statement

First, update all current NULL values to non-NULL values using the [UPDATE](https://www.sqltutorial.org/sql-update/) statement.

```
UPDATE table_name
SET column_name = 0
WHERE
	column_name IS NULL;
```

Second, add the NOT NULL constraint to the column using the [ALTER TABLE](https://www.sqltutorial.org/sql-alter-table/) statement

```
ALTER TABLE table_name
MODIFY column_name data_type NOT NULL;
```

## SQL Views

### Introduction to the SQL Views

SQL provides you with another way to see the data is by using the views.  A view is like a virtual table produced by executing a query. The relational database management system (RDBMS) stores a view as a named `SELECT` in the database catalog.

Whenever you issue a `SELECT` statement that contains a view name, the RDBMS executes the view-defining query to create the virtual table. That virtual table then is used as the source table of the query.

Views allow you to store complex queries in the database. For example, instead of issuing a complex SQL query each time you want to see the data, you just need to issue a simple query as follows:

```
SELECT column_list
FROM view_name;
```

Views help maintain database security. Rather than give the users access to database tables, you create a view to revealing only necessary data and grant the users to access to the view.

### Creating SQL views

To create a view, you use the `CREATE VIEW` statement as follows:

```
CREATE VIEW view_name 
AS
SELECT-statement
```

```
CREATE VIEW employee_contacts AS
    SELECT 
        first_name, last_name, email, phone_number, department_name
    FROM
        employees e
            INNER JOIN
        departments d ON d.department_id = e.department_id
    ORDER BY first_name;
```

By default, the names of columns of the view are the same as column specified in the `SELECT` statement. If you want to rename the columns in the view, you include the new column names after the `CREATE VIEW` clause as follows:

```
CREATE VIEW view_name(new_column_list) 
AS
SELECT-statement;
```

```
CREATE VIEW payroll (first_name , last_name , job, compensation) AS
    SELECT 
        first_name, last_name, job_title, salary
    FROM
        employees e
            INNER JOIN
        jobs j ON j.job_id= e.job_id
    ORDER BY first_name;
```

### Querying data from views

```
SELECT 
    *
FROM
    employee_contacts;
```

### Modifying SQL views

To modify a view, either adding new columns to the view or removing columns from a view, you use the same `CREATE OR REPLACE VIEW` statement.

```
CREATE OR REPLACE view_name AS
SELECT-statement;
```

```
CREATE OR REPLACE VIEW payroll (first_name , last_name , job , department , salary) AS
    SELECT 
        first_name, last_name, job_title, department_name, salary
    FROM
        employees e
            INNER JOIN
        jobs j ON j.job_id = e.job_id
            INNER JOIN
        departments d ON d.department_id = e.department_id
    ORDER BY first_name;
```

### Removing SQL views

To remove a view from the database, you use the `DROP VIEW` statement:

```
DROP VIEW view_name;
```

```
DROP VIEW payroll;
```

## SQL Triggers

### Introduction to SQL Triggers

A trigger is a piece of code executed automatically in response to a specific event occurred on a table in the database.

A trigger is always associated with a particular table. If the [table is deleted](https://www.sqltutorial.org/sql-drop-table/), all the associated triggers are also deleted automatically.

A trigger is invoked either before or after the following event:

- [INSERT](https://www.sqltutorial.org/sql-insert/) – when a new row is inserted
- [UPDATE](https://www.sqltutorial.org/sql-update/) – when an existing row is updated
- [DELETE](https://www.sqltutorial.org/sql-delete/) – when a row is deleted.

### Trigger creation statement syntax

To create a trigger, you use the following statement:

```
CREATE TRIGGER trigger_name [BEFORE|AFTER] event
ON table_name trigger_type
BEGIN
  -- trigger_logic
END;
```

Let’s examine the syntax in more detail:

- First, specify the name of the trigger after the `CREATE TRIGGER` clause.
- Next, use either `BEFORE` or `AFTER` keyword to determine when to the trigger should occur in response to a specific event e.g., `INSERT`, `UPDATE`, or `DELETE`.
- Then, specify the name of the table to which the trigger binds.
- After, specify the type of trigger using either `FOR EACH ROW` or `FOR EACH STATEMENT`. We will discuss more on this in the next section.
- Finally, put the logic of the trigger in the `BEGIN ... END` block.

Besides using the code in the `BEGIN END` block, you can execute a stored procedure as follows:

```
CREATE TRIGGER trigger_name 
[BEFORE|AFTER] event
ON table_name trigger_type
EXECUTE stored_procedure_name;
```

### Row level trigger vs. statement level trigger

There are two types of triggers: row and statement level triggers.

A row level trigger executes each time a row is affected by an `UPDATE` statement. If the `UPDATE` statement affects 10 rows, the row level trigger would execute 10 times, each time per row. If the `UPDATE` statement does not affect any row, the row level trigger is not executed at all.

Different from the row level trigger, a statement level trigger is called ***once*** regardless of how many rows affect by the `UPDATE` statement. Note that if the `UPDATE` statement did not affect any rows, the trigger will still be executed.

When creating a trigger, you can specify whether a trigger is row or statement level by using the `FOR EACH ROW` or `FOR EACH STATEMENT`respectively.

### SQL trigger usages

You typically use the triggers in the following scenarios:

- Log table modifications. Some tables have sensitive data such as customer email, employee salary, etc., that you want to log all the changes. In this case, you can create the `UPDATE` trigger to insert the changes into a separate table.
- Enforce complex integrity of data. In this scenario, you may define triggers to validate the data and reformat the data if necessary. For example, you can transform the data before insert or update using a `BEFORE INSERT` or `BEFORE UPDATE` trigger.

```
CREATE TRIGGER before_update_salary
BEFORE UPDATE ON employees
FOR EACH ROW
BEGIN
   IF NEW.salary <> OLD.salary THEN
	INSERT INTO salary_changes(employee_id,old_salary,new_salary)
        VALUES(NEW.employee_id,OLD.salary,NEW.salary);
    END IF;
END;
```

### Modify triggers

To change the trigger definition, you use the  `CREATE OR REPLACE TRIGGER` statement.

Basically, the `CREATE OR REPLACE TRIGGER` creates a new trigger if it does not exist and changes the trigger if it does exist.

```
CREATE OR REPLACE TRIGGER trigger_name 
[BEFORE|AFTER] event
ON table_name trigger_type
BEGIN
  -- trigger_logic
END;
```

### Delete triggers

To delete a trigger, you use the `DROP TRIGGER` statement as follows:

```
DROP TRIGGER [IF EXISTS] trigger_name;
```

```
DROP TRIGGER IF EXISTS before_update_salary;
```

## SQL Aggregate Functions

An SQL aggregate function calculates on a set of values and returns a single value. For example, the average function ( `AVG`) takes a list of values and returns the average.

Because an aggregate function operates on a set of values, it is often used with the `GROUP BY` clause of the `SELECT` statement. The `GROUP BY` clause divides the result set into groups of values and the aggregate function returns a single value for each group.

```
SELECT c1, aggregate_function(c2)
FROM table
GROUP BY c1;
```

The following are the commonly used SQL aggregate functions:

-  `AVG()` – returns the average of a set.
-  [`COUNT()`](https://www.sqltutorial.org/sql-aggregate-functions/sql-count/) – returns the number of items in a set.
-  [`MAX()`](https://www.sqltutorial.org/sql-max/) – returns the maximum value in a set.
-  [`MIN()`](https://www.sqltutorial.org/sql-aggregate-functions/sql-min/) – returns the minimum value in a set
-  [`SUM()`](https://www.sqltutorial.org/sql-aggregate-functions/sql-sum/) – returns the sum of all or distinct values in a set

###  `AVG`

The `AVG()` function returns the average values in a set. The following illustrates the syntax of the `AVG()` function:

```
AVG( ALL | DISTINCT)
```

###  `MIN`

The `MIN()` function returns the minimum value of a set. The following illustrates the syntax of the `MIN()` function:

```
MIN(column | expression)
```

###  `MAX`

The `MAX()` function returns the maximum value of a set. The `MAX()` function has the following syntax:

```
MAX(column | expression)
```

###  `COUNT`

The `COUNT()` function returns the number of items in a set. The following shows the syntax of the `COUNT()` function:

```
COUNT ( [ALL | DISTINCT] column | expression | *)
```

###  `SUM`

The `SUM()` function returns the sum of all values. The following illustrates the syntax of the `SUM()` function:

```
SUM(ALL | DISTINCT column)
```

### Notes

- The `ALL` keyword will include the duplicate values in the result.
- The `DISTINCT` keyword counts only unique values.
