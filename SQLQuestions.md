**Q1. 2nd highest salary**      
   
+-------------+------+         
| Column Name | Type |       
+-------------+------+       
| id          | int  |       
| salary      | int  |       
+-------------+------+       
id is the primary key column for this table.
Each row of this table contains information about the salary of an employee.

Write an SQL query to report the second highest salary from the Employee table. If there is no second highest salary, the query should report null.

Solution 1:
Find the max after kicking out the max --> 2nd highest

```
SELECT IFNULL(NULL, max(salary)) as SecondHighestSalary
FROM Employee
WHERE salary <> (SELECT max(salary) FROM Employee)
```

Solution 2:
A more general solution:

```
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
SET N = N-1;
  RETURN (
      SELECT DISTINCT(salary) from Employee order by salary DESC
      LIMIT 1 OFFSET N      
  );
END
```
