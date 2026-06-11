# PostgreSQL JSONB and JSON Processing

## Objective

This exercise demonstrates:

- Creating JSONB columns
- Inserting JSON data
- Querying JSON fields
- Difference between `->` and `->>`
- Filtering JSON data
- JSON containment operators
- Checking key existence
- Converting JSON arrays into rows
- Loading JSON data into relational tables

---

# Create JSONB Table

```sql
CREATE TABLE api_data (
    id SERIAL PRIMARY KEY,
    payload JSONB
);
```

## Theory

`JSONB` stores JSON in a binary format.

Advantages:

- Faster querying
- Supports indexing
- Removes duplicate keys
- Optimized storage

---

# Insert JSON Data

```sql
INSERT INTO api_data(payload)
VALUES ('{
    "sale_id": 11,
    "rep_name": "Rohan Bajaj",
    "department": "Engineering"
}');
```

JSON is stored inside the `payload` column.

---

# JSON Operators

## Using ->

```sql
SELECT payload->'rep_name'
FROM api_data;
```

Returns:

```json
"Rohan Bajaj"
```

Result type: JSON

---

## Using ->>

```sql
SELECT payload->>'rep_name'
FROM api_data;
```

Returns:

```text
Rohan Bajaj
```

Result type: TEXT

---

# Difference Between -> and ->>

| Operator | Returns |
|-----------|----------|
| -> | JSON Object/Value |
| ->> | Text Value |

Example:

```sql
payload->'amount'
```

returns:

```json
78000
```

while

```sql
payload->>'amount'
```

returns:

```text
78000
```

---

# Filtering JSON Values

Incorrect:

```sql
SELECT *
FROM api_data
WHERE payload->'amount' > 50000;
```

Why?

`payload->'amount'` returns JSON, not a numeric value.

---

Correct:

```sql
SELECT *
FROM api_data
WHERE (payload->>'amount')::INT > 50000;
```

## Theory

### Step 1

Extract text:

```sql
payload->>'amount'
```

### Step 2

Cast to integer:

```sql
(payload->>'amount')::INT
```

### Step 3

Perform comparison.

---

# JSON Containment Operator (@>)

```sql
SELECT payload->>'rep_name'
FROM api_data
WHERE payload @> '{"status":"open"}';
```

## Theory

`@>` means:

"Does the left JSON contain the right JSON?"

Example:

```json
{
  "status":"open",
  "amount":78000
}
```

contains:

```json
{
  "status":"open"
}
```

Result: TRUE

---

# Check Whether a Key Exists

```sql
SELECT *
FROM api_data
WHERE payload ? 'department';
```

## Theory

`?` checks whether a key exists.

Example:

```json
{
  "department":"Engineering"
}
```

contains the key:

```text
department
```

Result: TRUE

---

# json_to_recordset()

Convert a JSON array into rows.

```sql
SELECT *
FROM json_to_recordset('[
 {"sale_id":13,"rep_name":"Suresh Nair"},
 {"sale_id":14,"rep_name":"Divya Shah"}
]')
AS t(
    sale_id INT,
    rep_name TEXT
);
```

## Output

| sale_id | rep_name |
|----------|-----------|
| 13 | Suresh Nair |
| 14 | Divya Shah |

---

# Why Use json_to_recordset()?

Without it:

- JSON remains nested.
- Hard to query row-by-row.

With it:

- JSON becomes relational rows.
- Can join, filter, aggregate, and insert normally.

---

# Insert JSON Array Directly Into Table

```sql
INSERT INTO sales (
    sale_id,
    rep_name,
    department,
    city,
    amount,
    status
)
SELECT *
FROM json_to_recordset(...);
```

## Benefits

- Load API responses directly.
- No manual parsing.
- Useful in ETL pipelines.

---

# Real World Use Case

API Response

```json
[
  {
    "sale_id": 13,
    "rep_name": "Suresh Nair",
    "amount": 61000
  }
]
```

Workflow:

1. Store raw JSON in JSONB.
2. Validate data.
3. Convert using `json_to_recordset()`.
4. Insert into relational tables.
5. Run analytics.

---

# Common JSONB Operators

| Operator | Description |
|-----------|------------|
| -> | Get JSON value |
| ->> | Get text value |
| @> | Contains |
| ? | Key exists |
| ?| | Any key exists |
| ?& | All keys exist |

---

# Interview Questions

### Why use JSONB instead of JSON?

JSONB is stored in binary format, supports indexing, and provides faster querying.

### Difference between -> and ->>?

- `->` returns JSON.
- `->>` returns TEXT.

### Why did the amount comparison fail?

Because `payload->'amount'` returns JSON, not a numeric datatype.

### What does @> do?

Checks whether one JSON document contains another.

### What is json_to_recordset()?

A PostgreSQL function that converts a JSON array into relational rows.

### Why store raw API data in JSONB?

It preserves the original payload while allowing later transformation into structured tables.

---
