    # PostgreSQL Window Functions - Running Total and Moving Average

## Objective

This exercise demonstrates:

- Creating and populating a table
- Understanding Window Functions
- Using `SUM() OVER()` for Running Totals
- Using `AVG() OVER()` for Moving Averages
- Partitioning and Ordering Data

---

# Table Structure

```sql
CREATE TABLE scores (
    student_id  INT PRIMARY KEY,
    name        VARCHAR(100),
    subject     VARCHAR(50),
    score       INT,
    exam_date   DATE,
    grade       VARCHAR(5)
);
```

---

# Running Total

```sql
SELECT
    student_id,
    name,
    subject,
    score,
    SUM(score) OVER(
        PARTITION BY name
        ORDER BY exam_date
    ) AS running_total,
    exam_date,
    grade
FROM scores;
```

## Theory

A running total is a cumulative sum that grows as more rows are processed.

### Execution

For student Amit:

| Exam | Score | Running Total |
|--------|--------|--------|
| Maths | 92 | 92 |
| Science | 88 | 180 |
| English | 79 | 259 |

### Components

#### PARTITION BY

```sql
PARTITION BY name
```

Creates separate groups for each student.

#### ORDER BY

```sql
ORDER BY exam_date
```

Determines the sequence in which the cumulative total is calculated.

---

# Moving Average

```sql
SELECT *,
ROUND(
    AVG(score) OVER(
        ORDER BY exam_date
        ROWS BETWEEN 1 PRECEDING AND CURRENT ROW
    ),2
) AS moving_avg
FROM scores;
```

## Theory

A moving average calculates the average over a sliding window of rows.

### Window Definition

```sql
ROWS BETWEEN 1 PRECEDING AND CURRENT ROW
```

Means:

- Current row
- One row before current row

Example:

| Score |
|---------|
| 92 |
| 85 |

Moving Average:

(92 + 85) / 2 = 88.5

---

# Window Function Syntax

```sql
function_name() OVER(
    PARTITION BY column
    ORDER BY column
)
```

### Common Window Functions

| Function | Purpose |
|-----------|----------|
| SUM() | Running Total |
| AVG() | Moving Average |
| COUNT() | Running Count |
| ROW_NUMBER() | Unique Ranking |
| RANK() | Ranking with gaps |
| DENSE_RANK() | Ranking without gaps |
| LAG() | Previous row value |
| LEAD() | Next row value |

---

# Difference Between Aggregate and Window Functions

| Aggregate Function | Window Function |
|-------------------|-----------------|
| Returns one row per group | Returns every row |
| Uses GROUP BY | Uses OVER() |
| Collapses rows | Preserves rows |

Aggregate Example:

```sql
SELECT name, AVG(score)
FROM scores
GROUP BY name;
```

Window Example:

```sql
SELECT *,
AVG(score) OVER(PARTITION BY name)
FROM scores;
```

---

# Interview Questions

### What is a Window Function?

A function that performs calculations across a set of rows related to the current row without collapsing the result set.

### What is the purpose of OVER()?

It defines the window of rows on which the function operates.

### Difference between RANK() and DENSE_RANK()?

- RANK() leaves gaps after ties.
- DENSE_RANK() does not leave gaps.

### What is a Running Total?

A cumulative sum calculated over ordered rows.

### What is a Moving Average?

An average calculated over a sliding window of rows.

---


