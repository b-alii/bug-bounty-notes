# 🧠 Bug Hunting Methodology & Testing Checklist

This document outlines my structured approach to testing web applications for vulnerabilities.

---

## 🔍 1. Reconnaissance

* Identify target domains (in-scope only)
* Explore application features
* Map functionality (login, cart, search, etc.)
* Identify input points (parameters, forms, APIs)

---

## 🧪 2. Input Testing

Test all user-controlled inputs for:

* SQL Injection
* Cross-Site Scripting (XSS)
* Path Traversal

Look for:

* Errors
* Unexpected behavior
* Reflected input

---

## 🔐 3. Authentication Testing

* Login bypass attempts
* Password reset flaws
* Weak credential handling
* Session/token behavior

Questions:

* Can I access another account?
* Can I bypass authentication?

---

## 🛂 4. Authorization Testing

* Access control checks
* Horizontal privilege escalation (access another user’s data)
* Vertical privilege escalation (user → admin)

---

## 🛒 5. Business Logic Testing

* Price manipulation
* Quantity tampering
* Workflow bypass (skipping steps)

Questions:

* Can I abuse how the app works?
* Can I get something for free?

---

## 📡 6. Request Analysis (Burp Suite)

* Intercept and analyze requests

* Identify parameters:

  * IDs
  * Tokens
  * Prices
  * User inputs

* Modify values and observe responses

---

## ⚠️ 7. Safe Testing Rules

* Test only in-scope assets
* Avoid automated aggressive scanning
* Do not disrupt services
* Follow program rules

---

## 🧠 Key Mindset

* Do not spam payloads
* Focus on understanding the feature
* Break assumptions made by developers

---

## 🎯 Goal

To systematically identify vulnerabilities by combining:

* Technical testing
* Logical thinking
* Real-world application understanding
