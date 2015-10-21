--# interview
USE [master]
GO
ALTER DATABASE Interview SET SINGLE_USER WITH ROLLBACK IMMEDIATE 
GO
/****** Object:  Database [Interview]    Script Date: 09/21/2015 16:09:02 ******/
IF  EXISTS (SELECT name FROM sys.databases WHERE name = N'Interview')
DROP DATABASE [Interview]

GO
/****** Object:  Database [Interview]    Script Date: 09/21/2015 14:42:28 ******/
CREATE DATABASE [Interview] ON  PRIMARY 
( NAME = N'Interview', FILENAME = N'C:\Users\ebarnes\AppData\Local\Microsoft\Microsoft SQL Server Local DB\Instances\v11.0\Interview.mdf' , SIZE = 4096KB , MAXSIZE = UNLIMITED, FILEGROWTH = 1024KB )
 LOG ON 
( NAME = N'Interview_log', FILENAME = N'C:\Users\ebarnes\AppData\Local\Microsoft\Microsoft SQL Server Local DB\Instances\v11.0\Interview_log.ldf' , SIZE = 1024KB , MAXSIZE = 2048GB , FILEGROWTH = 10%)
GO

IF (1 = FULLTEXTSERVICEPROPERTY('IsFullTextInstalled'))
begin
EXEC [Interview].[dbo].[sp_fulltext_database] @action = 'enable'
end
GO

ALTER DATABASE [Interview] SET ANSI_NULL_DEFAULT OFF 
GO

ALTER DATABASE [Interview] SET ANSI_NULLS OFF 
GO

ALTER DATABASE [Interview] SET ANSI_PADDING OFF 
GO

ALTER DATABASE [Interview] SET ANSI_WARNINGS OFF 
GO

ALTER DATABASE [Interview] SET ARITHABORT OFF 
GO

ALTER DATABASE [Interview] SET AUTO_CLOSE OFF 
GO

ALTER DATABASE [Interview] SET AUTO_CREATE_STATISTICS ON 
GO

ALTER DATABASE [Interview] SET AUTO_SHRINK OFF 
GO

ALTER DATABASE [Interview] SET AUTO_UPDATE_STATISTICS ON 
GO

ALTER DATABASE [Interview] SET CURSOR_CLOSE_ON_COMMIT OFF 
GO

ALTER DATABASE [Interview] SET CURSOR_DEFAULT  GLOBAL 
GO

ALTER DATABASE [Interview] SET CONCAT_NULL_YIELDS_NULL OFF 
GO

ALTER DATABASE [Interview] SET NUMERIC_ROUNDABORT OFF 
GO

ALTER DATABASE [Interview] SET QUOTED_IDENTIFIER OFF 
GO

ALTER DATABASE [Interview] SET RECURSIVE_TRIGGERS OFF 
GO

ALTER DATABASE [Interview] SET  DISABLE_BROKER 
GO

ALTER DATABASE [Interview] SET AUTO_UPDATE_STATISTICS_ASYNC OFF 
GO

ALTER DATABASE [Interview] SET DATE_CORRELATION_OPTIMIZATION OFF 
GO

ALTER DATABASE [Interview] SET TRUSTWORTHY OFF 
GO

ALTER DATABASE [Interview] SET ALLOW_SNAPSHOT_ISOLATION OFF 
GO

ALTER DATABASE [Interview] SET PARAMETERIZATION SIMPLE 
GO

ALTER DATABASE [Interview] SET READ_COMMITTED_SNAPSHOT OFF 
GO

ALTER DATABASE [Interview] SET HONOR_BROKER_PRIORITY OFF 
GO

ALTER DATABASE [Interview] SET  READ_WRITE 
GO

ALTER DATABASE [Interview] SET RECOVERY SIMPLE 
GO

ALTER DATABASE [Interview] SET  MULTI_USER 
GO

ALTER DATABASE [Interview] SET PAGE_VERIFY CHECKSUM  
GO

ALTER DATABASE [Interview] SET DB_CHAINING OFF 
GO

USE Interview
GO

CREATE TABLE Products
(
	ProductId int IDENTITY(1,1),
	ProductName VARCHAR(100),
	Price MONEY
)

CREATE TABLE Customers
(
	CustomerID int IDENTITY(1,1),
	CustomerName VARCHAR (100)
)


CREATE TABLE Orders
(
	OrderId int IDENTITY(1,1),
	CustomerId int,
	OrderDate datetime
)

CREATE Table Sales
(
	SaleId int IDENTITY(1,1),
	OrderId int,
	ProductId int,
)


INSERT INTO Customers
(
	CustomerName
)
VALUES
('UHEAA'),
('USHE'),
('OCHE'),
('SBR'),
('NewCustomer')


INSERT INTO Products
(
	ProductName,
	Price
)
VALUES
('Product 1', 5.01),
('Product 2', 4.99),
('Product 3', 195.75),
('Product 4', 18.43),
('Product 5', 622.55),
('Product 6', 5.00),
('Product 7', 1.99),
('Product 8', 15.71),
('Product 9', 1.42),
('Product 10', 623.15)

