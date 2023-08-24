# EMP-DEPT-SALGRADE-BONUS Case Study
<pre>
-- EMP-DEPT-SALGRADE-BONUS Case Study
DROP DATABASE IF EXISTS SalgradeDB;
CREATE DATABASE IF NOT EXISTS SalgradeDB;

USE SalgradeDB;

-- Create tables for the following schemas:
--     dept(deptno (PK), dname, loc)
--     emp(empno (PK), ename, job, mgr, hiredate, sal, comm, deptno (FK))
--     bonus(ename, job, sal, comm) 
--     salgrade(grade, losal, hisal)
</pre>
<hr>
<pre>
-- Create dept Table
CREATE TABLE dept (
  deptno INT PRIMARY KEY,
  dname VARCHAR(14),
  loc VARCHAR(13)
);

-- Insert into dept
INSERT INTO dept VALUES (10, 'ACCOUNTING', 'NEW YORK');
INSERT INTO dept VALUES (20, 'RESEARCH', 'DALLAS');
INSERT INTO dept VALUES (30, 'SALES', 'CHICAGO');
INSERT INTO dept VALUES (40, 'OPERATIONS', 'BOSTON');

EXPLAIN dept;
SELECT * FROM dept;
</pre>
<hr>
<pre>
-- Create emp Table
CREATE TABLE emp (
  empno INT PRIMARY KEY,
  ename VARCHAR(10),
  job VARCHAR(9), 
  mgr INT,
  hiredate DATE, 
  sal DECIMAL(7,2),
  comm DECIMAL(7,2),
  deptno INT,
  FOREIGN KEY (deptno) REFERENCES dept(deptno)
);

-- Insert into emp
INSERT INTO emp VALUES(7839, 'KING', 'PRESIDENT', NULL, STR_TO_DATE('17-11-1981', '%d-%m-%Y'), 5000, NULL, 10);
INSERT INTO emp VALUES(7698, 'BLAKE', 'MANAGER', 7839, STR_TO_DATE('01-05-1981', '%d-%m-%Y'), 2850, NULL, 30);
INSERT INTO emp VALUES(7782, 'CLARK', 'MANAGER', 7839, STR_TO_DATE('09-06-1981', '%d-%m-%Y'), 2450, NULL, 10);
INSERT INTO emp VALUES(7566, 'JONES', 'MANAGER', 7839, STR_TO_DATE('02-04-1981', '%d-%m-%Y'), 2975, NULL, 20);
INSERT INTO emp VALUES(7788, 'SCOTT', 'ANALYST', 7566, STR_TO_DATE('13-07-1987', '%d-%m-%Y'), 3000, NULL, 20);
INSERT INTO emp VALUES(7902, 'FORD', 'ANALYST', 7566, STR_TO_DATE('03-12-1981', '%d-%m-%Y'), 3000, NULL, 20);
INSERT INTO emp VALUES(7369, 'SMITH', 'CLERK', 7902, STR_TO_DATE('17-12-1980', '%d-%m-%Y'), 800, NULL, 20);
INSERT INTO emp VALUES(7499, 'ALLEN', 'SALESMAN', 7698, STR_TO_DATE('20-02-1981', '%d-%m-%Y'), 1600, 300, 30);
INSERT INTO emp VALUES(7521, 'WARD', 'SALESMAN', 7698, STR_TO_DATE('22-02-1981', '%d-%m-%Y'), 1250, 500, 30);
INSERT INTO emp VALUES(7654, 'MARTIN', 'SALESMAN', 7698, STR_TO_DATE('28-09-1981', '%d-%m-%Y'), 1250, 1400, 30);
INSERT INTO emp VALUES(7844, 'TURNER', 'SALESMAN', 7698, STR_TO_DATE('08-09-1981', '%d-%m-%Y'), 1500, 0, 30);
INSERT INTO emp VALUES(7876, 'ADAMS', 'CLERK', 7788, STR_TO_DATE('13-07-1987', '%d-%m-%Y'), 1100, NULL, 20);
INSERT INTO emp VALUES(7900, 'JAMES', 'CLERK', 7698, STR_TO_DATE('03-12-1981', '%d-%m-%Y'), 950, NULL, 30); 
INSERT INTO emp VALUES(7934, 'MILLER', 'CLERK', 7782, STR_TO_DATE('23-01-1982', '%d-%m-%Y'), 1300, NULL, 10);

