📚 Book Store Management System (PostgreSQL)

A relational database project built using PostgreSQL to manage a Book Store’s inventory, customers, and orders.
This project demonstrates strong fundamentals in SQL, database design, normalization, and data querying.

🚀 Features
Manage book inventory (price, stock, author)
Store customer details
Track orders and purchase history
Perform real-world SQL queries (JOINs, subqueries, aggregations)
CSV data import using PostgreSQL

🛠 Tech Stack
Database: PostgreSQL
Language: SQL
Tools: pgAdmin / psql

🗂 Database Schema
Tables
Books
Customers
Orders

Relationships:
One customer → many orders
One book → many orders

🧾 Table Structure
📘 Books
Column	Type
book_id	SERIAL (PK)
title	VARCHAR
author	VARCHAR
price	NUMERIC
stock	INT

👤 Customers
Column	Type
customer_id	SERIAL (PK)
name	VARCHAR
email	VARCHAR
phone	VARCHAR

🛒 Orders
Column	Type
order_id	SERIAL (PK)
customer_id	INT (FK)
book_id	INT (FK)
quantity	INT
order_date	DATE


🔑 Key SQL Queries Implemented
List all available books
Find customers with highest purchases
Total revenue generated
Books with low stock
Orders placed in the last 30 days
JOIN queries across multiple tables

