# Korth Exercise 4.2 Answers

<pre>
CREATE DATABASE IF NOT EXISTS korthDB;
USE korthDB;
</pre>

**Create tables for the following schemas:**
* employee (employee_name, street, city)
* works (employee_name, company-name, salary)
* company (company_name, city)
* manages (employee_name, manager_name)

<pre>
-- Create table Employee
CREATE TABLE employee (
  employee_name VARCHAR(20),
  street VARCHAR(6),
  city VARCHAR(5)
);

-- Insert into Employee
INSERT INTO employee VALUES ('ashoke','vip','cal');
INSERT INTO employee VALUES ('sanjay','gt','mum');
INSERT INTO employee VALUES ('rakesh','bt','cal');
INSERT INTO employee VALUES ('dilip','vip','cal');
INSERT INTO employee VALUES ('manoj','gt','del');
INSERT INTO employee VALUES ('pranab','vip','cal');
INSERT INTO employee VALUES ('salim','vip','del');
INSERT INTO employee VALUES ('karim','bt','mum');
INSERT INTO employee VALUES ('sayan','gt','cal');
INSERT INTO employee VALUES ('rajib','vip','mum');

-- Create table Company
CREATE TABLE company (
  company_name VARCHAR(20),
  city VARCHAR(5) 
);

-- Insert into Company
INSERT INTO company VALUES ('satyam','mum');
INSERT INTO company VALUES ('satyam','del');
INSERT INTO company VALUES ('aol','cal');
INSERT INTO company VALUES ('vsnl','del');

-- Create table Works
CREATE TABLE works (
  employee_name VARCHAR(20),
  company_name VARCHAR(20),
  salary INT
);

-- Insert into Works
INSERT INTO works VALUES ('rakesh','satyam',8000);
INSERT INTO works VALUES ('rajib','aol',15000);
INSERT INTO works VALUES ('manoj','aol',10000); 
INSERT INTO works VALUES ('pranab','aol',50000);
INSERT INTO works VALUES ('karim','satyam',35000);
INSERT INTO works VALUES ('salim','satyam',6000);
INSERT INTO works VALUES ('sayan','aol',20000);
INSERT INTO works VALUES ('dilip','vsnl',35000);
INSERT INTO works VALUES ('ashoke','vsnl',13000);
INSERT INTO works VALUES ('sanjay','vsnl',30000);

-- Create table Manages
CREATE TABLE manages (
  employee_name VARCHAR(20),
  manager_name VARCHAR(20)
);

-- Insert into Manages
INSERT INTO manages VALUES ('ashoke','dilip');
INSERT INTO manages VALUES ('sanjay','dilip');
INSERT INTO manages VALUES ('rakesh','karim');
INSERT INTO manages VALUES ('manoj','pranab');
INSERT INTO manages VALUES ('salim','karim'); 
INSERT INTO manages VALUES ('sayan','pranab');
INSERT INTO manages VALUES ('rajib','pranab');

-- show table descriptions and records
DESC employee;
SELECT * FROM employee;
DESC company;
SELECT * FROM company ;
DESC works;
SELECT * FROM works;
DESC manages;
SELECT * FROM manages;

</pre>
* **Test different queries:**

* **A)	Find employees who work for VSNL.**
<pre>
SELECT employee_name 
FROM works
WHERE company_name = 'vsnl';
</pre>

![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/5be9e0df-7632-4ace-8a06-b00184cbff7f)

* **B)	Find names and cities of VSNL employees.**
<pre>
SELECT e.employee_name, e.city
FROM employee e JOIN works w
ON e.employee_name = w.employee_name
WHERE w.company_name = 'vsnl';

-- using set membership operator 'in'
SELECT employee_name, city
FROM employee
WHERE employee_name IN
	(SELECT employee_name FROM works WHERE company_name = 'vsnl');
</pre>

![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/cd0484af-d591-48eb-8da0-7ca7dc1b0bd6)

* **C)	Find AOL employees with salary > 20,000.**
<pre>
SELECT e.employee_name, e.street, e.city 
FROM employee e JOIN works w
ON e.employee_name = w.employee_name
WHERE w.company_name = 'aol' AND w.salary > 20000;
</pre>

![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/f79a3b48-7c0f-4caa-ad74-89d1dcb7a873)

* **D)	Find employees living in same city as their companies in which they work respectively.**
<pre>
SELECT DISTINCT e.employee_name
FROM employee e JOIN company c JOIN works w
ON e.city = c.city 
AND w.company_name = c.company_name
AND w.employee_name = e.employee_name;
</pre>

