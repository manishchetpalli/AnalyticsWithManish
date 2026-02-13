# SQL

## **What is a database?**

Database is structured, organised set of data.Think of it as a filecabinet whre you store data in different sections called tables.

---



---

## **What is DBMS?**

A software which allows users to interact with data. Stores data in structured format. A schema defines the structure of data.

---

## **Acid Properties**

- Atomicity:
Each transaction is either properly carried out or the database reverts back to the state before the transaction has started.Atomicity enforces the "all or nothing" principle, ensuring that an entire transaction either completes successfully or is rolled back, preventing partial, inconsistent states. In a financial transfer, if a crash occurs after deducting funds from one account but before adding them to another, an atomic system would detect the failure before the final COMMIT and roll back the entire transaction.

- Consistency:
Database must be in a consistent state before and after the transaction.Consistency guarantees that a transaction moves the database from one valid state to another, enforcing rules (like checking for sufficient funds) to prevent inconsistent results.

- Isolation:
Multiple transactions occur independently without interference.Isolation is the "no interference policy," meaning multiple concurrent transactions operate independently without seeing each other's uncommitted changes; Delta achieves this using. Optimistic Concurrency Control (OCC).

- Durability:
Successful transactions are persisted even in case of failure.Durability ensures that once a transaction is committed, it permanently remains recorded in the system, even in the event of crashes or power outages. This is achieved by logging the start of the operation and the final COMMIT status in a transaction log (ledger); if the system restarts and the log shows no COMMIT, the changes are rolled back

---

## **The Basics of a Query**

> --- ***SELECT and FROM***

A query is a way to ask the database a question to retrieve data without modifying the table structure or its contents.

- SELECT: Specifies which columns you want to retrieve. Use a star () to select all columns.
- FROM: Specifies the table where the data is located.

!!! Example
    `SELECT  FROM customers;`
    retrieves every column and row from the "customers" table.

> --- ***Selecting Specific Columns and Using Aliases***

Instead of retrieving everything, you can specify a list of columns separated by commas.

- Syntax: `SELECT column1, column2 FROM table_name;`.
!!! Note
    Do not place a comma after the last column in your list, or SQL will return an error.
    Aliases (AS): You can temporarily rename a column in your result set for better readability using the `AS` keyword.

!!! Example
    `SELECT country AS customer_country, SUM(score) AS total_score FROM customers GROUP BY country;`.

> --- ***Filtering Data: WHERE***

The `WHERE` clause filters rows based on a specific condition. Only data that meets the condition is included in the output.

- Syntax: `SELECT  FROM customers WHERE score != 0;` (Retrieves customers whose score is not zero).
- Handling Strings: If your condition involves text (characters), the value must be enclosed in single quotes.
!!! Example
    `SELECT  FROM customers WHERE country = 'Germany';`.

> --- ***Sorting Data: ORDER BY***

Use `ORDER BY` to sort your results in a specific order.

- Mechanisms: Use `ASC` for ascending (lowest to highest, default) or `DESC` for descending (highest to lowest).
- Nested Sorting: You can sort by multiple columns. SQL prioritizes the first column listed; the second column is used to "refine" the sorting if there are duplicate values in the first column.
!!! Example
    `SELECT  FROM customers ORDER BY country ASC, score DESC;` sorts by country alphabetically, then by highest score within each country.

> --- ***Aggregation and Grouping: GROUP BY***

The `GROUP BY` clause combines rows with the same values into summary rows. It is typically used with aggregate functions like `SUM`, `COUNT`, or `AVG`.

- Rule: Any non-aggregated column in your `SELECT` statement must also be included in the `GROUP BY` clause, or you will receive an error.
!!! Example
    `SELECT country, SUM(score) FROM customers GROUP BY country;` provides the total score for each unique country.

> --- ***Filtering Aggregated Data: HAVING***

`HAVING` is used to filter data after it has been aggregated by a `GROUP BY` clause.

- Difference from WHERE: `WHERE` filters individual rows before they are grouped; `HAVING` filters the results after the grouping logic is applied.
!!! Example
    To find countries with an average score greater than 430: `SELECT country, AVG(score) FROM customers GROUP BY country HAVING AVG(score) > 430;`.

> --- ***Distinct and Top Keywords***

- DISTINCT: Placed immediately after `SELECT`, it removes duplicate rows so that each value in the result is unique. Note that it is an "expensive" operation for the database, so it should only be used when necessary.
- TOP (or LIMIT): Restricts the number of rows returned based on their row number in the result set.
!!! Example
    `SELECT TOP 3  FROM customers ORDER BY score DESC;` retrieves the three customers with the highest scores.

> --- ***Coding Order vs. Execution Order***

The order in which you write a query is different from the order in which the database executes it. Understanding this helps in building correct queries.

| Step | Coding Order (Syntax) | Execution Order (Logic) |
| :--- | :--- | :--- |
| 1 | `SELECT` (with `DISTINCT` and `TOP`) | `FROM` (Find the data) |
| 2 | `FROM` | `WHERE` (Filter original rows) |
| 3 | `WHERE` | `GROUP BY` (Aggregate/combine rows) |
| 4 | `GROUP BY` | `HAVING` (Filter aggregated results) |
| 5 | `HAVING` | `SELECT` / `DISTINCT` (Pick columns/unique values) |
| 6 | `ORDER BY` | `ORDER BY` (Sort the final list) |
| 7 | | `TOP` (Limit the number of rows) |

> --- ***Comments in SQL***

Comments are notes for the coder that the database engine ignores.

