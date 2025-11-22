# Experiment 2: DDL Commands

## AIM
To study and implement DDL commands and different types of constraints.

## THEORY

### 1. CREATE
Used to create a new relation (table).

**Syntax:**
```sql
CREATE TABLE (
  field_1 data_type(size),
  field_2 data_type(size),
  ...
);
```
### 2. ALTER
Used to add, modify, drop, or rename fields in an existing relation.
(a) ADD
```sql
ALTER TABLE std ADD (Address CHAR(10));
```
(b) MODIFY
```sql
ALTER TABLE relation_name MODIFY (field_1 new_data_type(size));
```
(c) DROP
```sql
ALTER TABLE relation_name DROP COLUMN field_name;
```
(d) RENAME
```sql
ALTER TABLE relation_name RENAME COLUMN old_field_name TO new_field_name;
```
### 3. DROP TABLE
Used to permanently delete the structure and data of a table.
```sql
DROP TABLE relation_name;
```
### 4. RENAME
Used to rename an existing database object.
```sql
RENAME TABLE old_relation_name TO new_relation_name;
```
### CONSTRAINTS
Constraints are used to specify rules for the data in a table. If there is any violation between the constraint and the data action, the action is aborted by the constraint. It can be specified when the table is created (using CREATE TABLE) or after it is created (using ALTER TABLE).
### 1. NOT NULL
When a column is defined as NOT NULL, it becomes mandatory to enter a value in that column.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) NOT NULL
);
```
### 2. UNIQUE
Ensures that values in a column are unique.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) UNIQUE
);
```
### 3. CHECK
Specifies a condition that each row must satisfy.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) CHECK (logical_expression)
);
```
### 4. PRIMARY KEY
Used to uniquely identify each record in a table.
Properties:
Must contain unique values.
Cannot be null.
Should contain minimal fields.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) PRIMARY KEY
);
```
### 5. FOREIGN KEY
Used to reference the primary key of another table.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size),
  FOREIGN KEY (column_name) REFERENCES other_table(column)
);
```
### 6. DEFAULT
Used to insert a default value into a column if no value is specified.

Syntax:
```sql
CREATE TABLE Table_Name (
  col_name1 data_type,
  col_name2 data_type,
  col_name3 data_type DEFAULT 'default_value'
);
```
**Question 1**
--
-- Paste Question 1 here
Write a SQL query to Add a new column Mobilenumber as number in the Student_details table.

```sql
-- Paste your SQL code below for Question 1

 ALTER TABLE Student_details
ADD Mobilenumber number;

```

**Output:**

<img width="1250" height="197" alt="image" src="https://github.com/user-attachments/assets/edf56af3-4386-4dac-a002-601c75af251b" />


**Question 2**
---
--


Create a table named Products with the following constraints:

ProductID should be the primary key.
ProductName should be NOT NULL.
Price is of real datatype and should be greater than 0.
Stock is of integer datatype and should be greater than or equal to 0.

```sql
-- Paste your SQL code below for Question 2
CREATE TABLE Products(
   ProductID INT PRIMARY KEY,
   ProductName VARCHAR(100) NOT NULL,
   Price REAL CHECK(Price>0),
   Stock INT CHECK(Stock>=0)
);
```

**Output:**

<img width="1025" height="128" alt="image" src="https://github.com/user-attachments/assets/924acd7e-1156-467e-ad51-2abe32093c2b" />


**Question 3**
---
-- 
Write a SQL query to add a column named Date_of_birth as Date in the Student_details table.

```sql
-- Paste your SQL code below for Question 3
ALTER TABLE Student_details
ADD Date_of_birth Date;
```

**Output:**

<img width="1253" height="251" alt="image" src="https://github.com/user-attachments/assets/eaa5180f-355e-4992-acaa-a44572af587d" />


**Question 4**
---
-- 
Insert the following products into the Products table:

Name        Category     Price       Stock
----------  -----------  ----------  ----------
Smartphone  Electronics  800         150
Headphones  Accessories  200         300

```sql
-- Paste your SQL code below for Question 4
INSERT INTO Products(Name,Category,Price,Stock)
VALUES('Smartphone','Electronics',800,150);

