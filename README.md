# SQL_BOOK_STORE_PROJECT

CREATE TABLE Books (
    Book_ID SERIAL PRIMARY KEY,
    Title VARCHAR(100),
    Author VARCHAR(100),
    Genre VARCHAR(50),
    Published_Year INT,
    Price NUMERIC(10, 2),
    Stock INT
);

SELECT * FROM BOOKS;
DROP TABLE IF EXISTS customers;
CREATE TABLE Customers (
    Customer_ID SERIAL PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100),
    Phone VARCHAR(15),
    City VARCHAR(50),
    Country VARCHAR(150)
);

SELECT * FROM CUSTOMERS;
DROP TABLE IF EXISTS orders;
CREATE TABLE Orders (
    Order_ID SERIAL PRIMARY KEY,
    Customer_ID INT REFERENCES Customers(Customer_ID),
    Book_ID INT REFERENCES Books(Book_ID),
    Order_Date DATE,
    Quantity INT,
    Total_Amount NUMERIC(10, 2)
);
SELECT * FROM ORDERS;

SELECT * FROM BOOKS;

SELECT * FROM CUSTOMERS;

SELECT * FROM ORDERS;

-- 1) Retrieve all books in the "Fiction" genre:
SELECT * FROM BOOKS
WHERE GENRE = 'Fiction';

-- 2) Find books published after the year 1950:
SELECT * FROM BOOKS
WHERE PUBLISHED_YEAR>1950;


-- 3) List all customers from the Canada:
SELECT * FROM CUSTOMERS
WHERE COUNTRY = 'Canada';

-- 4) Show orders placed in November 2023:
SELECT * FROM ORDERS
WHERE ORDER_DATE BETWEEN '2023-11-01' AND '2023-11-30';

-- 5) Retrieve the total stock of books available:
SELECT SUM(STOCK) AS Total_stock
FROM BOOKS;

-- 6) Find the details of the most expensive book:
SELECT * FROM BOOKS
ORDER BY PRICE DESC
LIMIT 1;

-- 7) Show all customers who ordered more than 1 quantity of a book:
SELECT * FROM ORDERS
WHERE QUANTITY>1;

-- 8) Retrieve all orders where the total amount exceeds $20:
SELECT * FROM ORDERS
WHERE TOTAL_AMOUNT>20;

-- 9) List all genres available in the Books table:
SELECT DISTINCT GENRE FROM BOOKS;

-- 10) Find the book with the lowest stock:
SELECT * FROM BOOKS
ORDER BY STOCK ASC;
--LIMIT 1;

-- 11) Calculate the total revenue generated from all orders:
SELECT SUM(TOTAL_AMOUNT) AS total_revenue
FROM ORDERS;


-- Advance Questions : 

SELECT * FROM BOOKS;

SELECT * FROM CUSTOMERS;

SELECT * FROM ORDERS;

-- 1) Retrieve the total number of books sold for each genre:
SELECT B.GENRE, SUM(O.QUANTITY) AS total_books_sold
FROM ORDERS O
JOIN BOOKS B ON O.BOOK_ID = B.BOOK_ID
GROUP BY B.GENRE;

-- 2) Find the average price of books in the "Fantasy" genre:
SELECT AVG(PRICE) AS AVERAGE_PRICE
FROM BOOKS
WHERE GENRE = 'Fantasy';

-- 3) List customers who have placed at least 2 orders:
SELECT CUSTOMER_ID, COUNT(ORDER_ID) AS ORDER_COUNT
FROM ORDERS
GROUP BY CUSTOMER_ID
HAVING COUNT(ORDER_ID) >=2;

SELECT O.CUSTOMER_ID, C.NAME, COUNT(O.ORDER_ID) AS ORDER_COUNT
FROM ORDERS O
JOIN CUSTOMERS C ON O.CUSTOMER_ID = C.CUSTOMER_ID
GROUP BY O.CUSTOMER_ID, C.NAME
HAVING COUNT(ORDER_ID) >=2;

-- 4) Find the most frequently ordered book:
SELECT BOOK_ID, COUNT(ORDER_ID) AS ORDER_COUNT
FROM ORDERS
GROUP BY BOOK_ID;

SELECT BOOK_ID, COUNT(ORDER_ID) AS ORDER_COUNT
FROM ORDERS
GROUP BY BOOK_ID 
ORDER BY ORDER_COUNT DESC LIMIT 1;

SELECT O.BOOK_ID, B.TITLE, COUNT(O.ORDER_ID) AS ORDER_COUNT
FROM ORDERS O
JOIN BOOKS B ON O.BOOK_ID = B.BOOK_ID
GROUP BY O.BOOK_ID, B.TITLE
ORDER BY ORDER_COUNT DESC LIMIT 1;

-- 5) Show the top 3 most expensive books of 'Fantasy' Genre :
SELECT * FROM BOOKS
WHERE GENRE = 'Fantasy'
ORDER BY PRICE DESC LIMIT 3;

-- 6) Retrieve the total quantity of books sold by each author:
SELECT B.AUTHOR, SUM(O.QUANTITY) AS total_books_sold
FROM ORDERS O
JOIN BOOKS B ON O.BOOK_ID = B.BOOK_ID
GROUP BY B.AUTHOR;

-- 7) List the cities where customers who spent over $30 are located:
SELECT DISTINCT C.CITY, TOTAL_AMOUNT
FROM ORDERS O 
JOIN CUSTOMERS C ON O.CUSTOMER_ID = C.CUSTOMER_ID
WHERE O.TOTAL_AMOUNT > 30;

-- 8) Find the customer who spent the most on orders:
SELECT C.CUSTOMER_ID, C.NAME, SUM(O.TOTAL_AMOUNT) AS total_spent
FROM ORDERS O
JOIN CUSTOMERS C ON O.CUSTOMER_ID = C.CUSTOMER_ID
GROUP BY C.CUSTOMER_ID, C.NAME
ORDER BY TOTAL_SPENT DESC;

--9) Calculate the stock remaining after fulfilling all orders:
SELECT b.book_id, b.title, b.stock, COALESCE(SUM(o.quantity),0) AS Order_quantity,  
	b.stock- COALESCE(SUM(o.quantity),0) AS Remaining_Quantity
FROM books b
LEFT JOIN orders o ON b.book_id=o.book_id
GROUP BY b.book_id ORDER BY b.book_id;
