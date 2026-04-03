---


We’ll do:

* One full case study (deep)
* Problem statement → design → tradeoffs → final architecture
* Concepts used (from previous lessons)
* Interview Q/A

---

# 📘 CASE STUDY 1: DESIGN A URL SHORTENER (Like bit.ly)

👉 This is one of the MOST common system design interview problems.

---

# 🧠 1. PROBLEM STATEMENT

---

You are asked to build a system like:

👉 Bitly

---

## Requirements:

### Functional:

- User enters long URL
- System generates short URL
- Redirect short URL → original URL
- Track clicks (analytics optional)

---

### Non-functional:

- Very fast redirects (low latency)
- Highly available (must always work)
- Scalable (billions of URLs)
- Unique short links

---

# 🧠 2. CORE UNDERSTANDING

---

We are building:

```text id="cs1"
Long URL → Short URL → Redirect → Original URL
```

---

Example:

```text
https://amazon.com/product/xyz12345
→ bit.ly/abc123
```

---

# ⚙️ 3. HIGH LEVEL ARCHITECTURE

---

```text id="cs2"
User → Load Balancer → App Server → Database
                          ↓
                     Cache (Redis)
```

---

👉 Concepts used:

- Load Balancer (Lesson 21)
- Cache (Lesson 22)
- Database scaling (Sharding Lesson 19)

---

# 🧠 4. DESIGN CHOICES

---

## 1️⃣ URL GENERATION STRATEGY

We need unique short IDs.

---

### Option A: Random String

```text id="gen1"
abc123, xYz9Kp
```

✔ Simple
❌ Collision risk

---

### Option B: Base62 Encoding (BEST)

We convert ID → short string:

```text id="gen2"
ID: 125 → Base62 → "cb3"
```

Characters:

```
a-z, A-Z, 0-9
```

✔ No collisions
✔ Fast generation

---

### Option C: Hashing (MD5/SHA)

✔ Deterministic
❌ Long collisions possible

---

# 🧠 5. DATABASE DESIGN

---

## Table:

```sql
URL_TABLE
---------
id (primary key)
short_url
long_url
created_at
```

---

## Scaling Problem:

If billions of URLs:

👉 We use **Sharding (Lesson 19)**

- shard by id
- or hash(short_url)

---

# ⚡ 6. REDIRECT FLOW (CRITICAL PATH)

---

```text id="flow1"
User → short URL → Load Balancer → App Server → Cache → DB
```

---

## Step-by-step:

### 1. Check cache

- If exists → return instantly ⚡

### 2. Cache miss

- Query DB
- Store in cache
- Return response

---

👉 Uses:

- Caching (Lesson 22)
- Load balancing (Lesson 21)

---

# ⚡ 7. CACHE STRATEGY

---

We use:

👉 Cache-aside pattern

```text id="cache1"
App → Cache → DB (fallback)
```

---

Why?

✔ Fast redirects
✔ Reduces DB load

---

# 🧠 8. HOW REDIRECTION WORKS

---

Two types:

## 1. 301 Redirect (Permanent)

- Browser caches it

## 2. 302 Redirect (Temporary)

- Always hits server

---

👉 Most systems use 302 for tracking clicks

---

# ⚖️ 9. RATE LIMITING (IMPORTANT ADD-ON)

---

We must protect system:

👉 Prevent abuse of URL creation API

✔ Uses Rate Limiting (Lesson 25)

---

# 🧠 10. ANALYTICS (OPTIONAL FEATURE)

---

We can track:

- clicks per URL
- geo location
- device type

---

## Approach:

Use async system:

👉 Message Queue (future lesson preview)

---

# 🔥 11. FINAL ARCHITECTURE

---

```text id="final1"
            Users
              |
        Load Balancer
              |
       App Server Layer
        /        \
   Cache        Database
    |             |
 Redis        Sharded DB
```

---

# 🧠 12. CONCEPTS USED

---

| Concept       | Where used                  |
| ------------- | --------------------------- |
| Load Balancer | Traffic distribution        |
| Caching       | Fast redirects              |
| Sharding      | DB scaling                  |
| Rate Limiting | API protection              |
| CAP Tradeoff  | Availability vs consistency |
| Hash/Base62   | URL generation              |

---

# ⚖️ 13. TRADEOFFS

---

## 1. Cache vs DB

- Cache = fast but volatile
- DB = slow but permanent

---

## 2. Random vs Base62

- Random = simpler but collision risk
- Base62 = scalable and safe

---

## 3. Consistency vs Availability

- We prefer availability (AP system)

---

# 🔥 14. INTERVIEW QUESTIONS

---

### ❓ Q1: How do you generate short URLs?

✔ Answer:
Using Base62 encoding or unique ID generation.

---

### ❓ Q2: How do you handle collisions?

✔ Answer:
Use unique ID generator or database constraints.

---

### ❓ Q3: How do you make redirects fast?

✔ Answer:
Use caching (Redis) before database lookup.

---

### ❓ Q4: How do you scale the system?

✔ Answer:
Use sharding, caching, and load balancing.

---

### ❓ Q5: Why use 302 redirect?

✔ Answer:
To allow tracking and avoid browser caching.

---

# 🧠 FINAL SUMMARY

---

## URL SHORTENER =

👉 A system that maps long URLs → short URLs and redirects efficiently

---

## CORE IDEAS:

- Unique ID generation (Base62)
- Cache-first reads
- Sharded database
- Load balanced architecture
- High availability design

---

# 🚀 NEXT STEP

Now we move to a MUCH harder real-world system:
