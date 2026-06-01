**Library Management System using SQL Project --P2**

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
--Create: Inserted sample records into the books table.
--Read: Retrieved and displayed data from various tables.
--Update: Updated records in the employees table.
--Delete: Removed records from the members table as needed.

Task 1. Create a New Book Record -- "978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.')"
```sql
    INSERT INTO BOOK(isbn,book_title,category,rental_price,status,author,publisher)
    VALUES('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');
```
Task 2: Update an Existing Member's Address
```sql
      UPDATE MEMBERS 
      SET member_address='125 Oak St'
      where member_id = 'c103';
```
Task 3: Delete a Record from the Issued Status Table -- Objective: Delete the record with issued_id = 'IS121' from the issued_status table.
```sql
    DELETE FROM ISSUE_STATUS
    WHERE ISSUED_ID='IS121';
```
Task 4: Retrieve All Books Issued by a Specific Employee -- Objective: Select all books issued by the employee with emp_id = 'E101'.
```sql
    SELECT * FROM ISSUE_STATUS
    WHERE ISSUED_EMP_ID ='E101';
```
Task 5: List Members Who Have Issued More Than One Book -- Objective: Use GROUP BY to find members who have issued more than one book.
```sql
    SELECT ISSUED_EMP_ID,COUNT(*)
    FROM ISSUE_STATUS
    GROUP BY 1
    HAVING COUNT(*)>1;
```
3. CTAS (Create Table As Select)
Task 6: Create Summary Tables: Used CTAS to generate new tables based on query results - each book and total book_issued_cnt**
```sql
    CREATE TABLE BOOK_ISSUED_COUNT AS
    SELECT b.isbn, b.book_title, COUNT(a.issued_id)
    FROM ISSUE_STATUS AS a JOIN BOOK AS b
    ON A.issued_book_isbn = b.isbn
    GROUP BY b.isbn, b.book_title;
```
-- show table ---
```sql
    select * from BOOK_ISSUED_COUNT;
```

4. Data Analysis & Findings
The following SQL queries were used to address specific questions:

Task 7. Retrieve All Books in a Specific Category:
```sql
    SELECT * FROM BOOK
    WHERE category='Classic';
```
Task 8: Find Total Rental Income by Category:
```sql
    SELECT 
    	B.category,
    	SUM(b.rental_price),
    	COUNT(*)
    FROM BOOK AS B
    JOIN ISSUE_STATUS AS I
    ON I.issued_bOok_isbn = b.isbn
    GROUP BY 1;
```
List Members Who Registered in the Last 180 Days:
```sql
    SELECT * FROM MEMBERS 
    WHERE reg_date >= CURRENT_DATE - INTERVAL '180 DAYS';
```
List Employees with Their Branch Manager's Name and their branch details:
```sql
   SELECT 
  	E.emp_id,
  	E.emp_name,
  	E.position,
  	E.salary,
  	B.*,
  	M.emp_name AS manager 
  	FROM EMPLOYEES AS E
  	JOIN BRANCH AS B
  	on E.branch_id = B.branch_id
  	JOIN EMPLOYEES AS M
  	ON M.emp_id = B.manager_id;
```
Task 11. Create a Table of Books with Rental Price Above a Certain Threshold:
```sql
    CREATE TABLE EXPENCIIVE_BOOKS 
    AS
    SELECT * FROM BOOK
    wHERE rental_price > 7.00;
```
Task 12: Retrieve the List of Books Not Yet Returned
```sql
    SELECT * FROM issued_status as ist
    SELECT * FROM ISSUE_STATUS AS I
    LEFT JOIN RETURN_STATUS AS R
    ON I.issued_id = R.issued_id
    WHERE R.return_id IS NULL;
```
