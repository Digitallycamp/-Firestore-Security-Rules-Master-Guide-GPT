# 🔐 Firestore Security Rules Master Guide

A comprehensive guide to understanding, writing, and scaling Cloud Firestore Security Rules for production-grade applications.

---

# 📚 Table of Contents

1. Introduction to Firestore Security Rules
2. Rule Structure & Syntax
3. Request & Resource Objects
4. Basic Read/Write Rules
5. Authentication-Based Access Control
6. Role-Based Authorization
7. Data Validation Rules
8. Field-Level Security
9. Query-Based Rules
10. Security for Nested Collections
11. Custom Functions
12. Time-Based Rules
13. Common Patterns (Best Practices)
14. Debugging & Testing Rules
15. Production Hardening Checklist

---

# 1. 🧠 Introduction to Firestore Security Rules

Firestore Security Rules control:
- Who can read/write data
- What data they can access
- How data should be structured

---

# 2. 🧱 Rule Structure & Syntax

```js
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {

    match /products/{productId} {
      allow read, write: if true;
    }

  }
}
```

Explanation:
- rules_version → Enables latest syntax
- service → Defines Firestore service
- match → Path targeting
- allow → Defines access condition

---

# 3. 🔍 Request & Resource Objects

```js
allow create: if request.auth != null;
```

- request.auth → Auth info
- resource → Existing data
- request.resource → Incoming data

---

# 4. 🔐 Basic Rules

```js
allow read: if true;
allow write: if false;
```

---

# 5. 👤 Authentication

```js
allow create: if request.auth != null;
```

---

# 6. 🧑‍💼 Role-Based

```js
allow write: if get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == "admin";
```

---

# 7. ✅ Validation

```js
allow create: if request.resource.data.price > 0;
```

---

# 8. 🔒 Field Security

```js
allow update: if request.resource.data.keys().hasOnly(['name', 'price']);
```

---

# 9. 🔎 Query Rules

```js
allow list: if request.auth != null;
```

---

# 10. 📂 Nested Collections

```js
match /products/{productId}/reviews/{reviewId} {
  allow create: if request.auth != null;
}
```

---

# 11. 🧩 Functions

```js
function isOwner(userId) {
  return request.auth.uid == userId;
}
```

---

# 12. ⏱️ Time Rules

```js
allow create: if request.time < timestamp.date(2026, 12, 31);
```

---

# 13. 🧠 Best Practices

- Always validate data
- Never allow public writes
- Use roles

---

# 14. 🐞 Debugging

Use Firebase Emulator

---

# 15. 🚀 Checklist

- No open rules
- Auth enforced
- Validation added
