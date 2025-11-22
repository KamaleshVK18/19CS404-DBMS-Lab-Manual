# Experiment 8: PL/SQL Cursor Programs

## AIM
To write and execute PL/SQL programs using cursors and exception handling to manage runtime errors effectively and display appropriate messages.

## THEORY

In PL/SQL, cursors are used to handle query result sets row-by-row. 

There are two types of cursors:

- Implicit Cursors: Automatically created by PL/SQL for single-row queries.
- Explicit Cursors: Declared and controlled by the programmer for multi-row queries.

Types of Explicit Cursors:

1. Simple Cursor: Basic cursor to iterate over multiple rows.

2. Parameterized Cursor: Accepts parameters to filter the result dynamically.

3. Cursor FOR Loop: Simplifies cursor operations (open, fetch, close).

4. %ROWTYPE Cursor: Fetches entire row into a record using %ROWTYPE.

5. Cursor with FOR UPDATE: Used for row-level locking and updating the rows while looping.

**Syntax:**
```sql
DECLARE 
   <declarations section> 
BEGIN 
   <executable command(s)>
EXCEPTION 
   <exception handling> 
END;
```

### Basic Components of PL/SQL Block:

- DECLARE: Section to declare variables and constants.
- BEGIN: The execution section that contains PL/SQL statements.
- EXCEPTION: Handles errors or exceptions that occur in the program.
- END: Marks the end of the PL/SQL block.

**Exception Handling**

PL/SQL provides a robust mechanism to handle runtime errors using exception handling blocks. When an error occurs during execution, control is passed to the EXCEPTION section, where specific or general errors can be handled gracefully.

### Components of Exception Handling:
- Predefined Exceptions: Automatically raised by PL/SQL for common errors (e.g., NO_DATA_FOUND, TOO_MANY_ROWS, ZERO_DIVIDE).
- User-defined Exceptions: Declared explicitly in the declaration section using the EXCEPTION keyword.
- WHEN OTHERS: A generic handler for all exceptions not handled explicitly.

```sql
BEGIN
   -- Statements
EXCEPTION
   WHEN exception_name THEN
      -- Handling code
   WHEN OTHERS THEN
      -- Handling for unknown errors
END;
```

### **Question 1: Simple Cursor with Exception Handling**

**Write a PL/SQL program using a simple cursor to fetch employee names and designations from the `employees` table. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: When no rows are fetched.
2. **OTHERS**: Any other unexpected errors during execution.

**Steps:**

- Create an `employees` table with fields `emp_id`, `emp_name`, and `designation`.
- Insert some sample data into the table.
- Use a simple cursor to fetch and display employee names and designations.
- Implement exception handling to catch the relevant exceptions and display appropriate messages.
## Program:
```
BEGIN
    EXECUTE IMMEDIATE 'DROP TABLE employees';
EXCEPTION
    WHEN OTHERS THEN NULL; -- Ignore error if table doesn't exist
END;
/

CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50)
);
BEGIN
    INSERT INTO employees VALUES (1, 'Alice', 'Manager');
    INSERT INTO employees VALUES (2, 'Bob', 'Developer');
    INSERT INTO employees VALUES (3, 'Charlie', 'Tester');
    COMMIT;
END;
/
DECLARE
    CURSOR emp_cursor IS
        SELECT emp_name, designation FROM employees;

    v_name employees.emp_name%TYPE;
    v_desg employees.designation%TYPE;

    no_data BOOLEAN := TRUE;
BEGIN
    OPEN emp_cursor;
    LOOP
        FETCH emp_cursor INTO v_name, v_desg;
        EXIT WHEN emp_cursor%NOTFOUND;
        
        no_data := FALSE;
        DBMS_OUTPUT.PUT_LINE('Name: ' || v_name || ', Designation: ' || v_desg);
    END LOOP;
    CLOSE emp_cursor;

    IF no_data THEN
        RAISE NO_DATA_FOUND;
    END IF;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employee data found.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('An unexpected error occurred: ' || SQLERRM);
END;
/
```
**Output:**  
<img width="331" height="187" alt="image" src="https://github.com/user-attachments/assets/f1887dce-b997-4cb3-a752-b0645efc8a93" />

---

### **Question 2: Parameterized Cursor with Exception Handling**

**Write a PL/SQL program using a parameterized cursor to retrieve and display employees with a salary in a given range. Implement exception handling for the following errors:**

1. **NO_DATA_FOUND**: When no employees meet the salary criteria.
2. **OTHERS**: For any unexpected errors during the execution.

