# Notes

-- ORDER cannot be used as an unquoted column name because it is a SQL keyword.
-- Example:
-- CREATE TABLE test (order INT);   -- Error
-- CREATE TABLE test ("order" INT); -- Works

-- BY can be used as a column name because it is not a reserved keyword.
-- Example:
-- CREATE TABLE test (by INT);

-- USER is a special function/keyword in PostgreSQL.
-- It is better to avoid using USER as a table name unless quoted.
-- Example:
-- CREATE TABLE "user" (id INT);

--------------------------------------------------

CREATE TABLE sales (
    sale_id    INT PRIMARY KEY,
    rep_name   VARCHAR(100),
    department VARCHAR(50),
    city       VARCHAR(50),
    amount     DECIMAL(10,2),
    month      VARCHAR(20),
    status     VARCHAR(20)
);

INSERT INTO sales VALUES
(1,'Amit Sharma','Electronics','Mumbai',85000,'January','closed'),
(2,'Sara Khan','Clothing','Delhi',32000,'January','open'),
(3,'Ravi Verma','Electronics','Bangalore',91000,'February','closed'),
(4,'Neha Gupta','HR','Delhi',48000,'February','open'),
(5,'Karan Mehta','Clothing','Pune',73000,'March','closed'),
(6,'Priya Singh','Electronics','Delhi',55000,'March','closed'),
(7,'Dev Sharma','HR','Mumbai',51000,'January','open'),
(8,'Meera Nair','Clothing','Mumbai',68000,'February','closed'),
(9,'Arjun Rao','Electronics','Pune',42000,'March','open'),
(10,'Sana Ali','Clothing','Bangalore',29000,'January','closed');

--------------------------------------------------
-- Recreate table with sample data
--------------------------------------------------

DROP TABLE sales;

CREATE TABLE sales (
    sale_id    INT PRIMARY KEY,
    rep_name   VARCHAR(100),
    department VARCHAR(50),
    city       VARCHAR(50),
    amount     DECIMAL(10,2),
    month      VARCHAR(20),
    status     VARCHAR(20)
);

INSERT INTO sales VALUES
(1,'unnati','IT','kanpur',30000.50,'jan','active'),
(2,'priya','ITI','nagpur',340000.07,'feb','active'),
(3,'riya','SALES','durgapur',530000.04,'dec','not-active'),
(4,'diya','FINANCE','kanakpur',306000.50,'march','not-active');

SELECT * FROM sales;

--------------------------------------------------
-- Transaction Example
--------------------------------------------------

BEGIN;

DELETE FROM sales
WHERE amount > (
    SELECT SUM(amount) * 0.20
    FROM sales
);

SELECT * FROM sales;

ROLLBACK;

-- COMMIT;  -- Use COMMIT only if you want to save changes

--------------------------------------------------
-- Scalar Subquery Example
--------------------------------------------------

SELECT *
FROM sales
WHERE amount >
(
    SELECT AVG(amount)
    FROM sales
);

-- The subquery returns a single value (average amount),
-- so this is called a Scalar Subquery.

