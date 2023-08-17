# Different types of MySQL Functions

## Create database, tables and insert records -

<pre>
CREATE DATABASE IF NOT EXISTS erd2DB; 
USE erd2DB;

-- creating all database tables
-- create table for country
CREATE TABLE country (
  id INT AUTO_INCREMENT,
  country_name VARCHAR(128),
  country_name_eng VARCHAR(128),
  country_code CHAR(8),
  PRIMARY KEY (id)
);

-- create table for city (Here long is a keywork is MySQL, so using
-- longitude and latitude in place of long and lat
CREATE TABLE city (
  id INT AUTO_INCREMENT, 
  city_name VARCHAR(128),
  latitude DECIMAL(9,6),
  longitude DECIMAL(9,6),
  country_id INT,
  PRIMARY KEY (id)
);

-- create table for employee
CREATE TABLE employee (
  id INT AUTO_INCREMENT,
  first_name VARCHAR(255),
  last_name VARCHAR(255),
  PRIMARY KEY (id)
);

-- create table for customer
CREATE TABLE customer (
  id INT AUTO_INCREMENT,
  customer_name VARCHAR(255),
  city_id INT,
  customer_address VARCHAR(255),
  next_call_date DATE,
  ts_inserted DATETIME,
  PRIMARY KEY (id)
);

-- create table for call_table (call is a reserved word in MySQL, 
-- so giving the table name as call_table
CREATE TABLE call_table (
  id INT AUTO_INCREMENT,
  employee_id INT,
  customer_id INT,
  start_time DATETIME,
  end_time DATETIME,
  call_outcome_id INT,
  PRIMARY KEY (id)
);

-- create table for call_outcome
CREATE TABLE call_outcome (
  id INT AUTO_INCREMENT,
  outcome_text VARCHAR(128),
  PRIMARY KEY (id)
);

-- now adding foreign key constraints
-- add country id as foreign key in city table
ALTER TABLE city 
ADD CONSTRAINT city_country
FOREIGN KEY (country_id) 
REFERENCES country(id);

-- add call_outcome_id as foreign key in call_table
ALTER TABLE call_table
ADD CONSTRAINT call_call_outcome
FOREIGN KEY (call_outcome_id)
REFERENCES call_outcome(id);

-- add customer_id as foreign key in call_table
ALTER TABLE call_table
ADD CONSTRAINT call_customer
FOREIGN KEY (customer_id)
REFERENCES customer(id);

-- add employee_id as foreign key in call_table
ALTER TABLE call_table
ADD CONSTRAINT call_employee
FOREIGN KEY (employee_id)
REFERENCES employee(id);

-- add city_id as foreign key in customer
ALTER TABLE customer
ADD CONSTRAINT customer_city
FOREIGN KEY (city_id)
REFERENCES city(id);

-- Insert into country
INSERT INTO country (country_name, country_name_eng, country_code) VALUES ('Deutschland', 'Germany', 'DEU');
INSERT INTO country (country_name, country_name_eng, country_code) VALUES ('Srbija', 'Serbia', 'SRB');
INSERT INTO country (country_name, country_name_eng, country_code) VALUES ('Hrvatska', 'Croatia', 'HRV');
INSERT INTO country (country_name, country_name_eng, country_code) VALUES ('United Stated of America', 'United Stated of America', 'USA');
INSERT INTO country (country_name, country_name_eng, country_code) VALUES ('Polska', 'Poland', 'POL');
SELECT * FROM country;
EXPLAIN country;

-- Insert into city
INSERT INTO city (city_name, latitude, longitude, country_id) VALUES ('Berlin', 52.520008, 13.404954, 1);
INSERT INTO city (city_name, latitude, longitude, country_id) VALUES ('Belgrade', 44.787197, 20.457273, 2);
INSERT INTO city (city_name, latitude, longitude, country_id) VALUES ('Zagreb', 45.815399, 15.966568, 3);
INSERT INTO city (city_name, latitude, longitude, country_id) VALUES ('New York', 40.73061, -73.935242, 4);
INSERT INTO city (city_name, latitude, longitude, country_id) VALUES ('Los Angeles', 34.052235, -118.243683, 4);
INSERT INTO city (city_name, latitude, longitude, country_id) VALUES ('Warsaw', 52.237049, 21.017532, 5);
SELECT * FROM city;
EXPLAIN city;

-- Insert into call_outcome
INSERT INTO call_outcome (outcome_text) VALUES ('call started');
INSERT INTO call_outcome (outcome_text) VALUES ('finished - successfully');
INSERT INTO call_outcome (outcome_text) VALUES ('finished - unsuccessfully');
SELECT * FROM call_outcome;
EXPLAIN call_outcome;
	
-- Insert into employee
INSERT INTO employee (first_name, last_name)
VALUES ('Thomas (Neo)', 'Anderson');
INSERT INTO employee (first_name, last_name)
VALUES ('Agent', 'Smith');
SELECT * FROM employee;
EXPLAIN employee;

-- Insert into customer
INSERT INTO customer (customer_name, city_id, customer_address, next_call_date, ts_inserted) VALUES ('Jewelry Store', 4, 'Long Street 120', '2020-01-21', '2020-01-09 14:01:20');
INSERT INTO customer (customer_name, city_id, customer_address, next_call_date, ts_inserted) VALUES ('Bakery', 1, 'Kurfürstendamm 25', '2020-02-21', '2020-01-09 17:52:15');
INSERT INTO customer (customer_name, city_id, customer_address, next_call_date, ts_inserted) VALUES ('Café', 1, 'Tauentzienstraße 44', '2020-01-21', '2020-01-10 08:02:49');
INSERT INTO customer (customer_name, city_id, customer_address, next_call_date, ts_inserted) VALUES ('Restaurant', 3, 'Ulica lipa 15', '2020-01-21', '2020-01-10 09:20:21');
SELECT * FROM customer;
EXPLAIN customer;

-- Insert into call_table
INSERT INTO call_table (employee_id, customer_id, start_time, end_time, call_outcome_id) VALUES (1, 4, '2020-01-11 09:00:15', '2020-01-11 09:12:22', 2);
INSERT INTO call_table (employee_id, customer_id, start_time, end_time, call_outcome_id) VALUES (1, 2, '2020-01-11 09:14:50', '2020-01-11 09:20:01', 2);
INSERT INTO call_table (employee_id, customer_id, start_time, end_time, call_outcome_id) VALUES (2, 3, '2020-01-11 09:02:20', '2020-01-11 09:18:05', 3);
INSERT INTO call_table (employee_id, customer_id, start_time, end_time, call_outcome_id) VALUES (1, 1, '2020-01-11 09:24:15', '2020-01-11 09:25:05', 3);
INSERT INTO call_table (employee_id, customer_id, start_time, end_time, call_outcome_id) VALUES (1, 3, '2020-01-11 09:26:23', '2020-01-11 09:33:45', 2);
INSERT INTO call_table (employee_id, customer_id, start_time, end_time, call_outcome_id) VALUES (1, 2, '2020-01-11 09:40:31', '2020-01-11 09:42:32', 2);
INSERT INTO call_table (employee_id, customer_id, start_time, end_time, call_outcome_id) VALUES (2, 4, '2020-01-11 09:41:17', '2020-01-11 09:45:21', 2);
INSERT INTO call_table (employee_id, customer_id, start_time, end_time, call_outcome_id) VALUES (1, 1, '2020-01-11 09:42:32', '2020-01-11 09:46:53', 3);
INSERT INTO call_table (employee_id, customer_id, start_time, end_time, call_outcome_id) VALUES (2, 1, '2020-01-11 09:46:00', '2020-01-11 09:48:02', 2);
INSERT INTO call_table (employee_id, customer_id, start_time, end_time, call_outcome_id) VALUES (2, 2, '2020-01-11 09:50:12', '2020-01-11 09:55:35', 2);
SELECT * FROM call_table;
EXPLAIN call_table;

-- display records
SELECT * FROM call_table;
SELECT * FROM call_outcome;
SELECT * FROM customer;
SELECT * FROM employee;
SELECT * FROM city;
SELECT * FROM country;
</pre>

<hr>

## Some Function Based Queries:
### Text Functions
* **Query: Find customers whose name starts with 'J':**
<pre>
SELECT * FROM customer
WHERE LEFT(customer_name, 1) = 'J';
	
-- using wild-card characters
SELECT * FROM customer
WHERE customer_name LIKE "j%";
</pre>

* **Query: Find call outcomes containing 'finished':**
<pre>
SELECT * FROM call_outcome
WHERE outcome_text LIKE '%finished%';
</pre>

* **Query: Concatenate first and last name of employees:**
<pre>
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM employee;
</pre>

* **Query: Find length of customer address:**
<pre>
SELECT LENGTH(customer_address) 
FROM customer;
</pre>

* **Query: Uppercase and Lowercase country name:**
<pre>
SELECT UPPER(country_name), LOWER(country_name)
FROM country;
</pre>

* **Query: Find index of first space in name:**
<pre>
SELECT INSTR(first_name, ' ')
FROM employee;

SELECT customer_address, INSTR(customer_address, " ") FROM customer;
</pre>

* **Query: Eliminating redundant sapces from a string:**
<pre>
SELECT trim("    university   ") AS trim_university, LENGTH(trim("    university   "));
SELECT rtrim("    university   ") AS trim_university, LENGTH(rtrim("    university   "));
SELECT ltrim("    university   ") AS trim_university, LENGTH(ltrim("    university   "));
</pre>

* **Extra Query:"**
<pre>
SELECT * FROM call_outcome;
UPDATE call_outcome
SET outcome_text = CONCAT(UPPER(LEFT(outcome_text, 1)), LOWER(RIGHT(outcome_text, LENGTH(outcome_text) - 1)));
SELECT * FROM call_outcome;
</pre>

### Numeric Functions
* **Query: Round call duration to nearest minute:**
<pre>
SELECT ROUND(TIMESTAMPDIFF(MINUTE, start_time, end_time)) AS duration
FROM call_table;
</pre>

* **Query: Generate random number between 0 and 1:**
<pre>
SELECT RAND();
</pre>

* **Extra Query:**
<pre>
SELECT ROUND(10.1), ROUND(10.5), ROUND(10.9);
SELECT SQRT(100);
</pre>

### Date Functions
* **Query: Extract year from start time of calls:**
<pre>
SELECT YEAR(start_time) 
FROM call_table;
</pre>

* **Query: Get current date:**
<pre>
SELECT CURDATE();
</pre>

* **Query: Add interval of 1 day to next call date:**
<pre>
EXCLUSIVE DATE_ADD(next_call_date, INTERVAL 1 DAY)
FROM customer;
</pre>

* **Extra Query:**
<pre>
SELECT start_time, HOUR(start_time), MINUTE(start_time), SECOND(start_time)
FROM call_table	  
</pre>

### Null Functions [Null values required to test]
* **Query: Replace NULLs with default value:**
<pre>
SELECT * FROM call_outcome;
INSERT INTO call_outcome(id) VALUES(4);
SELECT * FROM call_outcome;
SELECT COUNT(id), COUNT(outcome_text) FROM call_outcome;
SELECT id, outcome_text, IFNULL(outcome_text, "In process") FROM call_outcome;
</pre>

### Date Functions
* **Query: Get current timestamp:**
<pre>
SELECT NOW();
</pre>

* **Query: Convert to date only from datetime:**
<pre>
SELECT start_time, DATE(start_time)
FROM call_table;
</pre>

* **Query: Difference between two dates:**
<pre>
SELECT DATEDIFF(next_call_date, ts_inserted) 
FROM customer;
</pre>

* **Query: Extract year, month and day from a date:**
<pre>
select start_time, year(start_time), month(start_time), day(start_time)
from call_table;
</pre>

### CASE Clauses:
* **Application 1: Simple CASE Expression:**
<pre>
SELECT 
  customer_id,
  CASE call_outcome_id
    WHEN 1 THEN 'Started'
    WHEN 2 THEN 'Finished Successfully'
    WHEN 3 THEN 'Finished Unsuccessfully'
  END AS call_status
FROM call_table;
</pre>

* **Application 2: Searched CASE Statement:**
<pre>
SELECT
  customer_id,
  CASE 
    WHEN end_time IS NULL THEN 'In Progress'
    WHEN end_time IS NOT NULL THEN 'Completed'
  END AS call_status  
FROM call_table;
</pre>

* **Application 3: CASE in SELECT Clause:**
<pre>
SELECT 
  customer_name,
  city_id,
  CASE 
    WHEN city_id = 1 THEN 'Berlin'
    WHEN city_id = 2 THEN 'Belgrade'
    ELSE 'Other'
  END AS city_name
FROM customer;
</pre>

* **Application 4: CASE in ORDER BY:**
<pre>
SELECT *
FROM call_table
ORDER BY
  CASE 
    WHEN end_time IS NULL THEN 1
    ELSE 0 
  END,
  start_time DESC;
</pre>

<hr>

<pre>
-- drop all foreig key constraints
ALTER TABLE city DROP CONSTRAINT city_country;
ALTER TABLE call_table DROP CONSTRAINT call_call_outcome;
ALTER TABLE call_table DROP CONSTRAINT call_customer;
ALTER TABLE call_table DROP CONSTRAINT call_employee;
ALTER TABLE customer DROP CONSTRAINT customer_city;

-- drop all databse tables
DROP TABLE IF EXISTS call_table;
DROP TABLE IF EXISTS call_outcome;
DROP TABLE IF EXISTS customer;
DROP TABLE IF EXISTS employee;
DROP TABLE IF EXISTS city;
DROP TABLE IF EXISTS country;

-- drop database
DROP DATAABASE IF EXISTS erd2DB;
</pre>