- Inline: Use two dashes (`--`).
- Multi-line: Wrap the text in `/` and `/`.

---

## **SQL DDL**

> --- ***The CREATE Command***

The `CREATE` command is used to build a new object, like a table, within the database. Usually, a newly created table is empty.

- Syntax & Logic: You must define the table name, column names, data types, and constraints.
- Data Types: Common types include `INT` (numbers), `VARCHAR` (characters/text), and `DATE`.
- Constraints: These are rules for the data, such as `NOT NULL` (the field must have a value) or `PRIMARY KEY` (a unique identifier for each row to ensure integrity).

!!! Example
    Creating a "Persons" Table
    ```sql
        CREATE TABLE persons (
        ID INT NOT NULL,
        person_name VARCHAR(50) NOT NULL,
        birth_date DATE, -- This is optional (nulls allowed)
        phone VARCHAR(15) NOT NULL,
        CONSTRAINT PK_persons PRIMARY KEY (ID) -- Setting the ID as the primary key
        );
    ```

> --- ***The ALTER Command***

The `ALTER` command is used to edit or change the definition of an existing table, such as adding or removing columns or changing data types.

- Adding a Column: When you add a new column, it is automatically placed at the end of the table. If you want a column in the middle, you must drop the whole table and recreate it from scratch.
- Removing a Column: You only need to specify the column name; you do not need to provide the data type because the database already knows it. Warning: Deleting a column also deletes all data stored in that column.

!!! Example
    Adding and Removing Columns

    To Add: `ALTER TABLE persons ADD email VARCHAR(50) NOT NULL;`
    
    To Remove: `ALTER TABLE persons DROP COLUMN phone;`

> --- ***The DROP Command***

The `DROP` command is used to completely remove a table and all its data from the database.

- Risk Level: This is described as the simplest but most risky command because it destroys the table and all its contents instantly.
- Comparison: Destroying a table with `DROP` is much easier than building one with `CREATE`.

!!! Example
    Deleting a Table
    ```sql
    DROP TABLE persons;
    ```

---

## **SQL DML**

> --- ***The INSERT Command***

The `INSERT` command is used to add new rows to a table. These rows are typically appended to the end of the existing data.

Method A: Manual Insertion

In this method, you manually specify the values to be added via a script.

- Syntax: `INSERT INTO table_name (column_list) VALUES (value_list);`.
- Key Rules:
       The number of columns and values must match.
       The order of values must match the order of defined columns.
       Specifying columns is optional; if skipped, SQL expects a value for every column in the table in its default order.
       Nulls: Use `NULL` to indicate unknown data. Note that columns with constraints (like Primary Keys) will not allow null values.

!!! Example
    (Multiple Rows):
    ```sql
    INSERT INTO customers (ID, first_name, country, score)
    VALUES (6, 'Anna', 'USA', NULL),
        (7, 'Sam', NULL, 100);
    (This adds two customers in a single execution; note the comma separating the value lists).
    ```

Method B: Inserting from Another Table

You can move data from a Source Table to a Target Table without writing manual values.

1. Select: Write a `SELECT` query to gather the desired data from the source.

2. Insert: Wrap that query in an `INSERT INTO` statement.
!!! Note
    Column names do not have to match between tables, but the data types and structure must be compatible.

> --- ***The UPDATE Command***

The `UPDATE` command modifies existing rows. Unlike `INSERT`, it does not create new records; it changes the content of what is already there.

- Syntax: `UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;`.
- The Risk of WHERE: If you omit the `WHERE` clause, every single row in the table will be updated with the new value.
- Best Practice: Test your `WHERE` clause with a `SELECT` statement first to ensure you are targeting the correct records before running the update.

!!! Example 
    (Updating Multiple Columns):
    To change customer 10's score and country simultaneously:
    ```sql
    UPDATE customers 
    SET score = 0, country = 'UK' 
    WHERE ID = 10;
    (This targets only the specific row where ID is 10).
    ```

> --- ***The DELETE and TRUNCATE Commands***

These commands are used to remove data from your tables.

DELETE:

Removes specific rows based on a condition.

- Syntax: `DELETE FROM table_name WHERE condition;`.
- Warning: Just like `UPDATE`, forgetting the `WHERE` clause will delete all data in the table.

!!! Example
    ```sql
    DELETE FROM customers WHERE ID > 5;
    (This removes all recently added customers with an ID higher than 5).
    ```

TRUNCATE:

If you want to empty a table completely while keeping its structure intact, `TRUNCATE` is the preferred method.

- Efficiency: It is much faster than `DELETE FROM table_name` because it does not use the same logging and background protocols.
- Behavior: It resets the table to an empty state and does not return the number of rows affected.

| Feature              | DROP                                                                 | TRUNCATE                                                              | DELETE                                                                 |
|----------------------|----------------------------------------------------------------------|------------------------------------------------------------------------|------------------------------------------------------------------------|
| Purpose          | Completely removes the entire table structure from the database      | Removes all rows from a table, but the table structure remains         | Removes specific rows based on a condition or all rows from a table, but the table structure remains |
| Transaction Control | Cannot be rolled back                                              | Cannot be rolled back                                                  | Can be rolled back                                                     |
| Space Reclaiming | Releases the object and its space                                    | Frees the space containing the table                                   | Doesn't free up space, but leaves empty space for future use           |
| Speed            | Fastest as it removes all data and structure                         | Faster than DELETE as it doesn't log individual row deletions          | Slowest as it logs individual row deletions                            |
| Referential Integrity | Not checked                                                     | Checked                                                                | Checked                                                                |
| Where Clause     | Not applicable                                                       | Not applicable                                                         | Applicable                                                             |
| Command Type     | DDL (Data Definition Language)                                       | DDL (Data Definition Language)                                         | DML (Data Manipulation Language)                                       |

