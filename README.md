# PTS_Databases

* **GitHub Link: https -** https://github.com/toarnabtrainer/PTS_Databases
* **GitHub Shortened Link -** https://tinyurl.com/2btenujh
* **Notepad.pw Link -** https://tinyurl.com/24rkwnxf
* **MySql Official Site -** https://www.mysql.com/
* **MySql Tutorial Site -** https://www.mysqltutorial.org/
* **MySQL Server and Workbench Download and Install YouTube Video -** https://www.youtube.com/watch?v=GwHpIl0vqY4

<br>

## Korth Chapter - 4 (Used Sample Database and Tables)<br>

<pre>
*********************************************
-- Korth Chapter - 4, Page - 144
-- All the schemas:
-- Branch(branch_name, branch_city, assets)
-- Account(account_number, balance, branch_name)
-- Customer(customer_name, customer_street, customer_city)
-- Depositor(account_number, customer_name)
-- Borrower(customer_name, loan_number)
-- Loan(loan_number, amount, branch_name)

create database korth;
use korth;

create table depositor (
	customer_name varchar(20),
	account_number varchar(20)
);

create table loan (
	loan_number varchar(20),
	branch_name varchar(20),
	amount int
);

create table borrower (
	customer_name varchar(20),
	loan_number varchar(20)
);

create table branch (
	branch_name varchar(20),
	branch_city varchar(20),
	assets int
);

create table account (
	account_number varchar(20),
	branch_name varchar(20),
	balance int
);

create table customer (
	customer_name varchar(20),
	customer_street varchar(20),
	customer_city varchar(20)
);

-- insert all the records to the tables from the corresponding csv files

-- or execute following queries
truncate table account;
insert into account values ('A-101','Downtown',500);
insert into account values ('A-102','Perryridge',400);
insert into account values ('A-201','Brighton',900);
insert into account values ('A-215','Mianus',700);
insert into account values ('A-217','Brighton',750);
insert into account values ('A-222','Redwood',700);
insert into account values ('A-305','Round Hill',350);
select * from account;
explain account;

truncate table borrower;
insert into borrower values ('Adams','L-16');
insert into borrower values ('Curry','L-93');
insert into borrower values ('Hayes','L-15');
insert into borrower values ('Jackson','L-14');
insert into borrower values ('Jones','L-17');
insert into borrower values ('Smith','L-11');
insert into borrower values ('Smith','L-23');
insert into borrower values ('Williams','L-17');
select * from borrower;
explain borrower;

truncate table branch;
insert into branch values ('Brighton','Brooklyn',7100000);
insert into branch values ('Downtown','Brooklyn',9000000);
insert into branch values ('Mianus','Horseneck',400000);
insert into branch values ('North Town','Rye',3700000);
insert into branch values ('Perryridge','Horseneck',1700000);
insert into branch values ('Pownal','Bennington',300000);
insert into branch values ('Redwood Palo','Alto',2100000);
insert into branch values ('Round Hill','Horseneck',8000000);
select * from branch;
explain branch;

truncate table customer;
insert into customer values ('Adams','Spring','Pittsfield');
insert into customer values ('Brooks','Senator','Brooklyn');
insert into customer values ('Curry','North','Rye');
insert into customer values ('Glenn','Sand Hill','Woodside');
insert into customer values ('Green','Walnut','Stamford');
insert into customer values ('Hayes','Main','Harrison');
insert into customer values ('Johnson','Alma Palo','Alto');
insert into customer values ('Jones','Main','Harrison');
insert into customer values ('Lindsay','Park','Pittsfield');
insert into customer values ('Smith','North','Rye');
insert into customer values ('Turner','Putnam','Stamford');
insert into customer values ('Williams','Nassau','Princeton');
select * from customer;
explain customer;

truncate table depositor;
insert into depositor values ('Hayes','A-102');
insert into depositor values ('Johnson','A-101');
insert into depositor values ('Johnson','A-201');
insert into depositor values ('Jones','A-217');
insert into depositor values ('Lindsay','A-222');
insert into depositor values ('Smith','A-215');
insert into depositor values ('Turner','A-305');
select * from depositor;
explain depositor;

truncate table loan;
insert into loan values ('L-11','Round Hill',900);
insert into loan values ('L-14','Downtown',1500);
insert into loan values ('L-15','Perryridge',1500);
insert into loan values ('L-16','Perryridge',1300);
insert into loan values ('L-17','Downtown',1000);
insert into loan values ('L-23','Redwood',2000);
insert into loan values ('L-93','Mianus',500);
select * from loan;
explain loan;

select * from account;
select * from borrower;
select * from branch;
select * from customer;
select * from depositor;
select * from loan;
</pre>

