# SQL-Projects

/*
Cleaning Data in SQL Queries
*/


SELECT  *
FROM vending_machine_sales;


--  Different products in vending machine 

SELECT  distinct product
FROM vending_machine_sales;




--  Different vending machine 

SELECT  distinct DeviceID
FROM vending_machine_sales;

-- Count of distinct products

SELECT  count(distinct product) 
FROM vending_machine_sales;




-- finding device id of missing product 

SELECT DeviceID, Machine, Product
FROM vending_machine_sales
WHERE Product IS NULL;


SET SQL_SAFE_UPDATES = 0;

UPDATE vending_machine_sales
SET Product= NULL
WHERE Product IS NULL;



-- finding device id where quantity is missing

SELECT DeviceID, Machine, Product, RQty
FROM vending_machine_sales
WHERE RQty IS NULL;

UPDATE vending_machine_sales
SET RQty=0
WHERE RQty IS NULL;

SET SQL_SAFE_UPDATES = 1;



-- Casting data type of date

SELECT  cast(pdate AS DATE) 
FROM vending_machine_sales;




-- categorizing cost into low, medium and high

SELECT  DeviceID, Product, TransTotal,
CASE WHEN Transtotal< 3 THEN '1. Low'
     WHEN Transtotal BETWEEN 3 AND 5 THEN '2. Medium'
     WHEN Transtotal> 5 THEN '3. High'
     END AS Cost_range
FROM vending_machine_sales
ORDER BY c_range;


ALTER TABLE  vending_machine_sales
ADD c_range varchar(10);


UPDATE vending_machine_sales
SET c_range=CASE WHEN Transtotal< 3 THEN '1. Low'
     WHEN Transtotal BETWEEN 3 AND 5 THEN '2. Medium'
     WHEN Transtotal> 5 THEN '3. High'
     END; 
 


--  finding multiple entries

SELECT  *
FROM vending_machine_sales;

SELECT  DeviceID, Machine, Transaction, count(Transaction) AS Duplicate_Transactions
FROM vending_machine_sales
GROUP BY Transaction
HAVING count(Transaction)>1;

WITH Rownum AS(
SELECT *, ROW_NUMBER () Over (PARTITION BY Transaction ORDER BY Transaction) AS rownumber
from vending_machine_sales 
)

DELETE
FROM Rownum
WHERE rownumber > 1;


-- Other method

CREATE TABLE copy_vending_machine_sales
SELECT Transaction, Status, DeviceID, Location, Machine, 
        Product, Category, TransDate,RQty,TransTotal,pdate
FROM vending_machine_sales
GROUP BY transaction ;



SELECT *
FROM copy_vending_machine_sales;


SELECT  DeviceID, Machine, Transaction, count(Transaction) AS Duplicate_Transactions
FROM copy_vending_machine_sales
GROUP BY Transaction
HAVING count(Transaction)>1;


drop table vending_machine_sales;

ALTER TABLE copy_vending_machine_sales
RENAME TO vending_machine_sales;

SELECT *
FROM vending_machine_sales;

SELECT  DeviceID, Machine, Transaction, count(Transaction) AS Duplicate_Transactions
FROM vending_machine_sales
GROUP BY Transaction
HAVING count(Transaction)>1;
