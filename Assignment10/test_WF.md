
CREATE TABLE employees (
    emp_id     INT PRIMARY KEY,
    name       VARCHAR(100),
    department VARCHAR(50),
    salary     DECIMAL(10,2),
    hire_date  DATE,
    city       VARCHAR(50)
);

---
``` sql
INSERT INTO employees VALUES
(1,  'Aarav Shah',     'Engineering', 92000, '2019-03-15', 'Mumbai'),
(2,  'Bhavna Reddy',   'Marketing',   67000, '2021-07-01', 'Delhi'),
(3,  'Chirag Malhotra','Engineering', 115000,'2017-11-20', 'Mumbai'),
(4,  'Disha Kapoor',   'HR',          54000, '2022-06-14', 'Bangalore'),
(5,  'Eshan Joshi',    'Marketing',   71000, '2020-09-01', 'Pune'),
(6,  'Fiona Nair',     'Engineering', 88000, '2021-02-28', 'Mumbai'),
(7,  'Gaurav Pillai',  'HR',          61000, '2020-10-15', 'Delhi'),
(8,  'Hina Patel',     'Marketing',   67000, '2022-08-22', 'Pune'),
(9,  'Ishaan Roy',     'Engineering', 92000, '2020-06-11', 'Bangalore'),
(10, 'Jay Menon',      'HR',          58000, '2019-02-07', 'Mumbai');
 
SELECT * FROM employees ORDER BY department, salary DESC;

```

## -- Q1: Aarav and Ishaan both earn ₹92,000 in Engineering.
## -- Write a query that shows each employee's name, department, salary,
## -- their RANK and DENSE_RANK within their department ordered by salary descending.
## -- Show only the Engineering department.

``` sql
Select name, department, salary, Rank()  over(partition by department order by salary desc) as rn, Dense_Rank() over(partition by department order by salary desc) as drn from employees where department = 'Engineering';
```
---

## -- Q2: The HR team wants to see employees listed 3 per page.
## -- Write a query that assigns a row number to all employees
## -- ordered by salary descending, then shows only page 2
## -- (rows 4, 5 and 6)

``` sql
Select name, department, salary, Row_Number()  over(order by salary desc) as rn from employees limit 3 offset 3;
```

## -- Q3: Management wants to know how each employee's salary compares
## -- to the person hired just before them in the same department
## -- (ordered by hire_date ascending).
## -- Write a query showing: name, department, hire_date, salary,
## -- the previous employee's salary in the same department,
## -- and the difference between current and previous salary.
## -- Show NULL where there is no previous employee.

``` sql
Select name, department, hire_date, salary, lag(salary) over( order by hire_date) as prev_salary, salary - lag(salary) over( order by hire_date) as sal_diff from employees;
```


## -- Q4: For each employee show their name, department, salary,
## -- and the salary of the next higher-paid person in the same department
## -- (ordered by salary ascending).
## -- Also show how much more the next person earns.
## -- If there is no next person show 0 for the difference

```sql
Select name, department, salary, lead(salary) over (partition by department order by salary), coalesce(lead(salary) over (partition by department order by salary) - salary,0) as salary_diff from employees;
```

## -- Q5: Write a single query that shows ALL employees with:
## -- Their name, department, salary
## -- Their DENSE_RANK within their department by salary descending
## -- A running total of salary within their department
## -- ordered by salary descending (cumulative from highest to current)
## -- A label: 'Top Earner' if dense_rank = 1, else 'Others'
## -- Sort by department, then salary descending.

``` sql
Select name, department, salary, dense_rank() over(partition by department order by salary desc) as denserank, sum(salary)
over(partition by department order by salary desc) as running_total, 
  case 
		when dense_rank() over(partition by department order by salary desc) =1 then 'Top earner'
		
		else 'Others' 
	end as label
from employees order by department, salary desc;
```
---