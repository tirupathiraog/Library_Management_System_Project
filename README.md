# 📚 Library Management System (SQL Project)

![Library Management System](library.png)


## 🚀 Project Overview

This project demonstrates the implementation of a **Library Management System** using SQL. It showcases **database design**, **CRUD operations**, **CTAS**, and **advanced querying techniques** to manage library operations such as book issues, returns, employees, and members efficiently.

The goal of this project is to display intermediate-level SQL skills and real-world data handling scenarios in a structured and maintainable way.

---

## 🛠️ Tech Stack

* **Database:** MySQL / PostgreSQL
* **Language:** SQL
* **Level:** Intermediate
* **Project Type:** Database Design & Querying Project

---

## 🏗️ Project Structure

### 🔹 Database Name

```sql
library_db
```

### 🔹 Tables Created

| Table Name      | Description                   |
| --------------- | ----------------------------- |
| `branch`        | Library branches and managers |
| `employees`     | Employee details              |
| `members`       | Library member records        |
| `books`         | Book inventory                |
| `issued_status` | Book issue records            |
| `return_status` | Book return history           |

---

## 🔧 Database Setup

### ✅ Create Database

```sql
CREATE DATABASE library_db;
```

### ✅ Create Tables

**Branch Table**

```sql
CREATE TABLE branch (
    branch_id VARCHAR(10) PRIMARY KEY,
    manager_id VARCHAR(10),
    branch_address VARCHAR(30),
    contact_no VARCHAR(15)
);
```

**Employees Table**

```sql
CREATE TABLE employees (
    emp_id VARCHAR(10) PRIMARY KEY,
    emp_name VARCHAR(30),
    position VARCHAR(30),
    salary DECIMAL(10,2),
    branch_id VARCHAR(10),
    FOREIGN KEY (branch_id) REFERENCES branch(branch_id)
);
```

**Members Table**

```sql
CREATE TABLE members (
    member_id VARCHAR(10) PRIMARY KEY,
    member_name VARCHAR(30),
    member_address VARCHAR(30),
    reg_date DATE
);
```

**Books Table**

```sql
CREATE TABLE books (
    isbn VARCHAR(50) PRIMARY KEY,
    book_title VARCHAR(80),
    category VARCHAR(30),
    rental_price DECIMAL(10,2),
    status VARCHAR(10),
    author VARCHAR(30),
    publisher VARCHAR(30)
);
```

**Issued Status Table**

```sql
CREATE TABLE issued_status (
    issued_id VARCHAR(10) PRIMARY KEY,
    issued_member_id VARCHAR(30),
    issued_book_name VARCHAR(80),
    issued_date DATE,
    issued_book_isbn VARCHAR(50),
    issued_emp_id VARCHAR(10),
    FOREIGN KEY (issued_member_id) REFERENCES members(member_id),
    FOREIGN KEY (issued_emp_id) REFERENCES employees(emp_id),
    FOREIGN KEY (issued_book_isbn) REFERENCES books(isbn)
);
```

**Return Status Table**

```sql
CREATE TABLE return_status (
    return_id VARCHAR(10) PRIMARY KEY,
    issued_id VARCHAR(30),
    return_book_name VARCHAR(80),
    return_date DATE,
    return_book_isbn VARCHAR(50),
    FOREIGN KEY (return_book_isbn) REFERENCES books(isbn)
);
```

---

## 🔄 CRUD Operations

### ➕ Insert a Book

```sql
INSERT INTO books VALUES (
'978-2-70728-234-9',
'Wings of Fire',
'Autobiography',
7.00,
'yes',
'APJ Abdul Kalam',
'SVC Publications'
);
```

### ✏️ Update Member Details

```sql
UPDATE members
SET member_name = 'Jack'
WHERE member_id = 'C119';
```

### ❌ Delete Issued Record

```sql
DELETE FROM issued_status
WHERE issued_id = 'IS121';
```

### 📖 Fetch Books Issued by Employee

```sql
SELECT e.emp_id, i.issued_book_name
FROM employees e
JOIN issued_status i
ON e.emp_id = i.issued_emp_id
WHERE emp_id = 'E102';
```

---

## 📊 Data Analysis Queries

### ✅ Members Who Issued More Than One Book

```sql
SELECT issued_emp_id, COUNT(*) AS total_count
FROM issued_status
GROUP BY issued_emp_id
HAVING COUNT(*) > 1;
```

### ✅ Books by Category

```sql
SELECT * FROM books
WHERE category = 'Classic';
```

### ✅ Total Rental Income by Category

```sql
SELECT b.category, SUM(b.rental_price)
FROM books b
JOIN issued_status i
ON b.isbn = i.issued_book_isbn
GROUP BY b.category;
```

### ✅ Recent Members (Last 180 Days)

```sql
SELECT * FROM members
WHERE reg_date >= CURRENT_DATE - INTERVAL '180 days';
```

### ✅ Employees with Branch Managers

```sql
SELECT 
  e1.emp_id, e1.emp_name, e1.position,
  e2.emp_name AS manager_name
FROM employees e1
JOIN branch b
ON e1.branch_id = b.branch_id
LEFT JOIN employees e2
ON e2.emp_id = b.manager_id;
```

### ✅ Books Not Yet Returned

```sql
SELECT *
FROM issued_status i
LEFT JOIN return_status r
ON i.issued_id = r.issued_id
WHERE r.return_id IS NULL;
```

---

## 🏗️ CTAS (Create Table As Select)

### 📌 Book Issue Summary Table

```sql
CREATE TABLE book_issued_count AS
SELECT
    isbn,
    book_title,
    COUNT(issued_id) AS count_issue
FROM books
JOIN issued_status
ON isbn = issued_book_isbn
GROUP BY isbn, book_title;
```

### 📌 Expensive Books Table

```sql
CREATE TABLE expensive_books AS
SELECT * FROM books
WHERE rental_price > 6.00;
```

---

## ✅ Key Features

* Relational database design
* Primary & foreign key implementation
* Aggregate functions
* Subqueries & joins
* CTAS usage
* Analytical SQL queries
* Real-world data modelling

---

## 🎯 Learning Outcomes

* Strong understanding of SQL relationships
* Hands-on practice with real-world business queries
* Query optimization & filtering
* Data summarization and reporting

---

## 📌 How to Run the Project

1. Create database `library_db`
2. Run table creation scripts
3. Insert sample data
4. Execute provided queries
5. Explore results and modify queries

---

## 👨‍💻 Author

**Tirupathi Rao G**
SQL Learner | Data Enthusiast | MCA Student

---

## ⭐ Support

If you like this project, **give it a star ⭐ on GitHub** and feel free to fork or improve it.
