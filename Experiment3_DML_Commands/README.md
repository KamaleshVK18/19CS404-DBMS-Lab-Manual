# Experiment 3: DML Commands

## AIM
To study and implement DML (Data Manipulation Language) commands.

## THEORY

### 1. INSERT INTO
Used to add records into a relation.
These are three type of INSERT INTO queries which are as
A)Inserting a single record
**Syntax (Single Row):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES (value_1, value_2, ...);
```
**Syntax (Multiple Rows):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES
(value_1, value_2, ...),
(value_3, value_4, ...);
```
**Syntax (Insert from another table):**
```sql
INSERT INTO table_name SELECT * FROM other_table WHERE condition;
```
### 2. UPDATE
Used to modify records in a relation.
Syntax:
```sql
UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
```
### 3. DELETE
Used to delete records from a relation.
**Syntax (All rows):**
```sql
DELETE FROM table_name;
```
**Syntax (Specific condition):**
```sql
DELETE FROM table_name WHERE condition;
```
### 4. SELECT
Used to retrieve records from a table.
**Syntax:**
```sql
SELECT column1, column2 FROM table_name WHERE condition;
```
**Question 1**
--
--Write a SQL statement to Update the grade of all customers in Chennai city as  5. 

```sql
UPDATE customer
SET grade=5
WHERE city = 'Chennai';
```

**Output:**

<img width="963" height="302" alt="image" src="https://github.com/user-attachments/assets/3dc25124-e362-4ddd-aa7e-8fa2c0f3ac12" />


**Question 2**
---
--Write a SQL statement to Increase the selling price by 10% for all products in the 'Bakery' category in the products table.

```sql

UPDATE products
SET sell_price = sell_price*1.10
WHERE category = 'Bakery';
```

**Output:**

<img width="1245" height="339" alt="image" src="https://github.com/user-attachments/assets/0ad7d11e-9c52-4a4b-bc6c-6861e2af8702" />


**Question 3**
---
-- Update the 'Selling_Price' to add 10% extra margin for all products supplied by the supplier with id 6.

```sql
UPDATE products
SET sell_price = ROUND(sell_price*1.10)
WHERE supplier_id=6;
```

**Output:**

<img width="1186" height="358" alt="image" src="https://github.com/user-attachments/assets/7743dc76-b7fa-4ebe-8801-a0791697a758" />


**Question 4**
---
--Write a SQL statement to Double the salary for employees in department 20 who have a job_id ending with 'MAN'

```sql
UPDATE employees
SET salary=salary*2
WHERE department_id=20
AND job_id LIKE '%MAN';
```

**Output:**

<img width="917" height="204" alt="image" src="https://github.com/user-attachments/assets/facc06bf-152e-4827-93d6-e66c7ed89c17" />


**Question 5**
---
--Write a SQL query to Delete All Doctors with a NULL Specialization

```sql
DELETE FROM doctors
WHERE specialization IS NULL;
```

**Output:**

<img width="934" height="253" alt="image" src="https://github.com/user-attachments/assets/3c18d65c-0100-4c2e-b726-bfff52846e09" />

**Question 6**
---
--Write a SQL query to remove rows from the table 'customer' with the following condition -

1. 'cust_city' should begin with the letter 'L',
```sql
DELETE FROM customer
WHERE cust_city LIKE 'L%';
```

**Output:**

<img width="1264" height="735" alt="image" src="https://github.com/user-attachments/assets/2b5f37d3-7e0f-4b7a-9e42-d82c241b29f8" />



**Question 7**
---
-- Write a SQL query to delete a doctor from Doctors table whose Specialization is 'Pediatrics' and First name is 'Michael'.

```sql
DELETE FROM doctors
WHERE specialization = 'Pediatrics'
AND first_name = 'Michael';
```

**Output:**

<img width="992" height="221" alt="image" src="https://github.com/user-attachments/assets/7254fda2-857a-47b3-ad5a-8f8e153a4103" />


**Question 8**
---
-- Write a query to fetch details of all employees excluding the employees with first names, “Sanjay” and “Sonia” from the EmployeeInfo table.

```sql
SELECT * FROM EmployeeInfo
WHERE EmpFname NOT IN ('Sanjay','Sonia');
```

**Output:**

<img width="1224" height="146" alt="image" src="https://github.com/user-attachments/assets/adc6bf77-938c-4151-8119-f26808954bbc" />


**Question 9**
---
-- Write a query to fetch all the records from the EmployeeInfo table ordered by EmpLname in descending order and Department in the ascending order.


```sql
 SELECT * FROM EMPLOYEEInfo
ORDER BY EmpFname DESC , Department ASC;
```

**Output:**

<img width="1240" height="182" alt="image" src="https://github.com/user-attachments/assets/1af03fda-3d5c-4374-9d48-5a42957c7c2f" />


**Question 10**
---
--Write a SQL query to determine the age group of value1 in the Calculations table as 'Child' if it is less than 13, 'Teen' if it is between 13 and 19, and 'Adult' if it is 20 or older.

```sql
SELECT 
     id,
     value1,
     CASE
         WHEN value1 < 13 THEN 'Child'
         WHEN value1 BETWEEN 13 AND 19 THEN 'Teen'
         ELSE 'Adult'
     END AS age_group
FROM Calculations;
```

**Output:**

<img width="596" height="229" alt="image" src="https://github.com/user-attachments/assets/49e9298f-c257-4370-b7b3-e8c9c5c6f763" />


## MARKS:

<img width="1915" height="523" alt="image" src="https://github.com/user-attachments/assets/d67405f8-9222-40f4-bf28-1f6996f41262" />


## RESULT
Thus, the SQL queries to implement DML commands have been executed successfully.
