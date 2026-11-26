# determine-number-of-columns

- **Source:** [PortSwigger](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns)
- **Level:** Practitioner
- **Vulnerability class:** SQL injection
- **Date:** 2026-11-23

## Goal
Determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values.

## What I tried
I injected into the `category` parameter, adding one `NULL` each time:

```
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
```

The first two returned an error, because a UNION only works when both queries return the same number of columns.

## Solution
`' UNION SELECT NULL,NULL,NULL--` worked, so the original query returns three columns.
I used `NULL` because it fits any data type, so the only thing that can make the query fail is the wrong number of columns.

## Root cause
User input is put directly into the SQL query instead of using a parameterized query.

## Fix
Use parameterized queries.

## What I learned
How to find the number of columns a query returns by adding `NULL`s to a UNION until it stops returning an error.