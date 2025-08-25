# SQL-Data-Cleaning-EcommerceII

Here, I have Cleaned the Products table and Customer Table Before I transfer them to PowerBI for visualization and Dashboard Creation

------------------------------------------------ Cleaning customer Table
Select *
FROM customer;


Create Table customer_staging
like customer;


Insert customer_staging
Select *
From customer;


SELECT *
FROM customer_staging;


Select Customer_Name, trim(Customer_Name) as Trimmed_Customer_Name
FROM customer_staging;


Update customer_staging
set Customer_Name = trim(Customer_Name);


Select distinct Order_Status
FROM Customer_staging;


SELECT DISTINCT Order_Status
FROM Customer_staging;


Select Order_status
FROM customer_staging
Where order_status LIKE 'Delive';


UPDATE customer_staging
SET Order_status = 'Delivered'
Where Order_status = 'Delive';


Select distinct Order_Status
FROM Customer_staging;


Select Distinct PaymentMode
FROM Customer_staging;


Update Customer_staging
SET PaymentMode = 'COD'
Where PaymentMode = 'CODE';


Select Distinct PaymentMode
FROM Customer_staging;


Select country
FROM Customer_staging
where country = 'brazil';


Update Customer_staging
SET country = 'Brazil'
Where country = 'brazil';


SELECT *
FROM customer_staging;


Select distinct Region
FROM Customer_staging;


Select Region,
CASE
	WHEN Region LIKE 'West%' THEN 'West'
    WHEN Region LIKE 'Eas%' THEN 'East'
    WHEN Region LIKE 'South%' THEN 'South'
    WHEN Region LIKE 'Central%' THEN 'Central'
END as Updated_Region
FROM Customer_staging;


UPDATE customer_staging
SET Region = 
CASE
	WHEN Region LIKE 'West%' THEN 'West'
    WHEN Region LIKE 'Eas%' THEN 'East'
    WHEN Region LIKE 'South%' THEN 'South'
    WHEN Region LIKE 'Central%' THEN 'Central'
END ;


select Price, round((Price),2) as Rounded_Up
from Customer_staging;


Update Customer_staging
Set Price = round((Price),2);


Select *
from Customer_staging;


Select *,
Row_number() OVER (
Partition By 
Product_ID, Customer_ID, Customer_Name,
Purchase_Date, PaymentMode, Country,
Region, Order_Status, Price,
Quantity, Discount, Profit) as row_num
From Customer_staging;


With cte_duplicate as(
Select *,
Row_number() OVER (
Partition By 
Product_ID, Customer_ID, Customer_Name,
Purchase_Date, PaymentMode, Country,
Region, Order_Status, Price,
Quantity, Discount, Profit) as row_num
From Customer_staging)
Select *
From cte_duplicate
Where row_num > 1;

----------------------------------------------------------- Done Cleaning. Will now transfer data to PowerBI

---------------------------------------------------------Cleaning Products Table
SELECT *
FROM product;

CREATE TABLE products_staging
Like product;

INSERT products_staging
SELECT *
FROM product;

SELECT *
FROM products_staging
Where Category is NULL;


-- Categories [Furniture],[Office Supplies], [Technology]
SELECT Category, Sub_Category
FROM products_staging;


Update Products_staging
SET Category = NULL
Where Category = '';


SELECT t1.Category, t1.Sub_Category, t2.Category, t2.Sub_Category
FROM products_staging t1
JOIN products_staging t2
	ON t1.Sub_Category = t2.Sub_Category
Where T1.Category is NULL AND t2.Category is NOT NULL ;


-- USED CODE TO UPDATE THE TABLE
UPDATE products_staging t1
JOIN products_staging t2
	ON t1.Sub_Category = t2.Sub_Category
SET t1.Category = t2.Category
Where T1.Category is NULL 
AND t2.Category is NOT NULL;

-- Table AFTER UPDATING (NO MORE NULLS)
SELECT *
FROM products_staging
Where Category is NULL;


SELECT *
FROM Products_staging;

------------------------------------------------------ Done Cleaning. Will now transfer data to PowerBI
