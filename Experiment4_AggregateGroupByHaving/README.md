# Experiment 4: Aggregate Functions, Group By and Having Clause

## AIM
To study and implement aggregate functions, GROUP BY, and HAVING clause with suitable examples.

## THEORY

### Aggregate Functions
These perform calculations on a set of values and return a single value.

- **MIN()** – Smallest value  
- **MAX()** – Largest value  
- **COUNT()** – Number of rows  
- **SUM()** – Total of values  
- **AVG()** – Average of values

**Syntax:**
```sql
SELECT AGG_FUNC(column_name) FROM table_name WHERE condition;
```
### GROUP BY
Groups records with the same values in specified columns.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name;
```
### HAVING
Filters the grouped records based on aggregate conditions.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

**Question 1**
--
--What is the most common time slot for appointments for each doctor?

```sql
--
Select DoctorID,
       SUBSTR(CAST(AppointmentDateTime AS CHAR),12,5) AS TimeSlot,
       Count(*) AS TotalAppointments
From Appointments
Group By DoctorID,Timeslot
Order By TotalAppointments DESC;
       
```

**Output:**

<img width="855" height="255" alt="image" src="https://github.com/user-attachments/assets/d6d2a172-037e-4de1-8c19-cb1b203545ef" />


**Question 2**
---
-- How many male and female doctors are there in each medical specialty?

```sql
--
Select Specialty,Gender,COUNT(*) AS TotalDoctors
from Doctors
Group By Specialty,Gender;
```

**Output:**

<img width="894" height="254" alt="image" src="https://github.com/user-attachments/assets/ceb48d66-14a8-46d3-9651-03f96f283a11" />


**Question 3**
---
-- How many medical records does each doctor have?

```sql
--
Select DoctorID,
       COUNT(RecordID) AS TotalRecords
from MedicalRecords
Group BY DoctorID;
```

**Output:**

<img width="565" height="236" alt="image" src="https://github.com/user-attachments/assets/94662d3f-6b5f-4112-84b4-975026553088" />


**Question 4**
---
-- Write a SQL query that counts the number of unique salespeople. Return number of salespeople.

```sql
--
Select COUNT(DISTINCT salesman_id) AS COUNT
from orders;
```

**Output:**

<img width="304" height="224" alt="image" src="https://github.com/user-attachments/assets/698bc1f2-7cc4-4bca-b9a0-5f1f95735ee8" />


**Question 5**
---
-- Write a SQL query to find the total income of employees aged 40 or above.


```sql
--
Select SUM(income) as total_income
from employee
Where age>=40;
```

**Output:**

<img width="343" height="219" alt="image" src="https://github.com/user-attachments/assets/b383fa0f-54bc-43b8-b9b2-618bacfd4d81" />


**Question 6**
---
--Write a SQL query to Calculate the average income of the employees with names starting with 'A': 

```sql
--
 Select Avg(income) AS avg_income
from employee
Where name LIKE 'A%';
```

**Output:**

<img width="302" height="224" alt="image" src="https://github.com/user-attachments/assets/8ae84e38-5ce3-41b6-867f-7b70a3756a86" />


**Question 7**
---
-- Write a SQL query to find the total number of unique cities in the customer table?

```sql
--
Select COUNT(DISTINCT city) AS unique_cities
from customer;
```

**Output:**

<img width="364" height="226" alt="image" src="https://github.com/user-attachments/assets/3d978bcb-e13b-4c00-9111-e2b3e3895ffc" />


**Question 8**
---
-- Write the SQL query that accomplishes the grouping of data by joining date (jdate), calculates the average work hours for each date, and excludes dates where the average work hour is not less than 10.

```sql
--
Select jdate,
       AVG(workhour)
from employee1
Group by jdate
Having AVG(workhour)<10;
```

**Output:**

<img width="576" height="247" alt="image" src="https://github.com/user-attachments/assets/dad79ff2-92fb-4bb9-8252-c1efb9021b85" />


**Question 9**
---
-- Write the SQL query that accomplishes the selection of number of products for each category from products table which includes only those products where the category ID is greater than 2.

```sql
--
Select category_id,
       Count(category_id) AS COUNT
from products
Group by category_id
Having category_id>2;
```

**Output:**

<img width="537" height="245" alt="image" src="https://github.com/user-attachments/assets/59584926-e290-4e39-a04c-ce763f53e926" />


**Question 10**
---
-- Write the SQL query that achieves the grouping of data by city, calculates the total income for each city, and includes only those cities where the total income sum is greater than 200,000.

```sql
--
Select city,
       SUM(income) AS Income
From employee
Group by city
Having SUM(income)>200000;
```

**Output:**

<img width="517" height="233" alt="image" src="https://github.com/user-attachments/assets/fa38070f-c364-47df-9769-59a0ad52b02c" />

## MARKS:
<img width="1913" height="1131" alt="image" src="https://github.com/user-attachments/assets/3a34e7ec-c861-47a2-b54e-77d89fc09109" />


## RESULT
Thus, the SQL queries to implement aggregate functions, GROUP BY, and HAVING clause have been executed successfully.
