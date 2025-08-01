# Library-System-Management-
```sql
SELECT * FROM books;
SELECT * FROM branch;
SELECT * FROM employees;
SELECT * FROM issued_status;
SELECT * FROM return_status;
SELECT * FROM members;
```

-- Project Task

-- Task 1. Create a New Book Record -- "978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.')"
```sql
INSERT INTO books(isbn, book_title, category, rental_price, status, author, publisher)
VALUES
('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');
SELECT * FROM books;
```

-- Task 2: Update an Existing Member's Address
```sql
UPDATE members
SET member_address = '892 Maple St'
WHERE member_id = 'C105';
SELECT * FROM members;
```

-- Task 3: Delete a Record from the Issued Status Table. 
-- Objective: Delete the record with issued_id = 'IS121' from the issued_status table.
```sql
SELECT * FROM issued_status
WHERE issued_id = 'IS121';

DELETE FROM issued_status
WHERE issued_id = 'IS121'
```


-- Task 4: Retrieve All Books Issued by a Specific Employee.
-- Objective: Select all books issued by the employee with emp_id = 'E101'.
```sql
SELECT * FROM issued_status
WHERE issued_emp_id = 'E101';
```
-- Task 5: List Members Who Have Issued More Than One Book. 
-- Objective: Use GROUP BY to find members who have issued more than one book.
```sql
SELECT 
	issued_emp_id,
	COUNT(*) AS total_books
FROM
	issued_status
GROUP BY 1
HAVING COUNT(*) > 1
ORDER BY total_books DESC
````
-- CTAS(CREATE TABLE AS SUMMARY)
-- Task 6: Create Summary Tables: Used CTAS to generate new tables based on query results - each book and total book_issued_cnt**
```sql
CREATE TABLE total_book_cnt 
AS
SELECT 
	b.isbn,
	b.book_title,
	COUNT(i.issued_id) AS total_book_issued
FROM books b	
JOIN issued_status i ON i.issued_book_isbn = b.isbn
GROUP BY 1, 2
```
-- Task 7. Retrieve All Books in a Specific Category:
-- Objective: Retrieve all books who's category is 'History'.
```sql
SELECT * FROM books
WHERE category = 'History'
````
-- Task 8: Find Total Rental Income by Category:
```sql
SELECT * FROM issued_status
SELECT * FROM books

SELECT
	b.category,
	SUM(rental_price) AS total_rental_income,
	COUNT(*) 
FROM 
	issued_status i
JOIN books b ON i.issued_book_isbn = b.isbn
GROUP BY 1
ORDER BY 2 DESC
```
-- Task 9: List Members Who Registered in the Last 180 Days:
```sql
INSERT INTO members(member_id, member_name, member_address, reg_date)
VALUES('C120', 'Franky', '284 Cedar St', '2025-02-02');


INSERT INTO members(member_id, member_name, member_address, reg_date)
VALUES('C121', 'Brook', '850 Maple St', '2025-02-02');

SELECT * FROM members
WHERE reg_date >= CURRENT_DATE - INTERVAL '180 days';
```
-- Task 10: List Employees with Their Branch Manager's Name and their branch details:
```sql
SELECT * FROM employees
SELECT * FROM branch

SELECT 
	e.*,
	b.manager_id,
	m.emp_name AS manager_name
FROM 
	employees e
JOIN branch b ON e.branch_id = b.branch_id
JOIN employees m ON m.emp_id = b.manager_id
```
-- Task 11. Create a Table of Books with Rental Price Above a Certain Threshold Minimum 8USD:
```sql
CREATE TABLE rental_book_price AS
SELECT * FROM books
WHERE rental_price >= 8
```
-- Task 12: Retrieve the List of Books Not Returned Yet
```sql
SELECT * FROM return_status
SELECT * FROM issued_status

SELECT DISTINCT i.issued_book_name
FROM issued_status i
LEFT JOIN return_status r ON r.issued_id = i.issued_id
WHERE r.return_id IS NULL
```





