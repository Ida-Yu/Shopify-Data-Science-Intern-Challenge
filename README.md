# Shopify-Data-Science-Intern-Challenge

This project includes the my answer to Shopify [Data Science Intern Challenge](https://docs.google.com/document/d/13VCtoyto9X1PZ74nPI4ZEDdb8hF8LAlcmLH1ZTHxKxE/edit#heading=h.5j27tl9uwcuc). 

The brief answers are listed below.   

Please find the detailed anaylsis in ***Shopify_DS_Challenge.ipynb*** above. The analysis was completed in Jupyter Notebook using Python.  

### Question 1: Given some sample data, write a program to answer the following: click here to access the required data set

On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

a. Think about what could be going wrong with our calculation. Think about a better way to evaluate this data. 

b. What metric would you report for this dataset?

c. What is its value?

### Answer for Q1  

a. By defination, average order value (AOV) is total revenue devided by the number of orders. It is widely used in evaluating online marketing performance and pricing strategy. Given sneakers is a relatively affordable item, it is not reasonable to see a AOV of $3145.13. The potential problem is the dataset contains some large amount outliers, which may caused by fraud (shop 42, user 607) or other mistakes, such as price input typo (shop 78).
A better way to evaluate this data is to calculate the AOV (mean of order amount) after removing the outliers, which is $302.58.

b. We could also use median as the metric to measure the central trend. Median is more robust for dataset with outliers. (This will be shown in below analysis)

c. The median of order amount is $284

### Question 2: For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

a. How many orders were shipped by Speedy Express in total?

b. What is the last name of the employee with the most orders?

c. What product was ordered the most by customers in Germany?

### Answer for Q2

a.
```
SELECT COUNT(*) AS Orders
FROM [Orders] AS o
INNER JOIN Shippers AS s ON o.ShipperID = s.ShipperID
WHERE ShipperName = "Speedy Express";
```
There are 54 orders shipped by Speedy Expresss in total.

b.
```
SELECT e.LastName, COUNT() AS Orders
FROM [Employees] AS e
INNER JOIN [Orders] AS o ON e.EmployeeID = o.EmployeeID
GROUP BY e.EmployeeID
ORDER BY COUNT() DESC
LIMIT 1;
```
The last name of the employee with the most orders is Peacock (with 40 orders).

c.
```
SELECT p.ProductName, SUM(od.Quantity) AS MostOrderedQuantity
FROM [Products] AS p
INNER JOIN [OrderDetails] AS od ON p.ProductID = od.ProductID
INNER JOIN [Orders] AS o ON od.OrderID = o.OrderID
INNER JOIN [Customers] AS c ON o.CustomerID = c.CustomerID
WHERE c.Country = "Germany"
GROUP BY p.ProductName
ORDER BY SUM(od.Quantity) DESC
LIMIT 1;
```
Boston Crab Meat was ordered the most (160 times) by Germany Customers


