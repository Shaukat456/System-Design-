---
---

# 📘 CASE STUDY 3: DESIGN INSTAGRAM-LIKE SOCIAL MEDIA FEED

We are designing something like:

👉 Instagram

This system is MUCH more complex than WhatsApp because it includes:

- feeds
- ranking
- massive fan-out traffic
- caching at scale
- recommendation systems (basic level)

---

# 🧠 1. PROBLEM STATEMENT

---

We want to build a system where users can:

## 📌 Functional Requirements:

- Post images/videos
- Follow/unfollow users
- See a home feed (timeline)
- Like/comment posts
- View profiles

---

## ⚙️ Non-Functional Requirements:

- Very fast feed loading (< 200ms)
- Highly scalable (billions of posts)
- Highly available
- Personalized feed
- Handles viral content (celebrity posts)

---

# 🧠 2. CORE PROBLEM (MOST IMPORTANT)

---

When user opens app:

👉 We must generate a **feed instantly**

---

Two approaches:

# ⚡ 3. TWO MAIN FEED STRATEGIES

---

# 🟢 1. PULL MODEL (ON-DEMAND)

---

When user opens feed:

👉 fetch posts from all followed users

---

```text id="f1"
User → Fetch posts from 1000 followings → Merge → Sort → Show feed
```

---

## ❌ Problem:

- Too slow
- Too many database calls

---

# 🔵 2. PUSH MODEL (FAN-OUT ON WRITE) ⭐ (USED IN REAL SYSTEMS)

---

When user posts something:

👉 system pushes post to followers’ feeds

---

```text id="f2"
User posts → system pushes to all followers’ feed cache
```

---

✔ Fast feed loading
❌ Expensive write operation

---

# 🧠 REAL SYSTEMS USE HYBRID

---

👉 celebrities = pull model
👉 normal users = push model

---

# ⚙️ 4. HIGH LEVEL ARCHITECTURE

---

```text id="arch1"
User App
   |
Load Balancer
   |
Feed Service
   |
--------------------------
|        |              |
Post DB  Feed Cache   User Graph DB
```

---

# 🧠 5. KEY COMPONENTS

---

## 1️⃣ User Graph (FOLLOW SYSTEM)

Stores:

```text id="g1"
User A → follows → User B, User C
```

We use:

👉 Graph Database / adjacency list

---

## 2️⃣ Post Service

Stores posts:

```sql id="p1"
POSTS
-----
post_id
user_id
content
timestamp
```

---

## 3️⃣ Feed Cache

Stores precomputed feeds:

```text id="c1"
User A Feed:
- Post X
- Post Y
- Post Z
```

---

# 🔄 6. HOW FEED IS GENERATED

---

## STEP 1: User posts content

```text id="flow1"
User A uploads post → Post Service stores it
```

---

## STEP 2: Fan-out process begins

```text id="flow2"
System finds all followers of User A
```

---

## STEP 3: Push into feeds

```text id="flow3"
For each follower:
   add post into their feed cache
```

---

# ⚠️ 7. SCALING PROBLEM (VERY IMPORTANT)

---

If user has:

👉 10 million followers

Then:

❌ fan-out becomes impossible in real time

---

# 🔥 SOLUTION: HYBRID MODEL

---

## 🟢 Normal users:

✔ push model (fan-out on write)

---

## 🔵 Celebrities:

✔ pull model (fan-out on read)

---

```text id="hyb1"
If followers < threshold → PUSH
Else → PULL
```

---

# ⚡ 8. CACHING STRATEGY

---

We cache:

- home feed
- recent posts
- trending posts

Using:

👉 Redis-style cache

---

# 🧠 9. RANKING SYSTEM (VERY IMPORTANT)

---

Feed is NOT just chronological.

We rank posts based on:

- likes
- comments
- recency
- user interaction

---

## Simple formula:

```text id="rank1"
Score = (Likes × 2) + Comments + Recency Boost
```

---

Real systems use ML models (future topic)

---

# 📊 10. DATABASE DESIGN

---

## USERS

```sql id="u1"
user_id
name
```

---

## POSTS

```sql id="u2"
post_id
user_id
content
timestamp
```

---

## FOLLOWERS

```sql id="u3"
follower_id
followee_id
```

---

# ⚡ 11. PERFORMANCE OPTIMIZATIONS

---

## 1. Precompute feeds

✔ reduces read time

---

## 2. Cache hot feeds

✔ fastest access

---

## 3. Pagination

✔ loads feed in chunks

---

# ⚖️ 12. TRADEOFFS

---

## PUSH MODEL:

✔ fast read
❌ slow write

---

## PULL MODEL:

✔ simple write
❌ slow read

---

## HYBRID:

✔ balanced (real-world standard)

---

# 🧠 13. CONCEPTS USED

---

| Concept        | Usage                |
| -------------- | -------------------- |
| Fan-out        | feed generation      |
| Graph DB       | followers system     |
| Cache          | feed storage         |
| Load balancing | traffic distribution |
| Sharding       | scaling posts        |
| Ranking system | feed ordering        |

---

# 🔥 14. INTERVIEW QUESTIONS

---

### ❓ Q1: How do you generate a feed?

✔ Answer:
Using fan-out strategy (push or hybrid model).

---

### ❓ Q2: How do you handle celebrities?

✔ Answer:
Use pull model instead of pushing to millions of followers.

---

### ❓ Q3: How do you rank posts?

✔ Answer:
Based on engagement signals like likes, comments, and recency.

---

### ❓ Q4: How do you scale feed system?

✔ Answer:

- caching
- sharding
- hybrid fan-out
- async processing

---

### ❓ Q5: Why not compute feed on request?

✔ Answer:
Too slow due to many database reads.

---

# 🧠 FINAL SUMMARY

---

Instagram-like system =

👉 A **high-scale feed generation + ranking + caching system**

Core ideas:

- Fan-out (push vs pull)
- Feed caching
- Graph relationships
- Ranking logic
- Hybrid architecture

---

/
