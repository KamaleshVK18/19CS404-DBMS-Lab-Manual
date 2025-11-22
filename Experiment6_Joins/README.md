# Experiment 6: Joins

## AIM
To study and implement different types of joins.

## THEORY

SQL Joins are used to combine records from two or more tables based on a related column.

### 1. INNER JOIN
Returns records with matching values in both tables.

**Syntax:**
```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

### 2. LEFT JOIN
Returns all records from the left table, and matched records from the right.

**Syntax:**

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;
```
### 3. RIGHT JOIN
Returns all records from the right table, and matched records from the left.

**Syntax:**

```sql
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;
```
### 4. FULL OUTER JOIN
Returns all records when there is a match in either left or right table.

**Syntax:**

```sql
SELECT columns
FROM table1
FULL OUTER JOIN table2
ON table1.column = table2.column;
```

**Question 1**
--
-- Write the SQL query that achieves the selection of all columns from the "patients" table and the specialization from the "doctors" table (aliased as "doctor_specialization"), with an inner join on the "doctor_id" column.

```sql
select
p.*,
d.specialization As doctor_specialization
from patients p
Inner join
doctors d on p.doctor_id=d.doctor_id;
```

**Output:**

<img width="1291" height="342" alt="image" src="https://github.com/user-attachments/assets/7b9d6ad0-7c4b-47f9-86fd-893069476aff" />


**Question 2**
---
-- From the following tables write a SQL query to find the details of an order. Return ord_no, ord_date, purch_amt, Customer Name, grade, Salesman, commission. 

```sql
SELECT 
    o.ord_no,
    o.ord_date,
    o.purch_amt,
    c.cust_name AS "Customer Name",
    c.grade,
    s.name AS Salesman,
    s.commission
FROM 
    orders o
INNER JOIN 
    customer c ON o.customer_id = c.customer_id
INNER JOIN 
    salesman s ON o.salesman_id = s.salesman_id;
```

**Output:**

<img width="1298" height="844" alt="image" src="https://github.com/user-attachments/assets/bf8ec2c0-8004-4606-ae13-25dd39872981" />


**Question 3**
---
-- From the following tables write a SQL query to locate those salespeople who do not live in the same city where their customers live and have received a commission of more than 12% from the company. Return Customer Name, customer city, Salesman, salesman city, commission

```sql
SELECT 
    c.cust_name AS "Customer Name",
    c.city AS "city",
    s.name AS "Salesman",
    s.city AS "city",
    s.commission
FROM 
    customer c
INNER JOIN 
    salesman s ON c.salesman_id = s.salesman_id
WHERE 
    c.city <> s.city
    AND s.commission > 0.12;
```

**Output:**

<img width="1149" height="401" alt="image" src="https://github.com/user-attachments/assets/f71333ee-9b0c-4bdf-9ade-0ac827f29abb" />


**Question 4**
---
Write the SQL query that achieves the selection of the "nurse_id" from the "nurses" table (aliased as "n") and the "department_name" from the "departments" table, with an inner join on the "department_id" column and conditions filtering for a nurse with the first name 'David' and last name 'Moore'.



```sql
SELECT 
    n.nurse_id,
    d.department_name
FROM 
    nurses n
INNER JOIN 
    departments d ON n.department_id = d.department_id
WHERE 
    n.first_name = 'David'
    AND n.last_name = 'Moore';
```

**Output:**

<img width="528" height="231" alt="image" src="https://github.com/user-attachments/assets/c67626ef-eb26-495d-ba63-cbef0c38ddf3" />


**Question 5**
---
From the following tables write a SQL query to display the customer name, customer city, grade, salesman, salesman city. The results should be sorted by ascending customer_id.  

```sql
SELECT 
    c.cust_name, 
    c.city,
    c.grade,
    s.name as "Salesman",
    s.city
FROM 
    customer c
JOIN 
    salesman s
ON 
    c.salesman_id = s.salesman_id
ORDER BY 
    c.customer_id ASC;

```

**Output:**

<img width="1144" height="228" alt="image" src="https://github.com/user-attachments/assets/aa3424ea-f2d2-4cac-919a-0a5ef735c413" />


**Question 6**
---
Write the SQL query that achieves the selection of the "name" column from the "salesman" table (aliased as "salesman_name") and the "cust_name" column from the "customer" table (aliased as "customer_name"), with a left join on the "salesman_id" column.

```sql
SELECT 
    s.name AS salesman_name,
    c.cust_name AS customer_name
FROM 
    salesman s
LEFT JOIN 
    customer c ON s.salesman_id = c.salesman_id;
```

**Output:**

<img width="527" height="225" alt="image" src="https://github.com/user-attachments/assets/6c76a479-80cf-4386-be73-2649d5a04541" />


**Question 7**
---
Write an SQL query to retrieve all columns from the 'customer' table (aliased as 'c') using a LEFT JOIN with the 'orders' table on the 'customer_id' column, and filter the results to include only those orders placed between '2012-07-01' and '2012-07-30'.

```sql
SELECT 
    c.*
FROM 
    customer c
LEFT JOIN 
    orders o ON c.customer_id = o.customer_id
WHERE 
    o.ord_date BETWEEN '2012-07-01' AND '2012-07-30';
    
```

**Output:**

<img width="1164" height="235" alt="image" src="https://github.com/user-attachments/assets/53511898-059d-40a9-9dc0-b992a2d3085b" />


**Question 8**
---
-- Write a SQL statement to join the tables salesman, customer and orders so that the same column of each table appears once and only the relational rows are returned. 

```sql
 SELECT 
    o.ord_no,
    o.purch_amt,
    o.ord_date,
    c.cust_name,
    c.city AS customer_city,
    c.grade,
    s.name AS salesman_name,
    s.city AS salesman_city,
    s.commission
FROM 
    orders o
INNER JOIN 
    customer c ON o.customer_id = c.customer_id
INNER JOIN 
    salesman s ON o.salesman_id = s.salesman_id;
```

**Output:**
<img width="1282" height="303" alt="image" src="https://github.com/user-attachments/assets/92459aaa-a81d-4c8b-87e2-87fe9670c24a" />

**Question 9**
---
--Write the SQL query that achieves the selection of all columns from the "patients" table, with an inner join on the "doctor_id" column, and includes a condition filtering for patients whose doctors have the first name 'John' and last name 'Smith'.

```sql
 SELECT 
    p.*
FROM 
    patients p
INNER JOIN 
    doctors d
ON 
    p.doctor_id = d.doctor_id
WHERE 
    d.first_name = 'John'
    AND d.last_name = 'Smith';

```

**Output:**

<img width="1290" height="231" alt="image" src="https://github.com/user-attachments/assets/3a528903-5a66-49e3-9b42-da6e5d562fe3" />


**Question 10**
---
 From the following tables write a SQL query to find the salesperson(s) and the customer(s) he represents. Return Customer Name, city, Salesman, commission.

```sql
SELECT 
    c.cust_name AS "Customer Name",
    c.city,
    s.name AS "Salesman",
    s.commission
FROM 
    customer c
INNER JOIN 
    salesman s ON c.salesman_id = s.salesman_id;
```

**Output:**

<img width="989" height="224" alt="image" src="https://github.com/user-attachments/assets/ac20762f-3bba-4476-957a-39c5928971c3" />


## MARKS:

<img width="1913" height="1131" alt="image" src="https://github.com/user-attachments/assets/81fc736c-6b34-4130-8e98-4facb278bc27" />




## RESULT
Thus, the SQL queries to implement different types of joins have been executed successfully.
