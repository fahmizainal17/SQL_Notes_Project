# **SQL Documentation: Fundamentals and Advanced Concepts**

## **Table of Contents**
1. [Introduction to SQL](#introduction-to-sql)
2. [Fundamental SQL Concepts](#fundamental-sql-concepts)
   - 2.1 [Data Types](#data-types)
   - 2.2 [Basic SQL Commands](#basic-sql-commands)
     - 2.2.1 [SELECT](#select)
     - 2.2.2 [WHERE](#where)
     - 2.2.3 [INSERT](#insert)
     - 2.2.4 [UPDATE](#update)
     - 2.2.5 [DELETE](#delete)
     - 2.2.6 [JOIN](#join)
   - 2.3 [SQL Functions](#sql-functions)
     - 2.3.1 [Aggregate Functions](#aggregate-functions)
     - 2.3.2 [String Functions](#string-functions)
     - 2.3.3 [Date Functions](#date-functions)
3. [Advanced SQL Concepts](#advanced-sql-concepts)
   - 3.1 [Subqueries](#subqueries)
   - 3.2 [Common Table Expressions (CTEs)](#common-table-expressions-ctes)
   - 3.3 [Window Functions](#window-functions)
   - 3.4 [Indexes](#indexes)
   - 3.5 [Views](#views)
   - 3.6 [Stored Procedures and Triggers](#stored-procedures-and-triggers)
4. [Use Case Scenario: Sales Data Analysis](#use-case-scenario-sales-data-analysis)
   - 4.1 [Scenario Overview](#scenario-overview)
   - 4.2 [Step-by-Step Analysis](#step-by-step-analysis)
5. [Conclusion](#conclusion)

---

## **1. Introduction to SQL**

SQL (Structured Query Language) is the standard language for interacting with relational databases. It allows users to create, read, update, and delete data within a database, as well as manage database structures like tables, indexes, and views.

## **2. Fundamental SQL Concepts**

### **2.1 Data Types**
Understanding SQL data types is crucial as they define the type of data that can be stored in a database table. Common data types include:
- **INT**: Integer numbers.
- **VARCHAR**: Variable-length character strings.
- **DATE**: Dates (YYYY-MM-DD format).
- **FLOAT**: Floating-point numbers.

### **2.2 Basic SQL Commands**

#### **2.2.1 SELECT**
The `SELECT` statement is used to retrieve data from one or more tables.

```sql
SELECT column1, column2 FROM table_name;
```

#### **2.2.2 WHERE**
The `WHERE` clause is used to filter records based on a condition.

```sql
SELECT * FROM employees WHERE department = 'Sales';
```

#### **2.2.3 INSERT**
The `INSERT` statement is used to add new records to a table.

```sql
INSERT INTO employees (name, department, salary) VALUES ('John Doe', 'HR', 50000);
```

#### **2.2.4 UPDATE**
The `UPDATE` statement is used to modify existing records in a table.

```sql
UPDATE employees SET salary = 55000 WHERE name = 'John Doe';
```

#### **2.2.5 DELETE**
The `DELETE` statement is used to remove records from a table.

```sql
DELETE FROM employees WHERE name = 'John Doe';
```

#### **2.2.6 JOIN**
`JOIN` operations are used to combine rows from two or more tables based on a related column.

- **INNER JOIN**: Returns records that have matching values in both tables.

  ```sql
  SELECT orders.order_id, customers.customer_name
  FROM orders
  INNER JOIN customers ON orders.customer_id = customers.customer_id;
  ```

### **2.3 SQL Functions**

#### **2.3.1 Aggregate Functions**
Aggregate functions perform a calculation on a set of values and return a single value.
- **COUNT**: Returns the number of rows.

  ```sql
  SELECT COUNT(*) FROM employees;
  ```

- **SUM**: Returns the total sum of a numeric column.

  ```sql
  SELECT SUM(salary) FROM employees;
  ```

- **AVG**: Returns the average value.

  ```sql
  SELECT AVG(salary) FROM employees;
  ```

#### **2.3.2 String Functions**
String functions allow for manipulation of string data types.
- **CONCAT**: Combines two or more strings.

  ```sql
  SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM employees;
  ```

- **UPPER**: Converts a string to uppercase.

  ```sql
  SELECT UPPER(name) FROM employees;
  ```

#### **2.3.3 Date Functions**
Date functions are used to perform operations on date data types.
- **NOW()**: Returns the current date and time.

  ```sql
  SELECT NOW();
  ```

- **DATEDIFF**: Returns the difference in days between two dates.

  ```sql
  SELECT DATEDIFF(NOW(), hire_date) FROM employees;
  ```

## **3. Advanced SQL Concepts**

### **3.1 Subqueries**
A subquery is a query within another query.

```sql
SELECT name FROM employees WHERE salary > (SELECT AVG(salary) FROM employees);
```

### **3.2 Common Table Expressions (CTEs)**
CTEs provide a way to create temporary result sets that can be referenced within a SELECT, INSERT, UPDATE, or DELETE statement.

```sql
WITH Sales_CTE AS (
    SELECT department, SUM(sales) AS total_sales
    FROM sales
    GROUP BY department
)
SELECT department, total_sales FROM Sales_CTE WHERE total_sales > 10000;
```

### **3.3 Window Functions**
Window functions perform a calculation across a set of table rows that are related to the current row.

```sql
SELECT employee_id, salary, RANK() OVER (ORDER BY salary DESC) AS rank FROM employees;
```

### **3.4 Indexes**
Indexes are used to speed up the retrieval of rows by using a pointer. They can be created on one or more columns of a table.

```sql
CREATE INDEX idx_salary ON employees(salary);
```

### **3.5 Views**
Views are virtual tables based on the result of a SELECT query. They simplify complex queries by providing a way to save them for repeated use.

```sql
CREATE VIEW high_salary_employees AS
SELECT name, salary FROM employees WHERE salary > 70000;
```

### **3.6 Stored Procedures and Triggers**
- **Stored Procedures**: A stored procedure is a set of SQL statements that can be executed as a program.

  ```sql
  CREATE PROCEDURE IncreaseSalary(@EmployeeID INT, @IncreaseAmount DECIMAL(10,2))
  AS
  BEGIN
      UPDATE employees
      SET salary = salary + @IncreaseAmount
      WHERE employee_id = @EmployeeID;
  END;
  ```

- **Triggers**: Triggers automatically execute in response to certain events on a particular table or view.

  ```sql
  CREATE TRIGGER trgAfterInsert
  ON employees
  AFTER INSERT
  AS
  BEGIN
      PRINT 'Record inserted into employees table';
  END;
  ```

## **4. Use Case Scenario: Sales Data Analysis**

### **4.1 Scenario Overview**
A retail company wants to analyze its sales data to identify trends and improve its business strategies. The database contains tables such as `orders`, `customers`, `products`, and `sales`. The goal is to generate reports that show sales performance, customer behavior, and product popularity.

### **4.2 Step-by-Step Analysis**

1. **Total Sales by Product:**
   - Identify which products are generating the most revenue.

   ```sql
   SELECT product_name, SUM(total_amount) AS total_sales
   FROM sales
   INNER JOIN products ON sales.product_id = products.product_id
   GROUP BY product_name
   ORDER BY total_sales DESC;
   ```

2. **Customer Purchase Frequency:**
   - Determine how often customers make purchases.

   ```sql
   SELECT customer_name, COUNT(order_id) AS purchase_count
   FROM orders
   INNER JOIN customers ON orders.customer_id = customers.customer_id
   GROUP BY customer_name
   ORDER BY purchase_count DESC;
   ```

3. **Monthly Sales Trend:**
   - Analyze sales trends on a monthly basis to identify peak seasons.

   ```sql
   SELECT MONTH(order_date) AS month, SUM(total_amount) AS total_sales
   FROM orders
   GROUP BY MONTH(order_date)
   ORDER BY month;
   ```

4. **Average Order Value by Customer:**
   - Calculate the average order value for each customer to understand spending patterns.

   ```sql
   SELECT customer_name, AVG(total_amount) AS average_order_value
   FROM orders
   INNER JOIN customers ON orders.customer_id = customers.customer_id
   GROUP BY customer_name
   ORDER BY average_order_value DESC;
   ```

## **5. Conclusion**

SQL is a powerful language for managing and analyzing data in relational databases. By mastering both fundamental and advanced SQL concepts, you can perform complex queries, optimize database performance, and generate insightful reports that drive business decisions.

---
