**Q1. 2nd highest salary**      
```   
+-------------+------+         
| Column Name | Type |       
+-------------+------+       
| id          | int  |       
| salary      | int  |       
+-------------+------+     
```
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
---
**Q2. Rank Scores**  
```   
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| score       | decimal |
+-------------+---------+    
```
id is the primary key for this table.
Each row of this table contains the score of a game. Score is a floating point value with two decimal places.

Write an SQL query to rank the scores. The ranking should be calculated according to the following rules:

The scores should be ranked from the highest to the lowest.
If there is a tie between two scores, both should have the same ranking.
After a tie, the next ranking number should be the next consecutive integer value. In other words, there should be no holes between ranks.
Return the result table ordered by score in descending order.

Solution:
```
SELECT score,
DENSE_RANK() OVER (order by score desc) as rank
FROM Scores
ORDER BY score desc 
```
Be careful here: 
for dense_rank(), the rank numbers will be consecutive.
cannot write "partiton by id" here cuz this means all scores will be categorized by each own id which is unique and this makes the ranking non-sense.

---
