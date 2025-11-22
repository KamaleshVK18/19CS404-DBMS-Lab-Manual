# Experiment 10: PL/SQL – Triggers

## AIM
To write and execute PL/SQL trigger programs for automating actions in response to specific table events like INSERT, UPDATE, or DELETE.

---

## THEORY

A **trigger** is a stored PL/SQL block that is automatically executed or fired when a specified event occurs on a table or view. Triggers can be used for enforcing business rules, auditing changes, or automatic updates.

### Types of Triggers:
- **Before Trigger**: Executes before the operation (INSERT, UPDATE, DELETE).
- **After Trigger**: Executes after the operation.
- **Row-level Trigger**: Executes for each affected row.
- **Statement-level Trigger**: Executes once for the triggering statement.

**Basic Syntax:**
```sql
CREATE OR REPLACE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE ON table_name
[FOR EACH ROW]
BEGIN
   -- trigger logic
END;
```

## 1. Write a trigger to log every insertion into a table.
**Steps:**
- Create two tables: `employees` (for storing data) and `employee_log` (for logging the inserts).
- Write an **AFTER INSERT** trigger on the `employees` table to log the new data into the `employee_log` table.
## Program:
```
*Steps:*
- Create two tables: employees (for storing data) and employee_log (for logging the inserts).
- Write an *AFTER INSERT* trigger on the employees table to log the new data into the employee_log table.

*Program:*
sql
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    salary NUMBER
);

CREATE TABLE employee_log (
    log_id NUMBER PRIMARY KEY,
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    salary NUMBER,
    log_date DATE
);

CREATE SEQUENCE log_seq START WITH 1 INCREMENT BY 1;

CREATE OR REPLACE TRIGGER trg_after_insert_employees
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_log (log_id, emp_id, emp_name, salary, log_date)
    VALUES (log_seq.NEXTVAL, :NEW.emp_id, :NEW.emp_name, :NEW.salary, SYSDATE);
END;
/

INSERT INTO employees (emp_id, emp_name, salary) VALUES (1, 'Alice', 5000);
COMMIT;

SELECT * FROM employee_log;
```
**Expected Output:**
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.
## Output:
<img width="1263" height="249" alt="image" src="https://github.com/user-attachments/assets/ee755fd1-31f2-4dbb-9bee-aaae0305a854" />

---
## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.
## Program
```
*Steps:*
- Write a *BEFORE DELETE* trigger on the sensitive_data table.
- Use RAISE_APPLICATION_ERROR to prevent deletion and issue a custom error message.

*Program:*
sql
CREATE TABLE sensitive_data (
    id NUMBER PRIMARY KEY,
    info VARCHAR2(100)
);

CREATE OR REPLACE TRIGGER trg_prevent_delete_sensitive
BEFORE DELETE ON sensitive_data
FOR EACH ROW
BEGIN
    RAISE_APPLICATION_ERROR(-20001, 'Deletion not allowed on this table.');
END;
/

INSERT INTO sensitive_data (id, info) VALUES (1, 'Secret Info');
COMMIT;

DELETE FROM sensitive_data WHERE id = 1;
```
**Expected Output:**
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`
## Output:
<img width="1271" height="223" alt="image" src="https://github.com/user-attachments/assets/fbd833b5-da85-49de-b3e6-ef7f5483b54c" />

---

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.
## Program:
```
CREATE TABLE products (
    product_id     NUMBER PRIMARY KEY,
    product_name   VARCHAR2(100),
    price          NUMBER(10, 2),
    quantity       NUMBER
);

INSERT INTO products (product_id, product_name, price, quantity)
VALUES (101, 'Laptop', 850.00, 10);

INSERT INTO products (product_id, product_name, price, quantity)
VALUES (102, 'Mouse', 25.50, 100);


CREATE OR REPLACE TRIGGER trg_update_last_modified
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    :NEW.last_modified := SYSDATE;
END;
/
-- Update a product
UPDATE products
SET product_name = 'New Name'
WHERE product_id = 101;

-- Check the last_modified column
SELECT product_id, product_name, last_modified
FROM products
WHERE product_id = 101;
```
**Expected Output:**
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.
## Output:
<img width="1258" height="300" alt="image" src="https://github.com/user-attachments/assets/d84f6f36-759d-4bb5-98cd-651f5bfbc602" />

---

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.
## Program:
```
CREATE TABLE customer_orders (
    order_id     NUMBER PRIMARY KEY,
    customer_id  NUMBER,
    order_date   DATE,
    total_amount NUMBER
);
CREATE TABLE audit_log (
    table_name     VARCHAR2(50) PRIMARY KEY,
    update_count   NUMBER DEFAULT 0
);
INSERT INTO audit_log (table_name, update_count)
VALUES ('CUSTOMER_ORDERS', 0);
CREATE OR REPLACE TRIGGER trg_count_updates_customer_orders
AFTER UPDATE ON customer_orders
BEGIN
    UPDATE audit_log
    SET update_count = update_count + 1
    WHERE table_name = 'CUSTOMER_ORDERS';
END;
/
-- Update a row in customer_orders
UPDATE customer_orders
SET total_amount = total_amount + 10
WHERE order_id = 1;

-- Check the update count
SELECT * FROM audit_log;
```

**Expected Output:**
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.
## Output:
<img width="1260" height="266" alt="image" src="https://github.com/user-attachments/assets/c411a06a-2331-4562-82db-65774e6285c0" />

---

## 5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.
## Program:
```
CREATE TABLE employee (
    emp_id     NUMBER PRIMARY KEY,
    emp_name   VARCHAR2(100),
    salary     NUMBER
);
CREATE OR REPLACE TRIGGER trg_check_min_salary
BEFORE INSERT ON employee
FOR EACH ROW
BEGIN
    IF :NEW.salary < 3000 THEN
        RAISE_APPLICATION_ERROR(-20002, 'ERROR: Salary below minimum threshold.');
    END IF;
END;
/
INSERT INTO employee (emp_id, emp_name, salary)
VALUES (1, 'John Doe', 2500);

INSERT INTO employee (emp_id, emp_name, salary)
VALUES (2, 'Jane Smith', 4000);
```
**Expected Output:**
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`
## Output:
<img width="1018" height="302" alt="image" src="https://github.com/user-attachments/assets/d0b6d677-b089-4e4e-82c8-e6bb0e3ef391" />

## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.