INSERT INTO Orders
(
	CustomerId,
	OrderDate
)
VALUES
(1, DATEADD(dd, -380, GETDATE())),
(1, DATEADD(dd, -59, GETDATE())),
(1, DATEADD(dd, -20, GETDATE())),
(2, DATEADD(dd, -21, GETDATE())),
(2, DATEADD(dd, -23, GETDATE())),
(2, DATEADD(dd, -24, GETDATE())),
(2, DATEADD(dd, -25, GETDATE())),
(2, DATEADD(dd, 10, GETDATE())),
(3, DATEADD(dd, -30, GETDATE())),
(3, DATEADD(dd, -140, GETDATE())),
(3, DATEADD(dd, -50, GETDATE())),
(4, DATEADD(dd, -10, GETDATE())),
(3, DATEADD(dd, -49, GETDATE())),
(2, DATEADD(dd, -29, GETDATE())),
(1, DATEADD(dd, -27, GETDATE())),
(2, DATEADD(dd, -23, GETDATE())),
(2, DATEADD(dd, -24, GETDATE())),
(4, DATEADD(dd, -25, GETDATE())),
(4, DATEADD(dd, 11, GETDATE())),
(3, DATEADD(dd, -31, GETDATE())),
(4, DATEADD(dd, -43, GETDATE())),
(4, DATEADD(dd, -55, GETDATE()))

--############################################

INSERT INTO Sales
(
	OrderId,
	ProductId
)
VALUES
(1, 1),
(1, 5),
(1, 6),
(1, 7),
(2, 8),
(2, 8),
(3, 3),
(4, 2),
(4, 1),
(4, 3),
(5, 10),
(5, 9),
(5, 8),
(6, 7),
(6, 2),
(7, 1),
(7, 10),
(8, 8),
(8, 8),
(9, 3),
(9, 2),
(10, 1),
(10, 3),
(10, 3),
(10, 4),
(11, 3),
(12, 10),
(13, 9),
(13, 8),
(13, 7),
(13, 2),
(14, 1),
(14, 10),
(15, 8),
(15, 3),
(15, 2),
(15, 1),
(16, 3),
(16, 10),
(17, 9),
(18, 8),
(18, 7),
(18, 2),
(18, 1),
(18, 10)

-- Write a query that will return a list of customers that have never placed an order
SELECT
	C.CustomerName
FROM
	Customers C
	LEFT JOIN Orders O on O.CustomerId = C.CustomerID
WHERE
	O.OrderId is NULL


--Write a single select statement that will return the number of products with an Price of $4.00 or less, and also the number of products with an Price of greater than $5.00
--The results should be returned in seperate columns
SELECT
	SUM(CASE  
		WHEN P.Price <= 4.00 THEN 1 ELSE 0
	END) [Under5],
	SUM(CASE	
		WHEN P.Price > 5.00 THEN 1 ELSE 0
	END) [Over5]
FROM
	Products P


--Write a query that will return Customer Name, Month of Sale, and Total Sales
--Show the total by month.
--Only include customers "OCHE" AND "UHEAA"
--Only include sales that have occured in the last 12 months. 
--Exclude months that have sales totaling less than $32.00
--Order the results by the total sales per month starting with the highest total sales

SELECT
	C.CustomerName,
	MONTH(O.OrderDate) [Mnth],
	SUM(P.Price) [TotalSales]
FROM
	Orders O
	INNER JOIN Customers C on C.CustomerID = O.CustomerId
	INNER JOIN Sales S on S.OrderId = O.OrderId
	INNER JOIN Products P on P.ProductId = S.ProductId
WHERE
	C.CustomerName in ('OCHE', 'UHEAA')
	AND
	O.OrderDate >= DATEADD(MONTH, -12, GETDATE())
	--DATEDIFF(MONTH, O.OrderDate , GETDATE()) <= 12
GROUP BY
	C.CustomerName,
	MONTH(O.OrderDate)
HAVING
	SUM(P.Price) >= 32.00
ORDER BY 
	[TotalSales] DESC


--Write statements that when executed will add a sale of "Product 3" to customer "UHEEA" occuring today.
DECLARE @OrderId int

INSERT INTO
	Orders
(
	CustomerId,
	OrderDate
)
VALUES
(
	1,
	GETDATE()
)

SET @OrderId = SCOPE_IDENTITY()

INSERT INTO
	Sales
(
	OrderId,
	ProductId
)
VALUES
(
	@OrderId,
	3 -- "Product 3"
)


-- Write a query that will return the Customer Name, the amount of their most expensive product purchased, and the date of their most recent purchase of that product
SELECT
	C.CustomerName,
	X.Price_max,
	MAX(O.OrderDate) [OrderDate]
FROM
	Customers C
	LEFT JOIN Orders O on O.CustomerId = C.CustomerID
	LEFT JOIN Sales S on S.OrderId = O.OrderId
	LEFT JOIN Products P on P.ProductId = S.ProductId
	LEFT JOIN
	( -- most expensive purchase by customer
		SELECT
			O.CustomerId,
			MAX(ISNULL(P.Price, 0)) [Price_max]
		FROM
			Orders O
			INNER JOIN Sales S on S.OrderId = O.OrderId
			INNER JOIN Products P on P.ProductId = S.ProductId
		GROUP BY
			O.CustomerId
	) X on X.CustomerId = C.CustomerID AND P.Price = X.Price_max
WHERE
	X.Price_max is not NULL
GROUP BY
	C.CustomerName,
	X.Price_max


SELECT
	X.CustomerName,
	X.Price_max,
	MAX(X.OrderDate) [OrderDate]
FROM
	(
		SELECT
			C.CustomerName,
			O.OrderDate,
			MAX(P.Price) OVER (PARTITION BY C.CustomerId) [Price_max]
		FROM
			Customers C
			LEFT JOIN Orders O on O.CustomerId = C.CustomerID
			LEFT JOIN Sales S on S.OrderId = O.OrderId
			LEFT JOIN Products P on P.ProductId = S.ProductId
	) X
GROUP BY
	X.CustomerName,
	X.Price_max
