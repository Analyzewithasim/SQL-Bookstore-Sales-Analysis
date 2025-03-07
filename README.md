# SQL-Bookstore-Sales-Analysis

# 📚 SQL Bookstore Sales Analysis

## 📌 Project Overview
This end-to-end SQL project analyzes **bookstore sales, customer trends, and inventory management** using PostgreSQL. The dataset consists of **Books, Customers, and Orders** tables, allowing for comprehensive sales and business insights.

## 🏗️ Database Schema
The project contains three tables:
- **Books** 📖 (Book details, price, stock levels)
- **Customers** 🧑 (Customer details, location, contact info)
- **Orders** 🛒 (Purchase transactions, total amount spent)

## 🔧 Technologies Used
- **SQL (PostgreSQL)**
- **ETL (Data Import using COPY command)**
- **Data Analysis & Aggregation**

## 📂 Files in this Repository
- `schema.sql` → Creates the database tables
- `data_import.sql` → Imports CSV data into tables
- `queries.sql` → SQL queries for insights & analysis

## 🔍 Key Insights
✅ **Top-selling book genres** and most frequently ordered books
✅ **Customer spending trends** and high-value customers
✅ **Real-time stock tracking** after sales fulfillment
✅ **Total revenue generated** for business performance

## 🚀 How to Use This Project
1. Clone the repo:
   ```bash
   git clone https://github.com/your-username/sql-bookstore-analysis.git
   cd sql-bookstore-analysis
   ```
2. Run `schema.sql` in PostgreSQL to create tables.
3. Import data using `data_import.sql`.
4. Execute `queries.sql` to generate insights.

## 📊 Queries:
```sql
-- Books Tables
CREATE TABLE Books (
    Book_ID SERIAL PRIMARY KEY,
    Title VARCHAR(100),
    Author VARCHAR(100),
    Genre VARCHAR(50),
    Published_Year INT,
    Price NUMERIC(10, 2),
    Stock INT
);
-- Customers Table
CREATE TABLE Customers (
    Customer_ID SERIAL PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100),
    Phone VARCHAR(15),
    City VARCHAR(50),
    Country VARCHAR(150)
);

-- Orders Table
CREATE TABLE Orders (
    Order_ID SERIAL PRIMARY KEY,
    Customer_ID INT REFERENCES Customers(Customer_ID),
    Book_ID INT REFERENCES Books(Book_ID),
    Order_Date DATE,
    Quantity INT,
    Total_Amount NUMERIC(10, 2)
);
```
**Import Data into Books Table**
```sql
COPY Books(Book_ID, Title, Author, Genre, Published_Year, Price, Stock) 
FROM 'F:\SQL\Project\Books.csv' 
CSV HEADER;

SELECT * FROM Books;
```
**Import Data into Customers Table**
```sql
COPY Customers(Customer_ID, Name, Email, Phone, City, Country) 
FROM 'F:\SQL\Project\Customers.csv' 
CSV HEADER;
```
SELECT * FROM Customers;

