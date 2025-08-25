# SQL-Data-Cleaning-EcommerceII

Here, I have Cleaned the Products table and Customer Table Before I transfer them to PowerBI for visualization and Dashboard Creation

------------------------------------------------ Cleaning customer Table <br>
Select * <br>
FROM customer; <br>
<br>
<br>
Create Table customer_staging <br>
like customer; <br>
<br>
<br>
Insert customer_staging <br>
Select *<br>
From customer;<br>
<br>
<br>
SELECT *<br>
FROM customer_staging;<br>
<br>
<br>
Select Customer_Name, trim(Customer_Name) as Trimmed_Customer_Name<br>
FROM customer_staging;<br>
<br>
<br>
Update customer_staging<br>
set Customer_Name = trim(Customer_Name);<br>
<br>
<br>
Select distinct Order_Status<br>
FROM Customer_staging;<br>
<br>
<br>
SELECT DISTINCT Order_Status<br>
FROM Customer_staging;<br>
<br>
<br>
Select Order_status<br>
FROM customer_staging<br>
Where order_status LIKE 'Delive';<br>
<br>
<br>
UPDATE customer_staging<br>
SET Order_status = 'Delivered'<br>
Where Order_status = 'Delive';<br>

<br><br>
Select distinct Order_Status<br>
FROM Customer_staging;<br>
<br><br>

Select Distinct PaymentMode<br>
FROM Customer_staging;<br>
<br>
<br>
Update Customer_staging<br>
SET PaymentMode = 'COD'<br>
Where PaymentMode = 'CODE';<br>
<br>
<br>
Select Distinct PaymentMode<br>
FROM Customer_staging;<br>
<br><br>

Select country<br>
FROM Customer_staging<br>
where country = 'brazil';<br>
<br>
<br>
Update Customer_staging<br>
SET country = 'Brazil'<br>
Where country = 'brazil';<br>
<br><br>

SELECT *<br>
FROM customer_staging;<br>
<br><br>

Select distinct Region<br>
FROM Customer_staging;<br>
<br>
<br>
Select Region,<br>
CASE<br>
	WHEN Region LIKE 'West%' THEN 'West'<br>
    WHEN Region LIKE 'Eas%' THEN 'East'<br>
    WHEN Region LIKE 'South%' THEN 'South'<br>
    WHEN Region LIKE 'Central%' THEN 'Central'<br>
END as Updated_Region<br>
FROM Customer_staging;<br>
<br><br>

UPDATE customer_staging<br>
SET Region = <br>
CASE<br>
	WHEN Region LIKE 'West%' THEN 'West'<br>
    WHEN Region LIKE 'Eas%' THEN 'East'<br>
    WHEN Region LIKE 'South%' THEN 'South'<br>
    WHEN Region LIKE 'Central%' THEN 'Central'<br>
END ;<br>
<br><br>

select Price, round((Price),2) as Rounded_Up<br>
from Customer_staging;<br>
<br><br>

Update Customer_staging<br>
Set Price = round((Price),2);<br>

<br><br>
Select *<br>
from Customer_staging;<br>

<br><br>
Select *,<br>
Row_number() OVER (<br>
Partition By <br>
Product_ID, Customer_ID, Customer_Name,<br>
Purchase_Date, PaymentMode, Country,<br>
Region, Order_Status, Price,<br>
Quantity, Discount, Profit) as row_num<br>
From Customer_staging;<br>
<br>
<br>
With cte_duplicate as(<br>
Select *,<br>
Row_number() OVER (<br>
Partition By <br>
Product_ID, Customer_ID, Customer_Name,<br>
Purchase_Date, PaymentMode, Country,<br>
Region, Order_Status, Price,<br>
Quantity, Discount, Profit) as row_num<br>
From Customer_staging)<br>
Select *<br>
From cte_duplicate<br>
Where row_num > 1;<br>
<br>
----------------------------------------------------------- Done Cleaning. Will now transfer data to PowerBI<br>
<br>
---------------------------------------------------------Cleaning Products Table<br>
SELECT *<br>
FROM product;<br>
<br>
CREATE TABLE products_staging<br>
Like product;<br>
<br>
INSERT products_staging<br>
SELECT *<br>
FROM product;<br>
<br>
SELECT *<br>
FROM products_staging<br>
Where Category is NULL;<br>

<br><br>
-- Categories [Furniture],[Office Supplies], [Technology]<br>
SELECT Category, Sub_Category<br>
FROM products_staging;<br>
<br>
<br>
Update Products_staging<br>
SET Category = NULL<br>
Where Category = '';<br>
<br><br>

SELECT t1.Category, t1.Sub_Category, t2.Category, t2.Sub_Category<br>
FROM products_staging t1<br>
JOIN products_staging t2<br>
	ON t1.Sub_Category = t2.Sub_Category<br>
Where T1.Category is NULL AND t2.Category is NOT NULL ;<br>
<br><br>

-- USED CODE TO UPDATE THE TABLE<br>
UPDATE products_staging t1<br>
JOIN products_staging t2<br>
	ON t1.Sub_Category = t2.Sub_Category<br>
SET t1.Category = t2.Category<br>
Where T1.Category is NULL <br>
AND t2.Category is NOT NULL;<br>
<br>
-- Table AFTER UPDATING (NO MORE NULLS)<br>
SELECT *<br>
FROM products_staging<br>
Where Category is NULL;<br>
<br><br>

SELECT *<br>
FROM Products_staging;<br>
<br>
------------------------------------------------------ Done Cleaning. Will now transfer data to PowerBI
