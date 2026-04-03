---
---

# 📘 LESSON 7: Caching (Redis, CDN, Strategies)

---

# 🧠 1. What is Caching?

👉 **Caching = storing frequently used data in a faster place**

---

## 🧩 Simple Analogy (Never Forget)

Think of studying 📚:

- Book in library → slow access
- Notes on your desk → fast access

👉 Notes = **cache**

---

# 💥 2. Why Caching is Needed?

Without cache:

```text
User → Server → Database → Server → User
```

👉 Database is SLOW

---

With cache:

```text
User → Server → Cache → User
```

👉 Much faster ⚡

---

## 🧠 Key Idea

👉 Cache reduces **latency** and **database load**

---

# ⚡ 3. Real Example

---

### Instagram

When you open feed:

- Posts are NOT fetched from DB every time
- They are served from cache

👉 That’s why it feels instant

---

# 🧱 4. Where Cache Lives?

---

## 📍 Types of Cache Locations

---

### 🔷 1. Client-side Cache

- Browser stores data
- Fastest

---

### 🔷 2. Server-side Cache

- Stored in memory (RAM)
- Example: Redis

---

### 🔷 3. CDN (Content Delivery Network)

---

## 🌍 What is CDN?

👉 Stores data near users globally

---

### 💥 Real Example

### YouTube

- Videos stored worldwide
- User gets from nearest server

👉 Faster streaming

---

# 🚀 5. Cache Hit vs Cache Miss

---

## ✅ Cache Hit

👉 Data found in cache
→ Fast response ⚡

---

## ❌ Cache Miss

👉 Data NOT in cache
→ Go to database (slow)

---

## 🧠 Goal

👉 Maximize **cache hit rate**

---

# 🔁 6. Cache Strategies (VERY IMPORTANT)

---

## 🔷 1. Cache-Aside (Lazy Loading) ⭐ MOST COMMON

---

### Flow:

1. Check cache
2. If miss → fetch from DB
3. Store in cache

---

### 💻 Example:

```text
if data in cache:
    return cache
else:
    data = DB
    store in cache
    return data
```

---

### ✅ Pros

- Simple
- Efficient

---

### ❌ Cons

- First request is slow

---

---

## 🔷 2. Write-Through Cache

---

### Flow:

- Write to cache AND database together

---

### ✅ Pros

- Cache always fresh

---

### ❌ Cons

- Slower writes

---

---

## 🔷 3. Write-Back Cache

---

### Flow:

- Write only to cache
- DB updated later

---

### ✅ Pros

- Very fast

---

### ❌ Cons

- Risk of data loss

---

---

## 🔷 4. Read-Through Cache

---

👉 Cache itself fetches from DB

---

---

# ⚠️ 7. Cache Eviction (IMPORTANT)

Cache cannot store everything

👉 So we remove old data

---

## 📊 Common Policies

---

### 🔥 LRU (Least Recently Used)

👉 Remove least used data

---

### 🔥 LFU (Least Frequently Used)

👉 Remove least accessed data

---

---

# 💥 8. Real World Thinking

---

## 🛒 Amazon

- Product pages cached
- Search results cached

👉 Reduces DB load massively

---

# ⚠️ 9. Cache Problems (INTERVIEW TRAPS)

---

## ❗ 1. Stale Data

Cache shows old data

---

## ❗ 2. Cache Invalidation

👉 Hardest problem in system design

---

### 🧠 Famous Joke

👉 “There are only 2 hard things:

- Cache invalidation
- Naming things 😄”

---

---

## ❗ 3. Cache Stampede

Many requests hit DB when cache expires

---

### 🧠 Solution:

- Add random expiration
- Use locking

---

# 🎯 10. Interview Questions (VERY IMPORTANT)

---

### ❓ Q1: What is caching?

✅ Answer:

Caching is storing frequently accessed data in a faster storage layer to reduce latency and improve performance.

---

### ❓ Q2: What is cache hit?

✅ Answer:

A cache hit occurs when requested data is found in cache.

---

### ❓ Q3: What is cache-aside?

✅ Answer:

A strategy where application checks cache first, then database if needed.

---

### ❓ Q4: Why is caching important?

✅ Answer:

It reduces database load, improves latency, and increases system performance.

---

### ❓ Q5: What is cache invalidation?

✅ Answer:

The process of updating or removing stale data from cache.

---

# 🧠 FINAL INTUITION (Never Forget)

👉 Database = slow but correct
👉 Cache = fast but temporary

---

👉 Systems use:

✔ Cache for speed
✔ Database for truth

---

# 🧪 Mini Exercise

Think:

👉 If Instagram didn’t use cache:

What happens?

✔ Slow feed
✔ DB overload
✔ System crash

---

# 📚 Book References

- _Designing Data-Intensive Applications_ — Martin Kleppmann
- _System Design Interview_ — Alex Xu

---
