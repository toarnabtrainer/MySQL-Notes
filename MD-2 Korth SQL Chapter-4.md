# Korth Chapter - 4 (Used Sample Database and Tables)<br>

![image](https://github.com/toarnabtrainer/MySQL_Notes/assets/111301975/836b9aaf-c21c-4cbd-bf12-05be5347dd81)


<pre>
*********************************************
-- Korth Chapter - 4, Page - 143
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
</pre>

<br>

* **Insert all the records to the tables from the corresponding CSV files**
* **Directory Path:** E:\Arnab Docs\MySQL\Korth CSV

<pre>
-- or execute following queries
-- truncate table account;
insert into account values ('A-101','Downtown',500);
insert into account values ('A-102','Perryridge',400);
insert into account values ('A-201','Brighton',900);
insert into account values ('A-215','Mianus',700);
insert into account values ('A-217','Brighton',750);
insert into account values ('A-222','Redwood',700);
insert into account values ('A-305','Round Hill',350);
select * from account;
explain account;

-- truncate table borrower;
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

-- truncate table branch;
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

-- truncate table customer;
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

-- truncate table depositor;
insert into depositor values ('Hayes','A-102');
insert into depositor values ('Johnson','A-101');
insert into depositor values ('Johnson','A-201');
insert into depositor values ('Jones','A-217');
insert into depositor values ('Lindsay','A-222');
insert into depositor values ('Smith','A-215');
insert into depositor values ('Turner','A-305');
select * from depositor;
explain depositor;

-- truncate table loan;
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

* **Demonstration of Queries of that Chapter**

<pre>
-- All the schemas:
-- Branch(branch_name, branch_city, assets)
-- Account(account_number, balance, branch_name)
-- Customer(customer_name, customer_street, customer_city)
-- Depositor(account_number, customer_name)
-- Borrower(customer_name, loan_number)
-- Loan(loan_number, amount, branch_name)

-- The select Clause
select branch_name from loan;
select distinct branch_name from loan;
select all branch_name from loan;  -- equivalent to query without 'all' clause
select loan_number, branch_name, amount * 100 from loan;
select loan_number, branch_name, amount, amount * 0.18 as gst_amount from loan;

-- The where Clause
select loan_number
from loan
where branch_name = "Perryridge" and amount > 1200;

select loan_number
from loan
where amount between 900 and 1000;   -- range has been modified

select loan_number
from loan
where amount >= 900 and amount <= 1000;

-- The from Clause
select borrower.customer_name, loan.loan_number, loan.amount
from loan, borrower
where borrower.loan_number = loan.loan_number;

select customer_name, loan.loan_number, amount
from loan, borrower
where borrower.loan_number = loan.loan_number;

select customer_name, loan.loan_number, amount
from loan, borrower
where borrower.loan_number = loan.loan_number
  and branch_name = "Perryridge";
  
-- The Rename Operation
select customer_name, borrower.loan_number, amount   -- without renaming
from borrower, loan
where borrower.loan_number = loan.loan_number;

select customer_name, borrower.loan_number as loan_id, amount   -- without renaming
from borrower, loan
where borrower.loan_number = loan.loan_number;

select customer_name, borrower.loan_number loan_id, amount   -- without renaming
from borrower, loan
where borrower.loan_number = loan.loan_number;

-- Tuple Variables
select customer_name, T.loan_number as loan_id, S.amount
from borrower as T, loan as S
where T.loan_number = S.loan_number;

select customer_name, T.loan_number loan_id, S.amount
from borrower T, loan S
where T.loan_number = S.loan_number;

select b1.branch_name
from branch b1, branch b2
where b2.branch_city = "Brooklyn"
  and b1.assets > b2.assets;
 
select *
from branch b1, branch b2
where b2.branch_city = "Brooklyn"
  and b1.assets > b2.assets;

-- String Operations
select customer_name, customer_street
from customer
where customer_street like "%Main%";
  
select customer_name, customer_street
from customer
where customer_street like "%ai%";

-- Ordering the Display of Tuples
select customer_name
from loan, borrower
where loan.loan_number = borrower.loan_number
  and branch_name = "Perryridge"
  order by customer_name desc;
  
select *
from loan
order by amount desc, loan_number asc;

select *
from loan
order by branch_name asc, loan_number desc;

-- Duplicates

-- Set Operations
-- The Union Operation
(select customer_name
from depositor)
union
(select customer_name
from borrower);
  
(select customer_name
from depositor)
union all
(select customer_name
from borrower);
  
select distinct customer_name
from
    ((select customer_name
      from depositor)
    union all
     (select customer_name
      from borrower)) as my_table;
      
-- The Intersect Operation
(select distinct customer_name
from depositor)
intersect
(select distinct customer_name
from borrower);

(select customer_name
from depositor)
intersect all
(select customer_name
from borrower);

-- The Except Operation (Set Difference)
(select distinct customer_name
from depositor)
except
(select customer_name
from borrower);

(select customer_name
from depositor)
except all
(select customer_name
from borrower);

-- SELECT those CUSTOMER_NAME having only savings bank a/c no loan a/c
(select distinct customer_name from depositor)
except
(select customer_name from borrower);

-- Aggregate Functions
select avg (balance) avg_balance
from account
where branch_name = "Perryridge";

select avg (balance) "avg balance"
from account
where branch_name = "Perryridge";

select avg(balance) "avg balance", max(balance) "max balance",
       min(balance) "min balance", sum(balance) "total balance",
       count(balance) "cnt balance"
from account
where branch_name = "Brighton";

select avg(balance) avg_balance, max(balance) max_balance,
       min(balance) min_balance, sum(balance) total_balance,
       count(balance) cnt_balance
from account;

select branch_name, avg(balance), min(balance), count(*)
from account
group by branch_name;

select branch_name, count(distinct customer_name)
from depositor, account
where depositor.account_number = account.account_number
group by branch_name;

select branch_name, avg(balance)
from account
where balance > 100
group by branch_name
having avg(balance) > 600
order by branch_name desc;

select depositor.customer_name, avg(balance)
from depositor, account, customer
where depositor.account_number = account.account_number and
depositor.customer_name = customer.customer_name and
customer_city = "Harrison"
group by depositor.customer_name
having count(distinct depositor.account_number) >= 1;

-- Null Values
select loan_number
from loan
where amount is null;

select loan_number
from loan
where amount is not null;

-- Nested Subqueries
-- Set Membership
-- customer names having both savings and loan accounts
select distinct customer_name                   -- set membership using nested query
from borrower
where customer_name in (select customer_name from depositor);

(select distinct customer_name from borrower)   -- set intersction operation
intersect
(select customer_name from depositor);

select distinct d.customer_name                 -- cartesian product
from depositor d, borrower b
where d.customer_name = b.customer_name;

select distinct customer_name                   -- nested sub-queries
from borrower
where customer_name not in (select customer_name from depositor);

(select distinct customer_name from borrower)   -- set difference
except
(select customer_name from depositor);

-- Set Comparisons
select Distinct T.branch_name
from branch as T, branch as S
where t.assets > s.assets and S.branch_city = "BROOKLYN";

select branch_name
from branch
where assets > some (select assets
		     from branch
		     where branch_city = "Brooklyn");
		
select branch_name
from branch
where assets > (select min(assets)
	        from branch
		where branch_city = "Brooklyn");
	
select branch_name
from branch
where assets > all (select assets
		    from branch
		    where branch_city = "Rye");
			
select branch_name
from branch
where assets > (select max(assets)
		from branch
		where branch_city = "Rye");

select branch_name, avg(balance)
from account
group by branch_name;

select branch_name, avg(balance)
from account
group by branch_name
having avg(balance) >= all
	(select avg(balance)
	 from account
	 group by branch_name);

-- Test for Empty Relations
select distinct customer_name
from borrower
where exists (select * from depositor
	      where depositor.customer_name = borrower.customer_name);

(select customer_name from borrower)
intersect
(select customer_name from depositor);

select distinct customer_name
from borrower
where not exists (select * from depositor
		  where depositor.customer_name = borrower.customer_name);

(select customer_name from borrower)
except
(select customer_name from depositor);

select distinct S.customer_name
from depositor as S
where not exists ((select branch_name
		   from branch
		   where branch_city = "Brooklyn")
		  except
		   (select R.branch_name
		    from depositor as T, account as R
		    where T.account_number = R.account_number and
		          S.customer_name = T.customer_name));
                   
-- Views
create view all_customer as
    (select branch_name, customer_name
     from depositor, account
     where depositor.account_number = account.account_number)
    union
    (select branch_name, customer_name
     from borrower, loan
     where borrower.loan_number = loan.loan_number);

select customer_name
from all_customer
where branch_name = "Perryridge";

-- Derived Relations
create table result as
(select branch_name as new_branch_name, avg(balance) as new_avg_balance
 from account
 group by branch_name);

-- drop table result;
select * from result;

------------------------------------------------- Day-2
use korth1;

select distinct S.customer_name
from depositor as S
where not exists ((select branch_name
                   from branch
		   where branch_city = "Brooklyn")
		 except
		  (select R.branch_name
		   from depositor as T, account as R
		   where T.account_number = R.account_number and
		         S.customer_name = T.customer_name));
                   
(select branch_name from branch
 where branch_city = "Brooklyn");
 
create view branch_total_loan(branch_name, total_loan) as
select branch_name, sum(amount)
from loan
group by branch_name;

select * from branch_total_loan;

-- Modification of the Database
-- Deletion
delete from loan;

delete from account
where branch_name = "Brighton";

delete from loan
where amount between 1300 and 1500;

select branch_name
from branch
where branch_city = "Brooklyn";

select * from account;
select count(*) from account;

delete from account
where branch_name in (
	select branch_name
	from branch
	where branch_city = "Brooklyn");
    
select * from account;
select count(*) from account;

delete from account
	where balance < (select avg(amount)
	from loan);
    
select * from account;
select count(*) from account;

insert into account
	(select loan_number, branch_name, 200
	 from loan
	 where branch_name = "Perryridge");

select * from loan;
select count(*) from loan where branch_name = "Perryridge";
select * from account;
select count(*) from account;

select * from depositor;
select count(*) from depositor;

insert into depositor (customer_name, account_number)
(select borrower.customer_name, loan.loan_number
from borrower, loan
where borrower.loan_number = loan.loan_number and
branch_name = "Perryridge");

select * from depositor;
select count(*) from depositor;
select * from borrower;

select * from account;
select count(*) from account;

insert into account
	(select *
	 from account);
	
select * from account;
select count(*) from account;

insert into account
values ("A-401", null, 1200);

select * from account;

-- Update
select * from account;
update account
set balance = balance * 1.05;
select * from account;

update account
set balance = balance * 1.05
where balance >= 1000;
select * from account;

update account
set balance = balance * 1.05
where balance > (select avg (balance)
from account);   -- this will not work

select * from account;

select * from loan;
create view loan_branch as
	select branch_name, loan_number
	from loan;
insert into loan_branch values ("Kolkata", "L-1001");
select * from loan;

-- create view all_customer as
-- (select branch_name, customer_name
-- from depositor, account
-- where depositor.account_number = account.account_number)
-- union
-- (select branch_name, customer_name
-- from borrower, loan
-- where borrower.loan_number = loan.loan_number); 

select * from all_customer;

insert into all_customer(branch_name, customer_name) values("Kolkata", "Tapan");
select * from loan;
select * from borrower;
select * from depositor;
select * from account;

-- Schema Definition in SQL
create table branch1
	(branch_name char(15),
	 branch_city char(30),
	 assets integer,
	 primary key (branch_name),
	 check (assets >= 0));
insert into branch1(branch_name,branch_city,assets) values('Shyambazar','Kolkata',50000);
insert into branch1(branch_name,branch_city,assets) values('Esplanade','Kolkata',20000);
insert into branch1(branch_name,branch_city,assets) values('Dumdum','Kolkata',30000);
insert into branch1(branch_name,branch_city,assets) values('Garia','Kolkata', -50000);
select * from branch1;

drop table depositor1;
create table depositor1
	(customer_name char(20),
	 account_number char(10),
	 primary key (customer_name, account_number));

insert into depositor1(customer_name, account_number) values('Tapan', 'A-0001');
insert into depositor1(customer_name, account_number) values('Tapan', 'A-0002');
insert into depositor1(customer_name, account_number) values('Bimal', 'A-0001');
insert into depositor1(customer_name, account_number) values('Tapan', 'A-0001');
select * from depositor1;

create table student (
	 name char(15) not null,
	 student_id char(10),
	 degree_level char(15),
	 primary key (student_id),
	 check (degree_level in ("Bachelors", "Masters", "Doctorate")));

insert into student values ("Amal", "Std-001","Masters");
insert into student values ("Kamal", "Std-002","BACHELORS");
insert into student values ("Shyamal", "Std-003","doctorate");
insert into student values ("Bimal", "Std-004","MCA");
select * from student;

describe loan;
alter table loan add commission int;
explain loan;
select * from loan;
update loan set commission = amount * 0.10;
select * from loan;
update loan set commission = null;
select * from loan;

explain loan;
select * from loan;
alter table loan drop commission;
explain loan;
select * from loan;

create index index_amount on loan(amount);
alter table loan drop index index_amount;

</pre>

