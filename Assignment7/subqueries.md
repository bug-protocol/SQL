# PostgreSQL Subqueries and Transactions - Practice README

## Objective

This exercise demonstrates:

- Creating and populating tables
- Using transactions (`BEGIN`, `COMMIT`, `ROLLBACK`)
- Using scalar subqueries
- Performing conditional deletes with subqueries
- Understanding aggregate functions inside subqueries

---

## Table Structure

```sql
CREATE TABLE sales (
    sale_id    INT PRIMARY KEY,
    rep_name   VARCHAR(100),
    department VARCHAR(50),
    city       VARCHAR(50),
    amount     DECIMAL(10,2),
    month      VARCHAR(20),
    status     VARCHAR(20)
);
```

## Transactions

A transaction is a sequence of SQL operations executed as a single unit of work.

```sql
BEGIN;

DELETE FROM sales
WHERE amount > (
    SELECT SUM(amount) * 0.20
    FROM sales
);

ROLLBACK;
```

### COMMIT vs ROLLBACK

- **COMMIT** permanently saves changes.
- **ROLLBACK** discards uncommitted changes.

---

## Scalar Subquery

A scalar subquery returns exactly one value.

```sql
SELECT *
FROM sales
WHERE amount >
(
    SELECT AVG(amount)
    FROM sales
);
```

### Execution Flow

1. Execute subquery.
2. Calculate average amount.
3. Execute outer query.
4. Compare each row against the average.
5. Return matching rows.

---

## Correlated Subquery

General form:

```sql
SELECT *
FROM table1 t1
WHERE condition >
(
    SELECT ...
    FROM table2 t2
    WHERE t2.col = t1.col
);
```

The inner query references the outer query and executes once for each row.

---

## Scalar vs Correlated Subquery

| Scalar Subquery | Correlated Subquery |
|----------------|---------------------|
| Executes once | Executes per row |
| Independent | Depends on outer query |
| Returns one value | Can return different values |
| Usually faster | Usually more expensive |

---

## Interview Questions

### What is a scalar subquery?
A subquery that returns a single value and can be used anywhere a single value is expected.

### What is a correlated subquery?
A subquery that references columns from the outer query and executes once for every row processed by the outer query.

### Why use transactions?
Transactions ensure consistency and allow changes to be committed or rolled back as a single unit.