INSERT INTO Products(Name,Category,Price,Stock)
VALUES('Headphones','Accessories',200,300);
```

**Output:**

<img width="1127" height="240" alt="image" src="https://github.com/user-attachments/assets/9bc834eb-c0a8-4604-82e0-9cfe48684fab" />


**Question 5**
---
--
Insert all students from Archived_students table into the Student_details table.

```sql
-- Paste your SQL code below for Question 5
INSERT INTO Student_details
SELECT * FROM Archived_students;
```

**Output:**

<img width="1202" height="179" alt="image" src="https://github.com/user-attachments/assets/1c748d38-4fe7-4417-b785-b0c2d912fde1" />


**Question 6**
---
--
Create a table named Orders with the following constraints:
OrderID as INTEGER should be the primary key.
OrderDate as DATE should be not NULL.
CustomerID as INTEGER should be a foreign key referencing Customers(CustomerID).

```sql
-- Paste your SQL code below for Question 6
CREATE TABLE Orders(
    OrderID INT PRIMARY KEY,
    OrderDate Date NOT NULL,
    CustomerID INT,
    FOREIGN KEY(CustomerID) REFERENCES
    Customers(CustomerID)
);
```

**Output:**

<img width="959" height="111" alt="image" src="https://github.com/user-attachments/assets/470d20a4-c875-40da-97d7-82f86ea403fe" />


**Question 7**
---
--
Create a table named Tasks with the following columns:

TaskID as INTEGER
TaskName as TEXT
DueDate as DATE

```sql
-- Paste your SQL code below for Question 7
CREATE TABLE Tasks(
     TaskID INTEGER,
     TaskName TEXT,
     DueDate DATE
);
```

**Output:**

<img width="1263" height="221" alt="image" src="https://github.com/user-attachments/assets/5e3eefed-5885-4164-9642-d5a9c9d9ab8a" />


**Question 8**
---
-- 
Insert a record with EmployeeID 001, Name Sarah Parker, Position Manager, Department HR, and Salary 60000 into the Employee table.

```sql
-- Paste your SQL code below for Question 8
INSERT INTO Employee(EmployeeId,Name,Position,Department,Salary)
VALUES(001,'Sarah Parker','Manager','HR',60000);
```

**Output:**

<img width="1102" height="140" alt="image" src="https://github.com/user-attachments/assets/fbebb045-1284-4fb0-b2eb-88e74c3fb6dd" />


**Question 9**
---
--

Create a table named ProjectAssignments with the following constraints:
AssignmentID as INTEGER should be the primary key.
EmployeeID as INTEGER should be a foreign key referencing Employees(EmployeeID).
ProjectID as INTEGER should be a foreign key referencing Projects(ProjectID).
AssignmentDate as DATE should be NOT NULL.
```sql
-- Paste your SQL code below for Question 9
CREATE TABLE ProjectAssignments(
   AssignmentID INTEGER PRIMARY KEY,
   EmployeeID INTEGER,
   ProjectID INTEGER,
   AssignmentDate DATE NOT NULL,
   FOREIGN KEY (EmployeeID) REFERENCES
   Employees(EmployeeID),
   FOREIGN KEY (ProjectID) REFERENCES
   Projects(ProjectID)
);
```

**Output:**

<img width="1095" height="116" alt="image" src="https://github.com/user-attachments/assets/bd7af639-34b7-4c8b-ac2c-0e5a4050fbc6" />


**Question 10**
---
-- 
create a table named jobs including columns job_id, job_title, min_salary and max_salary, and make sure that, the default value for job_title is blank and min_salary is 8000 and max_salary is NULL will be entered automatically at the time of insertion if no value assigned for the specified columns.

```sql
-- Paste your SQL code below for Question 10
CREATE TABLE jobs(
job_id INT PRIMARY KEY,
job_title VARCHAR(100) DEFAULT NULL,
min_salary INT DEFAULT 8000,
max_salary INT DEFAULT NULL
);
```

**Output:**

<img width="1047" height="147" alt="image" src="https://github.com/user-attachments/assets/55d1453b-3358-41de-ab87-187e811ae9af" />


## MARKS:

<img width="1912" height="1072" alt="image" src="https://github.com/user-attachments/assets/0d57c7fd-0836-45a4-ab11-90cd776ff434" />




## RESULT
Thus the sql queries for DDL Commands have been executed successfully.
