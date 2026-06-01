****Library Management System using SQL Project --P2****
**Project Overview**
**Project Title: Library Management System**
Level: Intermediate
Database: library_db

This project demonstrates the implementation of a Library Management System using SQL. It includes creating and managing tables, performing CRUD operations, and executing advanced SQL queries. The goal is to showcase skills in database design, manipulation, and querying.

Set up the Library Management System Database: Create and populate the database with tables for branches, employees, members, books, issued status, and return status.
CRUD Operations: Perform Create, Read, Update, and Delete operations on the data.
CTAS (Create Table As Select): Utilize CTAS to create new tables based on query results.

Project Structure
1. Database Setup
Database Creation: Created a database named SQL_PROJECT_02(LIBRARY MANAGEMENT).
Table Creation: Created tables for branches, employees, members, books, issued status, and return status. Each table includes relevant columns and relationships.
```SQL
  CREATE DATABASE SQL_PROJECT_02(LIBRARY MANAGEMENT);
```
```SQL
    CREATE TABLE branch
    (
                branch_id VARCHAR(10) PRIMARY KEY,
                manager_id VARCHAR(10),
                branch_address VARCHAR(30),
                contact_no VARCHAR(15)
    );
```

-- Create table "Employee"
```SQL
    CREATE TABLE employees
    (
                emp_id VARCHAR(10) PRIMARY KEY,
                emp_name VARCHAR(30),
                position VARCHAR(30),
                salary DECIMAL(10,2),
                branch_id VARCHAR(10),
                FOREIGN KEY (branch_id) REFERENCES  branch(branch_id)
    );
```

-- Create table "Members"
```SQL
    CREATE TABLE members
    (
                member_id VARCHAR(10) PRIMARY KEY,
                member_name VARCHAR(30),
                member_address VARCHAR(30),
                reg_date DATE
    );
```


-- Create table "Books"
```SQL
    CREATE TABLE books
    (
                isbn VARCHAR(50) PRIMARY KEY,
                book_title VARCHAR(80),
                category VARCHAR(30),
                rental_price DECIMAL(10,2),
                status VARCHAR(10),
                author VARCHAR(30),
                publisher VARCHAR(30)
    );
```


-- Create table "IssueStatus"
```SQL
      CREATE TABLE issue_status
      (
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
-- Create table "ReturnStatus"
```SQL
    CREATE TABLE return_status
    (
                return_id VARCHAR(10) PRIMARY KEY,
                issued_id VARCHAR(30),
                return_book_name VARCHAR(80),
                return_date DATE,
                return_book_isbn VARCHAR(50),
                FOREIGN KEY (return_book_isbn) REFERENCES books(isbn)
    );
```

**2. CRUD Operations**
Create: Inserted sample records into the books table.
Read: Retrieved and displayed data from various tables.
Update: Updated records in the employees table.
Delete: Removed records from the members table as needed.
