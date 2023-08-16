# Different Join Operations in MySQL

<pre>
CREATE DATABASE IF NOT EXISTS joinDB;

USE joinDB;

CREATE TABLE department (
  deptno INT, 
  dname VARCHAR(15), 
  loc VARCHAR(10)
);

INSERT INTO department VALUES (10,'Inventory','Hyderabad');
INSERT INTO department VALUES (20,'Finance','Bangalore');
INSERT INTO department VALUES (30,'HR','Mumbai');
INSERT INTO department VALUES (50,'IT','Delhi');

-- create employee table
CREATE TABLE employee (
   empno INT,
   ename VARCHAR(15),  
   job VARCHAR(10),
   mgr INT,
   deptno INT
);

-- inserting records to employee table
INSERT INTO employee VALUES (111,'Saketh','Analyst',444,10);
INSERT INTO employee VALUES (222,'Sudha','Clerk',333,20);
INSERT INTO employee VALUES (333,'Jagan','Manager',111,10); 
INSERT INTO employee VALUES (444,'Madhu','Engineer',222,40);
</pre>

* **Test different Joins:**
* **1. Equi Join**
* **2. Non-Equi Join**
* **3. Self Join**
* **4. Natural Join**
* **5. Cross Join**
* **6. Outer Join**
   * **a. Left outer**
   * **b. Right outer**
   * **c. Full outer**
* **7. Inner Join**

<pre>
-- Equi join
SELECT * 
FROM employee e
JOIN department d 
ON e.deptno = d.deptno;

-- Not Equi join
SELECT *
FROM employee e 
JOIN department d
ON e.deptno > d.deptno;

-- Self join
SELECT * 
FROM employee e1 
JOIN employee e2 
ON e1.empno = e2.mgr;

-- Natural Join
SELECT *
FROM employee
NATURAL JOIN department;

-- cross join
SELECT *
FROM employee
CROSS JOIN department;

-- outer join
-- left outer join
SELECT *
FROM employee e
LEFT JOIN department d 
ON e.deptno = d.deptno;

-- right outer join
SELECT *
FROM employee e
RIGHT JOIN department d
ON e.deptno = d.deptno;

-- full outer join 1
SELECT * FROM employee e 
LEFT JOIN department d
ON e.deptno = d.deptno
UNION 
SELECT * FROM employee e
RIGHT JOIN department d 
ON e.deptno = d.deptno;

-- Inner Join
SELECT *
FROM employee
INNER JOIN department USING (deptno);

-- Equi join with USING clause:
SELECT * 
FROM employee e
JOIN department d  
USING (deptno);

-- Equi join with ON clause:
SELECT * 
FROM employee e
JOIN department d  
ON (e.deptno = d.deptno);

SELECT *
FROM employee e
LEFT JOIN department d
ON e.deptno = d.deptno;

-- Full outer join 2
SELECT * FROM employee e
LEFT JOIN department d
ON e.deptno = d.deptno
UNION ALL
SELECT * FROM employee e 
RIGHT JOIN department d
ON e.deptno = d.deptno
WHERE e.deptno IS NULL OR d.deptno IS NULL;
</pre>
