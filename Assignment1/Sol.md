# Problem Statement

 A nightly billing job needs to update 1 million records. It crashed midway last week and the entire job had to restart from scratch.
  Write a SQL script that commits 1 million transactions efficiently. Your script should handle failures gracefully so the entire job never needs to restart from zero. Explain every decision you made and why.

---

# Using Transaction and Commit

Using a single transaction like this:

```sql
START TRANSACTION;

-- Update 1 million records

COMMIT;
```

It's gonna lose all the records in case of any failure in the process.

---

# Optimized Solution

## 1. Checkpoint

Checkpointing stores the progress of the job so the system knows where to resume after a failure.

Instead of restarting from record `1`, the process resumes from the last successfully processed record.


## Checkpoint Table

```sql
CREATE TABLE checkpoint (
    job_id INTEGER PRIMARY KEY,
    last_processed_id BIGINT,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# Reading the Last Checkpoint

```sql
SELECT COALESCE(last_processed_id, 0)
FROM checkpoint
WHERE job_id = 1;
```

## Coalesce

If no checkpoint exists yet, `NULL` would be returned.

`COALESCE(..., 0)` ensures:
- Processing starts from record `0`
- Prevents null-related issues

---

# Batch Processing

Processing in batches could be encouraged too. Through that the batch will be processed and in case of failures. Other batches would be safe irrespective of everything else.


