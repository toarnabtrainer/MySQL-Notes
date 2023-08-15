## Korth Chapter - 4 (Used Sample Database and Tables)<br>
![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/d06daab7-d701-4ea6-a4ae-19cda117dd87)

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