**Steps:**

- Modify the `employees` table by adding a `salary` column.
- Insert sample salary values for the employees.
- Use a parameterized cursor to accept a salary range as input and fetch employees within that range.
- Implement exception handling to catch and display relevant error messages.
## Program:
```

BEGIN
    EXECUTE IMMEDIATE 'DROP TABLE employees';
EXCEPTION
    WHEN OTHERS THEN
        NULL;  -- Ignore if table does not exist
END;
/
  
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50),
    salary NUMBER
);

INSERT INTO employees VALUES (1, 'Alice', 'Manager', 60000);
INSERT INTO employees VALUES (2, 'Bob', 'Developer', 45000);
INSERT INTO employees VALUES (3, 'Charlie', 'Tester', 50000);
COMMIT;

SET SERVEROUTPUT ON;

DECLARE
    v_min_salary NUMBER := 40000;  -- Minimum salary input
    v_max_salary NUMBER := 55000;  -- Maximum salary input

    CURSOR emp_cursor(p_min NUMBER, p_max NUMBER) IS
        SELECT emp_name, designation, salary
        FROM employees
        WHERE salary BETWEEN p_min AND p_max;

    no_data BOOLEAN := TRUE;

BEGIN
    FOR emp_rec IN emp_cursor(v_min_salary, v_max_salary) LOOP
        DBMS_OUTPUT.PUT_LINE('Name: ' || emp_rec.emp_name ||
                             ', Designation: ' || emp_rec.designation ||
                             ', Salary: ' || emp_rec.salary);
        no_data := FALSE;
    END LOOP;

    IF no_data THEN
        RAISE NO_DATA_FOUND;
    END IF;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in the given salary range.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```
**Output:**  
<img width="432" height="160" alt="image" src="https://github.com/user-attachments/assets/8bb9772e-775c-4816-96e3-55a9e3ec9b05" />


---

### **Question 3: Cursor FOR Loop with Exception Handling**

**Write a PL/SQL program using a cursor FOR loop to retrieve and display all employee names and their department numbers from the `employees` table. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: If no employees are found in the database.
2. **OTHERS**: For any other unexpected errors.

**Steps:**

- Modify the `employees` table by adding a `dept_no` column.
- Insert sample department numbers for employees.
- Use a cursor FOR loop to fetch and display employee names along with their department numbers.
- Implement exception handling to catch the relevant exceptions.
## Program:
```
BEGIN
    EXECUTE IMMEDIATE 'DROP TABLE employees';
EXCEPTION
    WHEN OTHERS THEN
        NULL;
END;
/

CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50),
    salary NUMBER,
    dept_no NUMBER
);

INSERT INTO employees VALUES (1, 'Alice', 'Manager', 60000, 10);
INSERT INTO employees VALUES (2, 'Bob', 'Developer', 45000, 20);
INSERT INTO employees VALUES (3, 'Charlie', 'Tester', 50000, 10);
COMMIT;

SET SERVEROUTPUT ON;

DECLARE
    no_data BOOLEAN := TRUE;
BEGIN
    FOR emp_rec IN (SELECT emp_name, dept_no FROM employees) LOOP
        DBMS_OUTPUT.PUT_LINE('Employee: ' || emp_rec.emp_name || ', Dept No: ' || emp_rec.dept_no);
        no_data := FALSE;
    END LOOP;

    IF no_data THEN
        RAISE NO_DATA_FOUND;
    END IF;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in the database.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```
**Output:**  
<img width="366" height="200" alt="image" src="https://github.com/user-attachments/assets/d0cf0a61-90e8-4d7a-949a-e92a27c48079" />


---

### **Question 4: Cursor with `%ROWTYPE` and Exception Handling**

**Write a PL/SQL program that uses a cursor with `%ROWTYPE` to fetch and display complete employee records (emp_id, emp_name, designation, salary). Implement exception handling for the following errors:**

1. **NO_DATA_FOUND**: When no employees are found in the database.
2. **OTHERS**: For any other errors that occur.

**Steps:**

