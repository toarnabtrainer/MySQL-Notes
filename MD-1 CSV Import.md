# Fetching data to Database Tables from CSV Files

<pre>
-- to drop if the database pre-exists
DROP DATABASE text_db;

-- to create a database (Green mark in O/P)
CREATE DATABASE test_db;

-- if we execute the same command then error will be shown (Red mark in O/P)
CREATE DATABASE test_db;

-- to use the database
USE test_db;

-- now creating the table stocks
-- according to the data source CSV file name is "data.csv"
CREATE TABLE stocks (
	TradeDate CHAR(10),
	SPY double,
	GLD double,
	AMZN double,
	GOOG double,
	KPTI double,
	GILD double,
	MPC double);

-- Showing empty table
SELECT * FROM stocks;

-- Right click on the database test_db in the left panel and select "Table Data Import Wizard"
-- And select the CSV file (E:\Arnab Docs\MySQL\data.csv) and select propoer table and check the mapping of the columns/attributes
-- select Next for couple of number of times and complete the process
-- Showing current content of the table
SELECT * FROM stocks;

-- If the first date column content is not in the proper format the use the following command
SET SQL_SAFE_UPDATES = 0;
UPDATE stocks SET TradeDate = str_to_date(TradeDate, "%m%d%y");

-- Now repeat the same "Table Data Import Wizard", but go for a new table
-- Select the datatype of Date attribute as datetime, and format as %Y-%m-%d
-- complete the data fetching process.
-- Showing current content of the table
SELECT * FROM data;

-- Renaming the "Date" column to "TradeDate"
ALTER TABLE data CHANGE Date TradeDate Date;

-- Showing current content of the table
SELECT * FROM data;

</pre>