EXPLAIN emp;
SELECT * FROM emp;
</pre>
<hr>
<pre>
-- Create salgrade Table
CREATE TABLE salgrade (
    grade INT,
    losal INT,
    hisal INT
);

-- Insert into salgrade
INSERT INTO salgrade VALUES (1, 700, 1200);
INSERT INTO salgrade VALUES (2, 1201, 1400);
INSERT INTO salgrade VALUES (3, 1401, 2000);
INSERT INTO salgrade VALUES (4, 2001, 3000);
INSERT INTO salgrade VALUES (5, 3001, 9999);

EXPLAIN salgrade;
SELECT * FROM salgrade;
</pre>
<hr>
<pre>
-- Create bonus Table
CREATE TABLE bonus (
  ename VARCHAR(10),
  job VARCHAR(9),
  sal INT,
  comm INT  
);

-- Insert into bonus
INSERT INTO bonus VALUES
  ('SMITH', 'CLERK', 800, NULL),
  ('ALLEN', 'SALESMAN', 1600, 300),
  ('WARD', 'SALESMAN', 1250, 500),
  ('MARTIN', 'SALESMAN', 1250, 1400),
  ('TURNER', 'SALESMAN', 1500, 0),
  ('ADAMS', 'CLERK', 1100, NULL),
  ('JAMES', 'CLERK', 950, NULL),
  ('MILLER', 'CLERK', 1300, NULL);
  
EXPLAIN bonus;
SELECT * FROM bonus;
</pre>
<hr>
<pre>
EXPLAIN emp;
SELECT * FROM emp;
EXPLAIN dept;
SELECT * FROM dept;
EXPLAIN bonus;
SELECT * FROM bonus;
EXPLAIN salgrade;
SELECT * FROM salgrade;
</pre>
<hr>

## Sample Queries
<hr>
<pre>
-- Q1: Get the name and job of all employees:
SELECT ename, job 
FROM emp;
</pre>
<hr>
<pre>
-- Q2: Get the name and location of all departments:
SELECT dname, loc
FROM dept;
</pre>
<hr>
<pre>
-- Q3: Get the salary and commission of employees with a commission:
SELECT sal, comm
FROM emp
WHERE comm IS NOT NULL;
</pre>
<hr>
<pre>
-- Q4: Get the maximum salary in the employees table:
SELECT MAX(sal)
FROM emp;
</pre>
<hr>
<pre>
-- Q5: Get the number of employees in each department:
SELECT deptno, COUNT(*)
FROM emp
GROUP BY deptno;
</pre>
<hr>
<pre>
-- Q6: Get the total salary of each department:
SELECT d.dname, SUM(e.sal) as total_salary
FROM dept d
JOIN emp e ON d.deptno = e.deptno
GROUP BY d.deptno;
</pre>
<hr>
<pre>
-- Q7: Get the number of employees in each job:
SELECT job, COUNT(*)
FROM emp
GROUP BY job;
</pre>
<hr>
<pre>
-- Q8: Get the highest, lowest, and average salary of each job type:
SELECT job, MAX(sal) as highest, MIN(sal) as lowest, AVG(sal) as average 
FROM emp
GROUP BY job;
</pre>
<hr>
<pre>
-- Q9: Get the bonus details of employees along with their salary:
SELECT e.ename, e.sal, b.comm 
FROM emp e
LEFT JOIN bonus b ON e.ename = b.ename;
</pre>
<hr>
<pre>
-- Q10: Get the grade range of each employee based on their salary:
SELECT e.ename, s.grade
FROM emp e
JOIN salgrade s ON e.sal BETWEEN s.losal AND s.hisal;
</pre>
<hr>
<pre>
-- Q11: Get employees with the highest salary in their department:
SELECT e.* 
FROM emp e
WHERE e.sal = (SELECT MAX(sal) 
               FROM emp 
               WHERE deptno = e.deptno);
-- otherwise
SELECT e.* 
FROM emp e
WHERE e.sal >= all (SELECT sal
               FROM emp 
               WHERE deptno = e.deptno);
</pre>
<hr>
<pre>
-- Q12: Get departments with more than 2 employees:
SELECT d.*
FROM dept d 
WHERE (SELECT COUNT(*) 
       FROM emp 
       WHERE deptno = d.deptno) > 2;
</pre>
<hr>
<pre>
-- Q13: Get employees whose salary is greater than the average salary of their department:
SELECT e.*
FROM emp e
WHERE e.sal > (SELECT AVG(sal)
               FROM emp 
               WHERE deptno = e.deptno);
</pre>
<hr>
<pre>
-- Q14: Get the total bonus given in each job category:
SELECT b.job, SUM(b.comm) as total_bonus 
FROM bonus b
GROUP BY b.job;
</pre>
<hr>
<pre>
-- Q15: Get the number of employees in the same job and department:
SELECT job, deptno, COUNT(*)
FROM emp
GROUP BY job, deptno;
</pre>
<hr>
<pre>
-- Q16: Get employees who earn more than the average salary in their department and job:
SELECT e.*
FROM emp e 
WHERE e.sal > (
  SELECT AVG(sal) 
  FROM emp
  WHERE deptno = e.deptno AND job = e.job
);
</pre>
<hr>
<pre>
-- Q17: Get the difference between the highest and lowest salary in each department:
SELECT d.deptno, 
(SELECT MAX(sal) FROM emp WHERE deptno = d.deptno) - 
(SELECT MIN(sal) FROM emp WHERE deptno = d.deptno) AS salary_difference
FROM dept d;
</pre>
<hr>
<pre>
-- Q18: Get employees who earn more than the average salary in their job category:
SELECT e.*
FROM emp e
WHERE e.sal > (
  SELECT AVG(sal)
  FROM emp
  WHERE job = e.job
);
</pre>
<hr>
<pre>
-- Q19: Get departments along with the number of employees in each job category:
SELECT d.dname, d.deptno, e.job, COUNT(*)
FROM dept d
LEFT JOIN emp e ON d.deptno = e.deptno
GROUP BY d.deptno, e.job;
</pre>
<hr>

## Applications
<hr>
<pre>
-- WITH Clauses (Common Table Expressions CTE)
-- Application 1 [This CTE extracts just the salary columns from emp table.]:
WITH emp_salaries AS (
  SELECT empno, ename, sal
  FROM emp 
)

SELECT *
FROM emp_salaries
WHERE sal > 3000;
</pre>
<hr>
<pre>
-- Application 2: This CTE extracts just the salary columns from emp table. [This calculates the total salary per department.]
WITH dept_costs AS (
  SELECT deptno, SUM(sal) AS dept_salary
  FROM emp
  GROUP BY deptno  
)

SELECT d.*, dc.dept_salary
FROM dept d
JOIN dept_costs dc ON d.deptno = dc.deptno;
</pre>
<hr>
<pre>
-- Application 3: This calculates the total salary per department. [This joins emp and bonus table to get all employee bonuses.]
WITH emp_bonuses AS (
  SELECT 
    e.ename, e.job, e.sal, b.comm
  FROM emp e
  LEFT JOIN bonus b ON e.ename = b.ename
)

SELECT * 
FROM emp_bonuses;
</pre>
<hr>
<pre>
-- Application 4: This joins emp and bonus table to get all employee bonuses. [This uses a CTE to get the top 5 highest earning employees.]
WITH top_earners AS (
  SELECT ename, sal
  FROM emp
  ORDER BY sal DESC
  LIMIT 5 
)

SELECT *
FROM top_earners;
</pre>
<hr>