**Import Data into Orders Table**
```sql
COPY Orders(Order_ID, Customer_ID, Book_ID, Order_Date, Quantity, Total_Amount) 
FROM 'F:\SQL\Project\Orders.csv' 
CSV HEADER;

SELECT * FROM Orders;
```
```sql
--Retrieve all books in the "Fiction" genre:

SELECT *
FROM Books
WHERE genre = 'Fiction';

--Find books published after the year 1950:

SELECT *
FROM Books
WHERE published_year > 1950
ORDER BY published_year;

--List all customers from the Canada:
SELECT * FROM Customers;

SELECT *
FROM Customers
WHERE country = 'Canada';

--Show orders placed in November 2023:

SELECT * FROM Orders;

SELECT * FROM Orders
WHERE order_date BETWEEN '2023-11-01' AND '2023-11-30';

--Retrieve the total stock of books available:
SELECT * FROM Books;

SELECT SUM(stock) AS T_S_A
FROM Books;

--Find the details of the most expensive book:
SELECT *
FROM Books;

SELECT * FROM Books
ORDER BY price DESC
LIMIT 1;

SELECT * FROM Customers;

--Show all customers who ordered more than 1 quantity of a book:

SELECT c.customer_id, c.name, o.quantity
FROM Customers c
JOIN Orders o
ON c.customer_id = o.customer_id
WHERE o.quantity > 1
ORDER BY o.quantity;

SELECT * FROM Orders
WHERE quantity > 1;

--Retrieve all orders where the total amount exceeds $20:
SELECT * FROM Books;

SELECT * FROM Orders
WHERE total_amount > 20;

--List all genres available in the Books table:
SELECT * FROM Books

SELECT DISTINCT genre
FROM Books;

--Find the book with the lowest stock:

SELECT *
FROM Books
ORDER BY stock
LIMIT 5;

--Calculate the total revenue generated from all orders:

SELECT * FROM Orders

SELECT SUM(total_amount) AS TRG
FROM Orders;

--Retrieve the total number of books sold for each genre:

SELECT b.genre, SUM(o.quantity) AS total_books_sold
FROM Books b
JOIN Orders o 
ON b.book_id = o.book_id
GROUP BY b.genre;

--Find the average price of books in the "Fantasy" genre:

SELECT * FROM Books;

SELECT AVG(price) AS Fantasy_AVG_Price
FROM Books
WHERE genre = 'Fantasy';

--List customers who have placed at least 2 orders:

SELECT * FROM Customers;
SELECT * FROM Orders;

SELECT customer_id, COUNT(order_id) AS Order_count
FROM Orders
GROUP BY customer_id
HAVING COUNT(order_id) >=2;

-- By Joining 2 tables if want to show customer name

SELECT o.customer_id, c.name, COUNT(o.order_id) AS Order_count
FROM Orders o
JOIN Customers c
ON o.customer_id = c.customer_id
GROUP BY o.customer_id, c.name
HAVING COUNT(o.order_id) >=2;

--Find the most frequently ordered book:

SELECT book_id, COUNT(order_id) AS Order_count
FROM Orders
GROUP BY Book_id
ORDER BY Order_Count DESC
LIMIT 1;

-- By Joining 2 tables if want to show with the book name

SELECT o.book_id, b.title, COUNT(o.order_id) AS Order_Count
FROM Orders o
JOIN Books b ON
o.book_id = b.book_id
GROUP BY o.book_id, b.title
ORDER BY Order_Count DESC
LIMIT 1;

--Show the top 3 most expensive books of 'Fantasy' Genre :

SELECT title, genre, price
FROM Books
WHERE genre = 'Fantasy'
ORDER BY price DESC
LIMIT 3;

--Retrieve the total quantity of books sold by each author:

SELECT * FROM Books;
SELECT * FROM Orders;

SELECT b.author, SUM(o.quantity) AS Total_Quantity_sold
FROM Books b
JOIN Orders o
ON b.book_id = o.book_id
GROUP BY b.author;

--List the cities where customers who spent over $30 are located:

SELECT * FROM Customers;
SELECT * FROM Books;
SELECT * FROM Orders;

SELECT DISTINCT c.city, o.total_amount
FROM Orders o
JOIN Customers c
ON o.customer_id = c.customer_id
WHERE o.total_amount > 30;

--Find the customer who spent the most on orders:

SELECT c.name, c.customer_id, SUM(o.total_amount)
FROM Customers c
JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.name, c.customer_id
ORDER BY SUM(o.total_amount) DESC
LIMIT 1;

--Calculate the stock remaining after fulfilling all orders:
SELECT b.book_id, b.title, b.stock, COALESCE(SUM(o.quantity),0) AS Order_Quantity,
b.stock-COALESCE(SUM(o.quantity),0) AS Remaining_Quantity
FROM Books b
LEFT JOIN Orders o
ON b.book_id = o.book_id
GROUP BY b.book_id ORDER BY b.book_id;


```

## 🏆 Future Enhancements
- **Interactive dashboards using Power BI / Tableau**
- **Stored procedures & indexing for query optimization**
- **Machine learning for sales forecasting**

📩 **Let’s Connect!** If you have feedback or suggestions, feel free to contribute! 🎯