---

## **SQL Where**

> --- ***Core Logic of SQL Conditions***

In SQL, the `WHERE` clause is used to filter data based on specific logic. A condition typically follows this formula: Expression -> Operator -> Expression.

You can compare several types of data

- Column to Column: e.g., comparing first name to last name.
- Column to Static Value: e.g., `first_name = 'John'`.
- Functions: Applying a function like `UPPER` to a column before comparing.
- Math Expressions: e.g., `price  quantity = 1000`.
- Subqueries: Comparing a column to the result of another complete query.

When a condition is applied, SQL evaluates the data row by row. If a row meets the condition (True), it is kept; if not (False), it is removed from the final result.

> --- ***Comparison Operators***

These are the most basic operators used to compare two values.

- Equal (`=`): Retrieves exact matches.
!!! Example
    `SELECT  FROM customers WHERE country = 'Germany';`.
- Not Equal (`!=` or `<>`): Retrieves everything except the specified value.
!!! Example
    `SELECT  FROM customers WHERE country != 'Germany';`.
- Greater Than (`>`) and Less Than (`<`): Used for numerical ranges.
!!! Example
    `WHERE score > 500;`.
- Greater Than or Equal to (`>=`) and Less Than or Equal to (`<=`): These are "mixed" operators that include the boundary value itself.
!!! Example
    `WHERE score >= 500;` (Includes rows where the score is exactly 500).

> --- ***Logical Operators***

Logical operators combine multiple conditions within a single `WHERE` clause.

- AND: This is restrictive. All conditions must be True for a row to be included.
!!! Example
    `WHERE country = 'USA' AND score > 500;` (Only keeps customers who meet both criteria).
- OR: This is less restrictive. A row is kept if at least one of the conditions is True.
!!! Example
    `WHERE country = 'USA' OR score > 500;`.
- NOT: This is a reverse operator. it switches True to False and vice versa to exclude matching values.
!!! Example
    `WHERE NOT score < 500;` (This will return all scores that are 500 or higher).

> --- ***Range Operator: BETWEEN***

The `BETWEEN` operator checks if a value falls within a specific range defined by a lower and upper boundary.

- Inclusivity: The boundaries are inclusive, meaning values exactly equal to the boundaries are considered inside the range.
!!! Example
    `SELECT  FROM customers WHERE score BETWEEN 100 AND 500;`.
    Alternative: You can achieve the same result using comparison operators: `WHERE score >= 100 AND score <= 500;`.

> --- ***Membership Operators: IN and NOT IN***

These operators check if a value exists within a specified list.

- Efficiency: Using `IN` is cleaner and more performant than writing multiple `OR` conditions for the same column.
!!! Example
    `SELECT  FROM customers WHERE country IN ('Germany', 'USA');`.
    NOT IN: Reverses the logic to find values not present in the list.

> --- ***Search Operator: LIKE***

The `LIKE` operator is used for pattern matching within text strings. It relies on two special wildcards:

1. Percentage (`%`): Represents "anything"—zero, one, or multiple characters.
2. Underscore (`_`): Represents exactly one character.

!!! Example
    Starts with: `WHERE first_name LIKE 'M%';` (Finds Maria, Martin, etc.).

    Ends with: `WHERE first_name LIKE '%n';` (Finds John, Martin, etc.).
    
    Contains: `WHERE first_name LIKE '%r%';` (Finds any name with an 'r' anywhere in it).
    
    Specific Position: `WHERE first_name LIKE '__r%';` (Uses two underscores to find names where 'r' is exactly the third character).

## **SQL Joins**

> --- ***Combining Tables: Joins vs. Set Operators***

When you want to combine data from two tables (Table A and Table B), you must first decide whether you want to combine columns or rows.

- Joins: Used to combine columns side-by-side. This makes the resulting table wider.
- Set Operators (e.g., UNION): Used to stack rows on top of each other. This makes the resulting table longer.
- Requirement for Joins: You must define key columns (usually IDs) that exist in both tables to connect them.

> --- ***Why Use Joins?***

There are three primary reasons to use joins:

1. Recombine Data: In professional databases, data is often spread across multiple tables (e.g., Customers, Addresses, Orders). Joins allow you to see the "big picture" in one result.
2. Data Enrichment: Using a "lookup" or reference table to add extra information to a master table (e.g., adding zip codes to a customer list).
3. Check Existence: Joining tables to filter data based on whether a record exists (or doesn't exist) in another table (e.g., finding customers who have never placed an order).

> --- ***The Four Basic Join Types****

A. Inner Join (The Default)

An Inner Join returns only the rows where there is a match in both tables.

- Logic: It represents the "overlapping" area of two circles. If a row in the left table has no match in the right table (or vice versa), it is excluded from the results.
- Syntax: `SELECT columns FROM tableA INNER JOIN tableB ON tableA.key = tableB.key;`
!!! Note
    The order of tables does not matter; you will get the same result whether you start with Table A or Table B.

B. Left Join
A Left Join returns all rows from the left table and only the matching rows from the right table.

- Logic: The left table is your "primary" source. If there is no match in the right table, SQL will still show the left table's data but will display NULL for the missing right-side values.
- Syntax: `SELECT columns FROM tableA LEFT JOIN tableB ON tableA.key = tableB.key;`
!!! Example
    To see all customers including those without orders, use a Left Join starting with the customers table.

C. Right Join
The Right Join is the exact opposite of the Left Join. It returns all rows from the right table and only the matching rows from the left table.
!!! Note
    Most developers prefer Left Joins. You can achieve a Right Join effect by simply switching the order of the tables in a Left Join.
!!! Example
    `SELECT columns FROM tableA RIGHT JOIN tableB...` is the same as `SELECT columns FROM tableB LEFT JOIN tableA...`.

D. Full Join
A Full Join returns everything from both tables, regardless of whether there is a match.

- Logic: It combines the effects of both Left and Right joins. You will see matching rows side-by-side, and unmatching rows from either table will show NULL for the missing side.
- Order: Like the Inner Join, the order of tables does not matter.

> --- ***Best Practices for Writing Joins***

- Specify the Type: While `JOIN` often defaults to `INNER JOIN`, it is best practice to explicitly write `INNER`, `LEFT`, or `FULL` to avoid confusion.
- Use Table Aliases: When tables have long names, use the `AS` keyword to give them short nicknames (e.g., `customers AS C`). This makes the query easier to read.
- Prefix Column Names: When joining tables, always specify which table a column belongs to (e.g., `C.ID` vs `O.ID`) to avoid "ambiguous column" errors if both tables have columns with the same name.

> --- ***Summary Table of Execution Logic***

| Join Type | Left Table Data | Right Table Data | If No Match Exists... |
| :--- | :--- | :--- | :--- |
| Inner | Matching Only | Matching Only | Row is excluded. |
| Left | All Rows | Matching Only | Right columns show NULL. |
| Right | Matching Only | All Rows | Left columns show NULL. |
| Full | All Rows | All Rows | Missing sides show NULL. |

> --- ***Left Anti-Join***

A Left Anti-Join returns only the rows from the left table that have no match in the right table. It effectively filters out any overlapping data, leaving you with only the unique records in the primary (left) table.

- Syntax & Logic: There is no specific "ANTI-JOIN" keyword in SQL Server. Instead, you create this effect in two steps:
    1. Perform a standard LEFT JOIN.
    2. Use a WHERE clause to filter for rows where the right table's key IS NULL.

!!! Example
    To find "inactive" customers who have never placed an order
    ```sql
    SELECT  FROM customers AS C
    LEFT JOIN orders AS O ON C.ID = O.customer_id
    WHERE O.customer_id IS NULL;
    This returns only the customers who are in the database but have no corresponding entries in the orders table.
    ```

> --- ***Right Anti-Join***

The Right Anti-Join is the exact opposite of the left version. It returns rows from the right table that have no match in the left table.

- Logic: The right table acts as the primary source, and the left table is used as a filter.
- The "Better" Way: While you can use a `RIGHT JOIN ... WHERE left_key IS NULL`, it is often cleaner to simply switch the table order and use a Left Join. Starting with the main table of interest on the left makes the query easier to read.

!!! Example
    To find "orphaned" orders that do not have a valid customer associated with them:
    ```sql
    SELECT  FROM orders AS O
    LEFT JOIN customers AS C ON O.customer_id = C.ID
    WHERE C.ID IS NULL;
    ```

> --- ***Full Anti-Join**

A Full Anti-Join returns all rows that do not match in either table. This is the opposite of an Inner Join; instead of showing only the overlap, it shows everything except the overlap.

- Syntax: Use a FULL JOIN combined with a WHERE clause using an OR operator to check for nulls on both sides.

!!! Example
    To find all "strange cases" (customers with no orders AND orders with no customers) simultaneously:
    ```sql
    SELECT  FROM customers AS C
    FULL JOIN orders AS O ON C.ID = O.customer_id
    WHERE C.ID IS NULL OR O.customer_id IS NULL;
    ```

> --- ***Mimicking an Inner Join (Bonus Tip)***

You can achieve the effect of an Inner Join by using a Left Join combined with a `WHERE ... IS NOT NULL` condition.

!!! Example
    `SELECT  FROM customers AS C LEFT JOIN orders AS O ON C.ID = O.customer_id WHERE O.customer_id IS NOT NULL;`
    This keeps only the rows where a match was found, effectively filtering out the non-matching left-table rows.

> --- ***Cross Join***

A Cross Join (or Cartesian Join) combines every row from the left table with every row from the right table.

- Key Characteristics:
       It generates all possible combinations.
       The total number of rows returned is the product of the row counts of both tables (e.g., 5 customers × 4 orders = 20 rows).
       No "ON" condition: Unlike other joins, you do not use a key to match rows because you are combining everything regardless of whether they match.
- Use Cases: It is rarely used in daily operations but is helpful for simulations, generating test data, or creating full matrices (e.g., combining every product with every available color).

!!! Example
    ```sql
    SELECT  FROM customers
    CROSS JOIN orders;
    ```

> --- ***Decision Tree for Joining Tables***

When deciding how to combine tables, your choice depends on whether you are looking for matching data, all data, or unmatching data.

- Matching Data Only: Use an Inner Join; this is the only type used when you strictly want the overlapping data between tables.

- All Data (No Missing Records):
    Left Join: Use this when you have a master table (a "main" table) that is more important than the others and you want to keep all its records.
    Full Join: Use this if all tables are equally important and you want to see all data from every side.

- Unmatching Data (Checkups):
    Left Anti-Join: Use this to see unmatching data from one important side.
    Full Anti-Join: Use this when both tables are important and you want to see unmatching data from both.

## **SQL SET**

> --- ***Core Rules for Set Operators***

For a set operator to function, the participating queries must follow these strict structural rules:

- Rule 1: One Order By: You can only use the `ORDER BY` clause once, and it must be placed at the very end of the entire query.
- Rule 2: Matching Column Count: Both queries must select the exact same number of columns.
- Rule 3: Compatible Data Types: The data types of the columns must match or be compatible. SQL uses the first query to set the data type; if the second query's data cannot be converted (e.g., trying to map text to an integer), the query will fail.
- Rule 4: Identical Column Order: SQL maps columns by their position (the first column of Query A with the first column of Query B). You must ensure the data in those positions matches logically.
- Rule 5: First Query Controls Aliases: The column names (aliases) shown in the final output are determined solely by the first query. Any aliases in subsequent queries are ignored.
- Rule 6: Logical Mapping: SQL only checks if data types match, not if the data makes sense. It is the user's responsibility to ensure they aren't accidentally mapping "first names" to "last names".

> --- ***The Four Set Operators***

A. UNION

Returns all distinct, unique rows from both queries. It combines the data and removes any duplicates found in the overlapping sets.
!!! Example
    Combining a customer list and an employee list to find every unique person. If "Kevin Brown" is both a customer and an employee, he will appear only once in the result.

B. UNION ALL

Returns all rows from both queries, including duplicates.

- Performance: `UNION ALL` is significantly faster than `UNION` because it skips the extra step of searching for and removing duplicates.
!!! Example
    Combining the same lists as above, "Kevin Brown" would appear twice in the output.

C. EXCEPT (or MINUS)

Returns distinct rows from the first query that are not found in the second query. This is the only set operator where the order of queries changes the result.
!!! Example
    `SELECT name FROM employees EXCEPT SELECT name FROM customers;` finds employees who are not also customers.

D. INTERSECT

Returns only the rows that are common to both queries (the overlap) and removes duplicates.
!!! Example
    `SELECT name FROM employees INTERSECT SELECT name FROM customers;` finds only the people who are both employees and customers.

> --- 3. Professional Use Cases and Examples

- Use Case 1: Consolidating Similar Tables
Instead of writing four separate reports for Employees, Customers, Suppliers, and Students, you can use `UNION` to combine them into one "Persons" table. This allows you to write a single analytical query on the combined data rather than repeating logic for four different tables.

- Use Case 2: Handling Split Tables
Databases often split large tables by year (e.g., `Orders_2023`, `Orders_2024`). You can use `UNION` to recreate one master `Orders` table for multi-year analysis.

- Use Case 3: Data Migration and Quality Checks

To ensure two tables are identical after a migration:

1. Run `TableA EXCEPT TableB`.
2. Run `TableB EXCEPT TableA`.
3. If both results are empty, the tables are 100% identical.

> --- ***Best Practices for Set Operators***

- Avoid `SELECT`: Explicitly list your columns. This prevents errors if one table's schema changes (e.g., adding or reordering columns) in the future.

- Add a "Source" Column: When combining tables, add a static string to identify where each row originated.

!!! Example
    ```sql
    SELECT 'Orders' AS SourceTable, OrderID FROM Orders
    UNION
    SELECT 'Archive' AS SourceTable, OrderID FROM Orders_Archive;
    This helps users distinguish between active and archived data in the final report.
    ```

## **SQL Functions**

> --- ***What is an SQL Function?***

A function is a built-in code block that accepts an input value, processes it through transformations, and returns a result as an output value. Functions are essential for four main types of tasks:

- Data Manipulation: Changing values to solve specific tasks.
- Aggregations & Analysis: Finding insights and building reports.
- Data Cleansing: Identifying and fixing "bad" data within tables.
- Data Transformation: Converting data into a more usable format.

> --- ***The Two Main Categories***

Functions are divided into two high-level groups based on how they handle rows:

- Single-Row Functions: These follow a "one value in, one value out" logic.
!!! Example
    If you input the name "Maria," the function processes that specific string and returns a single modified value (like "MARIA" or "ma").
- Multi-Row (Aggregate) Functions: These take multiple rows as input and return a single summary value.
!!! Example
    The `SUM` function can take a column of values (e.g., 30, 10, 20, 40) and return the single total of 100.

> --- ***Nesting Functions (The "Factory" Logic)***

In SQL, you can nest functions, meaning you use multiple functions together to manipulate a single value. The sources compare this to a factory where material is processed through multiple stations.

- How it Works: The output of the first function becomes the input for the next function.
!!! Example
    1. LEFT: Extract the first two characters from "Maria" → Output: "Ma".
    2. LOWER: Take "Ma" and convert it to lowercase → Output: "ma".
    3. LEN (Length): Measure the characters in "ma" → Output: 2.
- Execution Order: SQL always executes from the inner function to the outer function. In the example above, `LEFT` runs first, followed by `LOWER`, and finally `LEN`.

> --- ***Functional Subcategories***

Within the two main categories, functions are further grouped by the type of data they handle:

| Single-Row Functions (Data Prep) | Multi-Row Functions (Analysis) |
| :--- | :--- |
| String: Manipulating text values. | Basic Aggregates: Standard summaries like SUM or AVG. |
| Numeric: Handling math and numbers. | Window/Analytical: Advanced functions for complex data analysis. |
| Date & Time: Formatting and calculating time. | |
| Null Handling: Managing missing data. | |

## **SQL String Functions**

> --- ***Data Manipulation Functions***

These functions are used to transform or combine existing string values into a new format.

- CONCAT (Concatenation): Combines multiple string values into a single column. This is useful for merging separated data, such as first and last names, into a "Full Name" field.
!!! Example
    `SELECT CONCAT(first_name, ' ', country) AS name_country FROM customers;`.
- UPPER and LOWER: These change the case of a string. `UPPER` capitalizes every character, while `LOWER` converts everything to lowercase.
!!! Example
    `SELECT LOWER(first_name) AS low_name, UPPER(first_name) AS up_name FROM customers;`,.
- TRIM: Removes "leading" (start) and "trailing" (end) spaces from a string. Spaces are often considered "evil" in data because they are hard to see but can cause errors in queries. To find rows with hidden spaces, compare the column to its trimmed version: `WHERE first_name != TRIM(first_name)`.
- REPLACE: Substitutes a specific "old" character or string with a "new" one. To remove a character entirely, replace the old value with an empty string (`''`),.
!!! Example
    Changing a file extension from `.txt` to `.csv`: `REPLACE('reports.txt', '.txt', '.csv')`.

> --- ***Calculation Functions***

LEN (Length): Counts the total number of characters, digits, or symbols in a value.

- Versatility: It works on strings, numbers, and dates (including separators like underscores or dashes).
!!! Example
    `SELECT LEN(first_name) FROM customers;`.

> --- ***Extraction Functions***
These are used to pull out specific parts of a string based on position,.

- LEFT and RIGHT: Extracts a specific number of characters starting from either the beginning (`LEFT`) or the end (`RIGHT`) of the string,. It is often best to `TRIM` a value before using `LEFT` or `RIGHT` to ensure you aren't accidentally extracting empty spaces.
!!! Example
    `SELECT LEFT(first_name, 2)` retrieves the first two characters.
- SUBSTRING: Extracts a part of a string starting from any specified position. It requires three arguments: the value, the starting position, and the number of characters to extract.
!!! Example
    To extract two characters starting after the second character, you would set the starting position to 3.

> --- ***Advanced Concept: Building Dynamic Queries***

In professional SQL, functions are often nested to solve complex tasks. A common "trick" for the `SUBSTRING` function is to make it dynamic so it works on strings of varying lengths,.

- Dynamic Extraction: If you want to extract "everything after the first character," you can start at position 2 and use the `LEN` function as the third argument,. This ensures that whether a name is 4 characters or 20 characters long, the query will always capture the remainder of the string.

!!! Example
    ```sql
    SELECT SUBSTRING(TRIM(first_name), 2, LEN(first_name)) FROM customers;
    ```

---

## **SQL Number Functions**

> --- ***The ROUND Function***

The `ROUND` function is used to round a numeric value to a specified number of decimal places.

- How it Works: SQL looks at the digit immediately following your specified decimal place to determine whether to round the number up or leave it as is.
   The "5 or Higher" Rule:
       If the deciding digit is 5 or higher, SQL rounds the number up,.
       If the deciding digit is less than 5, the number does not round up and stays as it is.
- Resetting Digits: Any digits remaining after the rounding point are reset to zero,.

!!! Examples
    Rounding to 2 Decimal Places: `ROUND(3.516, 2)` results in 3.52. The third digit (6) is higher than 5, so the second digit (1) rounds up to 2.
    Rounding to 1 Decimal Place: `ROUND(3.516, 1)` results in 3.5. The second digit (1) is less than 5, so the first digit (5) remains unchanged.
    Rounding to 0 Decimal Places: `ROUND(3.516, 0)` results in 4. Because the first digit after the decimal (5) is 5 or higher, the integer (3) is rounded up to 4.

> --- ***The ABS (Absolute) Function***

The `ABS` function is used to determine the absolute value of a number, effectively converting any negative number into a positive number.

- Behavior:
       If the input is negative, it returns the positive version.
       If the input is already positive, the function does nothing and returns the same value.
- Professional Application: This is highly useful for correcting database errors. For example, if a database accidentally contains "negative sales" (which is logically impossible), you can use `ABS` to transform those errors into valid positive numbers.

!!! Examples
    `ABS(-10)` returns 10.
    `ABS(10)` returns 10.

---

## **SQL Date Time Functions**

> --- ***Three Sources of Dates***

You can query date information from three different sources.
1. Stored Columns: Data already saved in database tables (e.g., `order_date` or `creation_time`).
2. Hardcoded Strings: Static dates added manually to a query (e.g., `'2025-08-20'`).
3. GETDATE() Function: A fundamental function that returns the current system date and time at the moment the query is executed.

> --- ***Part Extraction Functions***

These functions extract specific components from a date.

- Basic Extraction (Day, Month, Year)
These functions are quick and return the result as an integer.
!!! Example
    `SELECT MONTH(creation_time) AS month_num` returns `1` for January, `2` for February, etc..

- DATEPART
Returns a specific part of a date as an integer. It is more powerful than the basic functions because it can extract parts like weeks, quarters, and hours.
!!! Example
    `DATEPART(quarter, creation_time)` returns `1` for dates in January through March.

- DATENAME
Similar to `DATEPART`, but it returns the name of the part as a string (text), making it ideal for human-readable reports.
!!! Example:
    `DATENAME(month, '2025-08-20')` returns "August" (string), whereas `DATEPART` would return 8 (integer).
    Usage Tip: Use `DATENAME(weekday, date)` to get the full name of the day, such as "Wednesday".

> --- ***Truncation and Calculations***

- DATETRUNC
This function "truncates" a date to a specific level in the hierarchy by resetting all lower levels to their minimum values (0 for time, 01 for days). If you truncate at the Month level, the Year and Month are kept, but the Day is reset to `01` and the Time is reset to `00:00:00`.
!!! Example
    `DATETRUNC(year, creation_time)` resets everything to January 1st of that year.

- EOMONTH (End of Month)
Returns the last day of the month for a given date.
!!! Example
    If the input is `'2025-02-01'`, `EOMONTH` returns `'2025-02-28'`.
    Trick for Start of Month: While there is no "Start of Month" function, you can use `DATETRUNC` at the month level to get the first day of any month.

> --- ***Professional Applications and Best Practices***

- Data Aggregation
Extracting parts allows you to group data for reporting, such as calculating "Total Sales by Year" or "Orders per Month".
!!! Example
    To count orders per month:
    `SELECT MONTH(order_date), COUNT() FROM sales_orders GROUP BY MONTH(order_date);`.

- Filtering Data
You can use extracted parts in a `WHERE` clause to filter for specific timeframes.
!!! Example
    To see only February orders: `WHERE MONTH(order_date) = 2;`.
    Performance Tip: Always prefer filtering by integers (using `MONTH` or `DATEPART`) rather than strings (using `DATENAME`). Searching for numbers is faster for the database.

> ---  ***Formatting vs. Casting***

- Formatting: Changing how a value looks. For example, displaying a date with slashes instead of dashes, or showing a number with a currency symbol.
- Casting: Changing the data type from one to another. For example, converting a string of text ("123") into an actual integer (123) so you can perform math on it.

> --- ***Date Format Specifiers***

To format dates, SQL uses "format specifiers" which act as a code for different components. These are case-sensitive.

- Year: `yyyy` (4 digits) or `yy` (2 digits).
- Month: `MM` (2 digits), `MMM` (abbreviation), or `MMMM` (full name).
!!! Note
    Big M is for Month; small m is for minutes.
- Day: `dd` (2 digits), `ddd` (abbreviated name), or `dddd` (full name).
- Time: `HH` (24-hour), `hh` (12-hour), `mm` (minutes), `ss` (seconds), and `tt` (AM/PM designator).

> --- ***The `FORMAT` Function***

This function is used primarily to change the look of dates and numbers into a string value.

- Syntax: `FORMAT(value, format, [culture])`

!!! Examples
    Date Formatting: `FORMAT(creation_time, 'dd-MM-yyyy')` changes the international standard (yyyy-MM-dd) to a European style.
    Number Formatting:
       'N': Adds commas to large numbers (Numeric).
       'C': Adds a currency symbol (Currency).
       'P': Converts the number to a percentage (Percentage).
    Custom Strings: You can combine static text with `FORMAT` to create complex labels, such as "Day: Monday Jan Q1 2025".

> --- ***The `CONVERT` Function***

`CONVERT` is a versatile function that can perform both casting and formatting for date and time values.

- Syntax: `CONVERT(data_type, value, [style_number])`
!!! Examples
    Casting Only: `CONVERT(int, '123')` turns a string into an integer.
    Casting and Formatting: `CONVERT(varchar, creation_time, 32)` converts a date-time value into a string formatted as `MM-dd-yyyy`.

> --- ***The `CAST` Function***

`CAST` is the most straightforward function, used strictly for changing data types. It does not allow for custom styling or formatting; it always returns the SQL standard format.

- Syntax: `CAST(value AS data_type)`

!!! Examples
    String to Number: `CAST('123' AS int)`.
    DateTime to Date: `CAST(creation_time AS date)` removes the time information and keeps only the date.
    Integer to String: `CAST(123 AS varchar)`.

> --- ***Function Comparison Summary***

| Feature | `CAST` | `CONVERT` | `FORMAT` |
| :--- | :--- | :--- | :--- |
| Primary Goal | Change data type | Change type & format | Change look (to string) |
| Formatting | No formatting | Date/Time styles only | Date/Time and Numbers |
| Output Type | Any type | Any type | String only |
| Best For... | Quick type changes | Date standard styles | Custom reports/Numbers |

!!! Tip
    `FORMAT` is highly useful for data preparation before aggregation. You can format dates as 'MMM-yy' (e.g., "Jan-25") and then group by that column to create clean, readable monthly sales reports.

> --- ***DATEADD Function***

The DATEADD function is used to add or subtract a specific time interval (years, months, days, etc.) to or from a date,.

- Syntax: `DATEADD(part, interval, date)`.
    Part: The specific portion of the date you want to manipulate (e.g., `year`, `month`, `day`).
    Interval: The amount you want to change. A positive number adds time, while a negative number subtracts time.
    Date: The original date value you are starting from.

!!! Examples
    Adding Years: `DATEADD(year, 2, order_date)` results in a date exactly two years later than the original.
    Adding Months: `DATEADD(month, 3, order_date)` moves the date three months forward (e.g., January becomes April).
    Subtracting Days: `DATEADD(day, -10, order_date)` retrieves a date 10 days before the original.

> --- ***DATEDIFF Function***

The DATEDIFF function calculates the difference between two dates and returns the result as a number,.

- Syntax: `DATEDIFF(part, start_date, end_date)`.
    Part: The unit of measurement for the result (e.g., how many `years`, `months`, or `days` are between them).
    Start Date: The earlier (younger) date.
    End Date: The later (older) date.

- Professional Use Cases:

1. Calculating Age: To find an employee's age, you calculate the years between their birth date and the current date: `DATEDIFF(year, birth_date, GETDATE())`,.
2. Shipping Duration: To find how long it took to ship an order, you calculate the days between the order date and the shipping date: `DATEDIFF(day, order_date, ship_date)`.
3. Time Gap Analysis: You can use `DATEDIFF` alongside the `LAG` window function to find the number of days between a current order and the previous order placed by a customer,.

> --- ***ISDATE Function***

The ISDATE function is a validation tool used to check if a string or value is a valid date.

- Return Values: It returns 1 (True) if the value is a valid date and 0 (False) if it is not.
- Logic: SQL evaluates whether the value follows standard database date formats.
!!! Examples
    `ISDATE('2025-08-20')` returns 1.
    `ISDATE('123')` returns 0.
    `ISDATE('2025')` returns 1 (SQL is smart enough to interpret a single year as the first of January of that year).
    Non-standard formats: If you provide a date in a format SQL does not recognize (e.g., `day/month/year` when the system expects `year/month/day`), it will return 0.

> --- ***Advanced Use Case: Data Cleansing***

A major use for `ISDATE` is handling data quality issues where a string column contains mostly dates but some "corrupt" or invalid text.

- The Problem: Attempting to `CAST` a string column directly to a `DATE` type will result in a query error if even one row contains invalid data.
- The Solution: Use a CASE WHEN statement combined with `ISDATE` to safely convert the data.

!!! Example
    ```sql
    CASE
        WHEN ISDATE(order_date) = 1 THEN CAST(order_date AS DATE)
        ELSE NULL -- Or a dummy value like '9999-12-31'
    END AS New_Order_Date
    This approach allows you to successfully convert valid strings into dates while turning invalid strings into `NULL` (or a identifiable dummy value) instead of crashing the entire query.
    ```

---

## **SQL NULL Functions**

> --- ***Understanding NULLs***

In SQL, a NULL represents a missing, unknown, or nonexistent value.
   It is not zero: NULL is not equivalent to the number 0.
   It is not an empty string: It is not a blank space or an empty set of quotes ('').
   Logical Meaning: A NULL simply tells us that there is "nothing" there or the value is currently unknown.

> --- ***Functions to Replace NULLs***

If you want to remove a NULL from your results and replace it with a meaningful value, you use `ISNULL` or `COALESCE`.

A. ISNULL (SQL Server Specific)

This function checks a value and replaces it if it is NULL. It is limited to two arguments.
   Syntax: `ISNULL(check_expression, replacement_value)`.

!!! Example
    (Static Replacement): `ISNULL(shipping_address, 'Unknown')` replaces any missing address with the word "Unknown".
    (Column Replacement): `ISNULL(shipping_address, billing_address)` uses the billing address as a backup if the shipping address is missing.

B. COALESCE (Standard SQL)

`COALESCE` is more flexible and powerful because it accepts a list of multiple values. It returns the first non-null value in that list, checking from left to right.

!!! Example
    `COALESCE(shipping_address, billing_address, 'N/A')`. SQL checks the shipping address; if that's null, it checks the billing address; if both are null, it returns "N/A".
    Advantage: It is a database standard and works across SQL Server, Oracle, and MySQL, making scripts easier to migrate.

> --- ***Function to Create NULLs: NULLIF***

`NULLIF` does the opposite of the functions above; it replaces a real value with a NULL.
  
- Logic: It returns NULL if the two values are equal. If they are not equal, it returns the first value.
Use Case (Data Cleansing): If a price is entered as -1 (an error), you can turn it into a NULL: `NULLIF(price, -1)`.
Use Case (Preventing Errors): To avoid "Divide by Zero" errors, you can wrap the divisor: `Sales / NULLIF(quantity, 0)`.

> --- ***Keywords for Checking NULLs***

To filter or check for the existence of NULLs without changing them, use these boolean keywords.

- IS NULL: Returns true if the value is missing.
!!! Example
    `WHERE score IS NULL` finds customers with no scores.
- IS NOT NULL: Returns true if a value exists.
!!! Example
    `WHERE score IS NOT NULL` retrieves a clean list of customers with scores.

> --- ***Professional Use Cases***

- Data Aggregations
Standard aggregate functions (SUM, AVG, MIN, MAX) totally ignore NULLs.
   The Risk: If you have sales of 15, 25, and NULL, `AVG` will return 20 (it divides by 2 instead of 3).
   The Fix: If the business considers a NULL to be a zero, handle it first: `AVG(COALESCE(score, 0))`.

- Mathematical & String Operations
Any operation involving a NULL (using `+`, `-`, etc.) results in a NULL because you cannot perform math on an unknown value.
   String Concatenation: `First_Name + ' ' + Last_Name` will return NULL if the last name is missing.
   The Fix: Use `COALESCE(last_name, '')` to replace the NULL with an empty string so the first name still displays.

- Joining Tables
SQL cannot compare NULLs in join keys. If Table A has a NULL in the key and Table B also has a NULL, an Inner Join will ignore them, and you will lose data.
   The Fix: Handle the NULLs directly in the `ON` clause: `ON ISNULL(T1.Type, '') = ISNULL(T2.Type, '')`.

- Sorting Data
By default, `ORDER BY` places NULLs at the start of an ascending list.

Professional Solution: To force NULLs to the end while keeping the rest in ascending order, create a "flag" column

    ```sql
    ORDER BY (CASE WHEN score IS NULL THEN 1 ELSE 0 END), score;
    This pushes the NULLs (flag 1) to the bottom regardless of the actual score values.
    ```
    

- Advanced: Left Anti-Join
This technique finds records in the left table that have no match in the right table (e.g., customers who have never placed an order).
   How-To: Perform a `LEFT JOIN` and then use `WHERE [Right_Table_Key] IS NULL` to filter for the unmatching rows.