## ERD to Database (Used Sample Database and Tables)<br>
![image](https://github.com/toarnabtrainer/PTS_Databases/assets/111301975/bed1eaaf-b549-4f12-a092-6270f8657b1e)

<pre>
create database if not exists erd2Database;
use erd2Database;

-- creating all database table
-- create table for country
create table country (
    id int auto_increment,
    country_name varchar(128),
    country_name_eng varchar(128),
    country_code varchar(8),
    primary key(id)
);

-- create table for city (Here long is a keywork is MySQL, so using
-- longitude and latitude in place of long and lat
create table city (
    id int auto_increment,
    city_name varchar(128),
    longitude decimal(9,6),
    latitude decimal(9,6),
    country_id int,
    primary key (id)
);

-- create table for employee
create table employee (
    id int auto_increment,
    first_name varchar(255),
    last_name varchar(255),
    primary key (id)
);

-- create table for customer
create table customer (
    id int auto_increment,
    customer_name varchar(255),
    city_id int,
    customer_address varchar(255),
    next_call_date date,
    ts_inserted datetime null,
    primary key (id)
);

-- create table for call_table (call is a reserved word in MySQL, 
-- so giving the table name as call_table
create table call_table (
    id int auto_increment,
    employee_id int,
    customer_id int,
    start_time datetime,
    end_time datetime null,
    call_outcome_id int null,
    primary key (id)
);

-- create table for call_outcome
create table call_outcome (
    id int auto_increment,
    outcome_text varchar(128),
    primary key (id)
);

-- now adding foreign key constraints
-- add country id as foreign key in city table
alter table city
add constraint city_country
foreign key (country_id)
references country(id);

-- add call_outcome_id as foreign key in call_table
alter table call_table
add constraint call_call_outcome
foreign key (call_outcome_id)
references call_outcome(id);

-- add customer_id as foreign key in call_table
alter table call_table
add constraint call_customer
foreign key (customer_id)
references customer(id);

-- add employee_id as foreign key in call_table
alter table call_table
add constraint call_employee
foreign key (employee_id)
references employee(id);

-- add city_id as foreign key in customer
alter table customer
add constraint customer_city
foreign key (city_id)
references city(id);

-- Insert into country
TRUNCATE TABLE country;
INSERT INTO country (country_name, country_name_eng, country_code) VALUES ('Deutschland', 'Germany', 'DEU');
INSERT INTO country (country_name, country_name_eng, country_code) VALUES ('Srbija', 'Serbia', 'SRB');
INSERT INTO country (country_name, country_name_eng, country_code) VALUES ('Hrvatska', 'Croatia', 'HRV');
INSERT INTO country (country_name, country_name_eng, country_code) VALUES ('United Stated of America', 'United Stated of America', 'USA');
INSERT INTO country (country_name, country_name_eng, country_code) VALUES ('Polska', 'Poland', 'POL');
SELECT * FROM country;
EXPLAIN country;

-- Insert into city
TRUNCATE TABLE city;
INSERT INTO city (city_name, latitude, longitude, country_id) VALUES ('Berlin', 52.520008, 13.404954, 1);
INSERT INTO city (city_name, latitude, longitude, country_id) VALUES ('Belgrade', 44.787197, 20.457273, 2);
INSERT INTO city (city_name, latitude, longitude, country_id) VALUES ('Zagreb', 45.815399, 15.966568, 3);
INSERT INTO city (city_name, latitude, longitude, country_id) VALUES ('New York', 40.73061, -73.935242, 4);
INSERT INTO city (city_name, latitude, longitude, country_id) VALUES ('Los Angeles', 34.052235, -118.243683, 4);
INSERT INTO city (city_name, latitude, longitude, country_id) VALUES ('Warsaw', 52.237049, 21.017532, 5);
SELECT * FROM city;
EXPLAIN city;

-- Insert into call_outcome
TRUNCATE TABLE call_outcome;
INSERT INTO call_outcome (outcome_text) VALUES ('call started');
INSERT INTO call_outcome (outcome_text) VALUES ('finished - successfully');
INSERT INTO call_outcome (outcome_text) VALUES ('finished - unsuccessfully');
SELECT * FROM call_outcome;
EXPLAIN call_outcome;
	
-- Insert into employee
TRUNCATE TABLE employee;
INSERT INTO employee (first_name, last_name)
VALUES ('Thomas (Neo)', 'Anderson');
INSERT INTO employee (first_name, last_name)
VALUES ('Agent', 'Smith');
SELECT * FROM employee;
EXPLAIN employee;

-- Insert into customer
TRUNCATE TABLE customer;
INSERT INTO customer (customer_name, city_id, customer_address, next_call_date, ts_inserted) VALUES ('Jewelry Store', 4, 'Long Street 120', '2020-01-21', '2020-01-09 14:01:20');
INSERT INTO customer (customer_name, city_id, customer_address, next_call_date, ts_inserted) VALUES ('Bakery', 1, 'Kurfürstendamm 25', '2020-02-21', '2020-01-09 17:52:15');
INSERT INTO customer (customer_name, city_id, customer_address, next_call_date, ts_inserted) VALUES ('Café', 1, 'Tauentzienstraße 44', '2020-01-21', '2020-01-10 08:02:49');
INSERT INTO customer (customer_name, city_id, customer_address, next_call_date, ts_inserted) VALUES ('Restaurant', 3, 'Ulica lipa 15', '2020-01-21', '2020-01-10 09:20:21');
SELECT * FROM customer;
EXPLAIN customer;

-- Insert into call_table
TRUNCATE TABLE call_table;
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

ALTER TABLE city DROP CONSTRAINT city_country;
ALTER TABLE call_table DROP CONSTRAINT call_call_outcome;
ALTER TABLE call_table DROP CONSTRAINT call_customer;
ALTER TABLE call_table DROP CONSTRAINT call_employee;
ALTER TABLE customer DROP CONSTRAINT customer_city;

DROP TABLE IF EXISTS call_table;
DROP TABLE IF EXISTS call_outcome;
DROP TABLE IF EXISTS customer;
DROP TABLE IF EXISTS employee;
DROP TABLE IF EXISTS city;
DROP TABLE IF EXISTS country;

DROP DATAABASE IF EXISTS erd2DB;
</pre>

![image](https://github.com/toarnabtrainer/PTS_Databases/assets/111301975/f13555a1-bd32-48cd-8611-9c874fe0c0b1)