![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/b2a0e5bb-fc97-4266-8a4c-7595694ca45f)

* **E)	Find employees living in same city and street as their respective managers.**
<pre>
SELECT e.employee_name  
FROM employee e JOIN manages m
ON e.employee_name = m.employee_name
WHERE (e.street, e.city) IN (
  SELECT street, city
  FROM employee
  WHERE employee_name = m.manager_name
);

SELECT emp.employee_name
FROM employee emp, employee man
WHERE emp.street = man.street
  AND emp.city = man.city
  AND (emp.employee_name, man.employee_name) IN
      (SELECT * FROM manages);
</pre>

![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/4bb388df-3c69-49d0-950e-52bae35a7695)

* **F)	Find employees not working for VSNL.**
<pre>
SELECT employee_name
FROM works  
WHERE company_name <> 'vsnl';

SELECT employee_name
FROM works  
WHERE company_name != 'vsnl';

(SELECT employee_name FROM works)
EXCEPT
(SELECT employee_name FROM works WHERE company_name = 'vsnl');
</pre>

![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/96843040-e829-4aef-9b03-bd883e38d85a)

* **G)	Find employees earning more than every employee of Satyam.**
<pre>
SELECT employee_name
FROM works
WHERE salary > (
  SELECT MAX(salary) 
  FROM works
  WHERE company_name = 'satyam'
);

SELECT employee_name
FROM works
WHERE salary > all (
  SELECT salary
  FROM works
  WHERE company_name = 'satyam'
);
</pre>

![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/bc1620f0-f20e-447c-b0bd-b6f004134227)

* **H)	Find companies located in every city where VSNL is located.**
<pre>
SELECT DISTINCT c.company_name
FROM company c
WHERE NOT EXISTS (
	  (SELECT c1.city
     FROM company c1
     WHERE c1.company_name = 'vsnl')
    EXCEPT
    (SELECT c2.city
	   FROM company c2
     WHERE c2.company_name = c.company_name));

SELECT DISTINCT c.company_name
FROM company c
WHERE NOT EXISTS (
  SELECT v.city
  FROM company v
  WHERE v.company_name = 'vsnl'  
  AND NOT EXISTS (
    SELECT c2.city
    FROM company c2 
    WHERE c2.company_name = c.company_name
    AND c2.city = v.city
  )  
);
</pre>

![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/293849b9-ea86-4e00-8487-9e11a0db3a93)

* **I)	Find employees earning more than their company average payroll. Payroll means total salary given by that company.**
<pre>
SELECT employee_name
FROM works w
WHERE salary > (
  SELECT AVG(salary)
  FROM works
  WHERE w.company_name = company_name
  GROUP BY company_name
);

SELECT employee_name
FROM works w1
WHERE salary > (
	SELECT avg(salary)
	FROM works w2
    WHERE w1.company_name = w2.company_name);
</pre>

![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/7284a094-0a28-455f-865c-fbd5cff5ecf7)

* **J)	Find company with most number of employees.**
<pre>
SELECT company_name
FROM works
GROUP BY company_name
HAVING COUNT(employee_name) >= ALL (
  SELECT COUNT(employee_name)
  FROM works
  GROUP BY company_name
);

SELECT company_name FROM works
GROUP BY company_name
ORDER BY count(*) DESC
LIMIT 1;
</pre>

![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/c492a0c2-f8d3-4dab-b4b9-8d607a4e8e02)

* **K)	Find company with smallest payroll. Payroll means total salary given by that company.**
<pre>
SELECT company_name
FROM works  
GROUP BY company_name
HAVING SUM(salary) <= ALL (
  SELECT SUM(salary) 
  FROM works
  GROUP BY company_name
);

SELECT company_name
FROM works
GROUP BY company_name
ORDER BY SUM(salary) ASC
LIMIT 1;
</pre>

![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/f22e5b58-3bbc-4d4e-9f87-7f6c875fc9a7)

* **L)	Find companies whose average salary is more than Satyam's average salary.**
<pre>
SELECT company_name
FROM works
GROUP BY company_name  
HAVING AVG(salary) > (
  SELECT AVG(salary)
  FROM works
  WHERE company_name = 'satyam' 
);
</pre>

![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/1147ad36-3b3a-4834-8d26-11fde17ceff9)

(Total 12 Queries)