- Modify the `employees` table by adding `emp_id`, `emp_name`, `designation`, and `salary` fields.
- Insert sample data into the `employees` table.
- Declare a cursor using `%ROWTYPE` to fetch complete rows from the `employees` table.
- Implement exception handling to catch the relevant exceptions and display appropriate messages.
## Program:
```
BEGIN
    EXECUTE IMMEDIATE 'DROP TABLE employees';
EXCEPTION
    WHEN OTHERS THEN
        NULL;
END;
/

CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50),
    salary NUMBER
);

INSERT INTO employees VALUES (1, 'Alice', 'Manager', 60000);
INSERT INTO employees VALUES (2, 'Bob', 'Developer', 45000);
INSERT INTO employees VALUES (3, 'Charlie', 'Tester', 50000);
COMMIT;

SET SERVEROUTPUT ON;

DECLARE
    CURSOR emp_cur IS
        SELECT * FROM employees;

    emp_rec emp_cur%ROWTYPE;

    no_data BOOLEAN := TRUE;
BEGIN
    OPEN emp_cur;
    LOOP
        FETCH emp_cur INTO emp_rec;
        EXIT WHEN emp_cur%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE('Emp ID: ' || emp_rec.emp_id ||
                             ', Name: ' || emp_rec.emp_name ||
                             ', Designation: ' || emp_rec.designation ||
                             ', Salary: ' || emp_rec.salary);

        no_data := FALSE;
    END LOOP;
    CLOSE emp_cur;

    IF no_data THEN
        RAISE NO_DATA_FOUND;
    END IF;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in the database.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```
**Output:**  
<img width="537" height="192" alt="image" src="https://github.com/user-attachments/assets/3e1193db-29d2-44eb-8270-9382f762e328" />


---

### **Question 5: Cursor with FOR UPDATE Clause and Exception Handling**

**Write a PL/SQL program using a cursor with the `FOR UPDATE` clause to update the salary of employees in a specific department. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: If no rows are affected by the update.
2. **OTHERS**: For any unexpected errors during execution.

**Steps:**

- Modify the `employees` table to include a `dept_no` and `salary` field.
- Insert sample data into the `employees` table with different department numbers.
- Use a cursor with the `FOR UPDATE` clause to lock the rows of employees in a specific department and update their salary.
- Implement exception handling to handle `NO_DATA_FOUND` or other errors that may occur.
## Program:
```
-- Step 1: Drop table if exists (optional)
BEGIN
    EXECUTE IMMEDIATE 'DROP TABLE employees';
EXCEPTION
    WHEN OTHERS THEN NULL; -- ignore if not exists
END;
/

-- Step 2: Create employees table with dept_no and salary
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50),
    dept_no NUMBER,
    salary NUMBER
);

-- Step 3: Insert sample data
INSERT INTO employees VALUES (1, 'Alice', 'Manager', 10, 60000);
INSERT INTO employees VALUES (2, 'Bob', 'Developer', 20, 45000);
INSERT INTO employees VALUES (3, 'Charlie', 'Tester', 10, 50000);
INSERT INTO employees VALUES (4, 'Diana', 'Analyst', 30, 55000);
COMMIT;

-- Step 4: Enable output
SET SERVEROUTPUT ON;

-- Step 5: PL/SQL block with cursor FOR UPDATE to update salary in a given dept_no
DECLARE
    v_dept_no NUMBER := 10;           -- Department number to update
    v_increment NUMBER := 5000;       -- Salary increment amount

    CURSOR emp_cur IS
        SELECT emp_id, emp_name, salary
        FROM employees
        WHERE dept_no = v_dept_no
        FOR UPDATE OF salary NOWAIT;

    emp_rec emp_cur%ROWTYPE;
    rows_updated NUMBER := 0;
BEGIN
    OPEN emp_cur;
    LOOP
        FETCH emp_cur INTO emp_rec;
        EXIT WHEN emp_cur%NOTFOUND;

        -- Update salary by increment
        UPDATE employees
        SET salary = salary + v_increment
        WHERE CURRENT OF emp_cur;

        rows_updated := rows_updated + 1;

        DBMS_OUTPUT.PUT_LINE('Updated salary for ' || emp_rec.emp_name || ' to ' || (emp_rec.salary + v_increment));
    END LOOP;
    CLOSE emp_cur;

    IF rows_updated = 0 THEN
        RAISE NO_DATA_FOUND;
    ELSE
        DBMS_OUTPUT.PUT_LINE(rows_updated || ' employee(s) updated in department ' || v_dept_no);
    END IF;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in department ' || v_dept_no);
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```
**Output:**  
<img width="381" height="196" alt="image" src="https://github.com/user-attachments/assets/850fba9b-07f0-4d98-ba4d-ee384dac88ae" />

---

## RESULT
Thus, the program successfully executed and displayed employee details using a cursor. 

