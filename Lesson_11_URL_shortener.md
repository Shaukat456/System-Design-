---
---

# 📘 LESSON 11: URL Shortener System (Bit.ly Design)

---

# 🧠 1. Problem Statement

We want to design a system like:

👉 Bitly

It does this:

```text
https://verylongwebsite.com/article/how-to-learn-system-design
→
https://bit.ly/abc123
```

---

# 🎯 2. Functional Requirements

---

## User should be able to:

1. Shorten a long URL
2. Open short URL → redirect to original
3. Track clicks (optional)

---

# ⚡ 3. Non-Functional Requirements

---

We design for:

- High availability (99.99%)
- Very fast redirects (< 100ms)
- Scalable (millions of URLs)
- Low latency

---

# 🧩 4. Core Idea

---

We need:

| Component     | Purpose            |
| ------------- | ------------------ |
| API           | create + redirect  |
| Database      | store mappings     |
| Cache         | speed up redirects |
| Load Balancer | distribute traffic |

---

# 🏗️ 5. High-Level Architecture

---

```text id="urlsys1"
User → Load Balancer → Application Server → Cache → Database
```

---

# 🔁 6. Two Main APIs

---

## 🔷 1. Create Short URL

```http
POST /shorten
```

Input:

```json
{
  "long_url": "https://example.com/very-long-url"
}
```

Output:

```json
{
  "short_url": "bit.ly/abc123"
}
```

---

## 🔷 2. Redirect

```http
GET /abc123
```

👉 Returns:

- 301 redirect → original URL

---

# 🧠 7. Database Design

---

We need a simple mapping:

```text
short_code | long_url
-----------|------------------
abc123     | https://example.com
```

---

## Best Choice:

👉 NoSQL or Key-Value store

Example:

- Redis (cache)
- DynamoDB / MongoDB (persistent)

---

# ⚡ 8. How Short URL is Generated

---

## Method 1: Hashing

```text id="hash1"
long_url → MD5 → abc12345 → take first 6 chars
```

---

## Method 2: Counter (BEST in interviews)

```text id="counter1"
1 → abc
2 → abd
3 → abe
```

---

👉 Uses Base62 encoding:

- A-Z
- a-z
- 0-9

---

# 🚀 9. Redirect Flow (IMPORTANT)

---

```text id="flow1"
User → bit.ly/abc123
      → Cache check
      → If miss → DB lookup
      → Return long URL
```

---

# ⚡ 10. Where Cache is Used

---

👉 Very important optimization:

- Most URLs are frequently accessed
- Cache stores mapping

---

## Result:

✔ Fast redirects
✔ Less DB load

---

# 🌍 11. Scaling the System

---

## Problem:

Millions of URLs per day

---

## Solution:

### 🔷 Horizontal scaling

- Multiple app servers
- Load balancer distributes traffic

---

### 🔷 Database scaling

- Sharding by short code
- Distributed NoSQL

---

### 🔷 Cache scaling

- Redis cluster

---

# 📊 12. Analytics (Optional Feature)

---

We can track:

- Click count
- Location
- Device type

---

## Use:

👉 Message Queue (Kafka)

```text id="analytics1"
Redirect → Queue → Analytics service
```

---

# ⚠️ 13. Edge Cases

---

## ❗ Collision problem

Two URLs generate same short code

👉 Solution:

- retry generation
- or use counter system

---

## ❗ Expiration

Some URLs expire after time

---

# 🎯 14. Final Architecture

---

```text id="finalarch"
User
 ↓
Load Balancer
 ↓
App Server
 ↓
Cache (Redis)
 ↓
Database (NoSQL)
 ↓
Analytics Queue (Kafka)
```

---

# 🧠 15. Key Insights (VERY IMPORTANT)

---

## 🔥 Why this system works:

- Cache → speed
- DB → persistence
- Queue → analytics
- LB → scaling

---

# 🎯 16. Interview Questions

---

### ❓ Q1: How does URL shortener work?

✅ Answer:

It maps long URLs to short unique keys stored in a database and redirects using that mapping.

---

### ❓ Q2: What DB would you use?

✅ Answer:

Key-value NoSQL database for fast lookup and scalability.

---

### ❓ Q3: How do you generate short URLs?

✅ Answer:

Using Base62 encoding of a unique ID or hashing techniques.

---

### ❓ Q4: How do you handle scale?

✅ Answer:

Using caching, horizontal scaling, load balancers, and distributed databases.

---

### ❓ Q5: Why use cache here?

✅ Answer:

To reduce database load and improve redirect speed.

---

# 🧠 FINAL INTUITION (Never Forget)

👉 URL shortener =

✔ Simple system
✔ But uses ALL system design concepts:

- Scaling
- DB
- Cache
- Load balancing
- Queues

---

# 📚 Book References

- _System Design Interview_ — Alex Xu
- _Designing Data-Intensive Applications_ — Martin Kleppmann

---
