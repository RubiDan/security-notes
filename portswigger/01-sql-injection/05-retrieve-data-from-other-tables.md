# retrieve-data-from-other-tables

- **Source:** [PortSwigger](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables)
- **Level:** Practitioner
- **Vulnerability class:** SQL injection
- **Date:** 2026-11-26

## Goal
The database contains a different table called `users`, with columns called `username` and `password`. To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

## What I tried
First I checked how many columns the query returns, using the technique from [determine-number-of-columns](03-determine-number-of-columns.md). I injected into the `category` parameter:

```
' UNION SELECT NULL,NULL,NULL--
' UNION SELECT NULL,NULL--
```

The three-column payload returned an error, and the two-column one worked, so the query returns two columns.

Then I checked the column types by putting a string in each column instead of a `NULL`. Both columns accept text, so both can hold the usernames and passwords.

## Solution
```
' UNION SELECT username, password FROM users--
```

This returns every username and password from the `users` table. I read the administrator's password from the results and used it to log in as administrator.

(The payload I used in the lab was `' UNION SELECT username, password FROM users WHERE username LIKE 'administrator'--`, which returns only the admin row. That works too, but the version above matches the goal of retrieving all users.)

## Root cause
User input is put directly into the SQL query instead of using a parameterized query.

## Fix
Use parameterized queries.

## What I learned
How to use a UNION attack to pull data from a different table (`users`) than the one the page normally queries, and use it to retrieve credentials and log in.