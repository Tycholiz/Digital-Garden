
A window function performs a calculation across a set of table rows. Each row is somehow related to the current row.
- Window Functions differ from aggregate functions in that they do not cause rows to become grouped into a single output row. Instead, the rows retain their separate identities.
	- Behind the scenes, the window function is able to access more than just the current row of the query result.
- ex. Imagine we want a result set of all employees, and have their individual salaries compared against the average of their individual departments:
```
SELECT 
	depname, 
	empno, 
	salary, 
	avg(salary) 
OVER (PARTITION BY depname) 
FROM empsalary;
```
would result in:
```
  depname  | empno | salary |          avg          
-----------+-------+--------+-----------------------
 develop   |    11 |   5200 | 5020.0000000000000000
 develop   |     7 |   4200 | 5020.0000000000000000
 develop   |     9 |   4500 | 5020.0000000000000000
 develop   |     8 |   6000 | 5020.0000000000000000
 develop   |    10 |   5200 | 5020.0000000000000000
 personnel |     5 |   3500 | 3700.0000000000000000
 personnel |     2 |   3900 | 3700.0000000000000000
 sales     |     3 |   4800 | 4866.6666666666666667
 sales     |     1 |   5000 | 4866.6666666666666667
 sales     |     4 |   4800 | 4866.6666666666666667
(10 rows)
```

### Over
The OVER clause determines exactly how the rows of the query are split up for processing by the window function
- the OVER clause is what causes it to be treated as a window function, and therefore computed across the window frame.
- A window function call always contains an OVER clause directly following the window function's name and argument(s). 
