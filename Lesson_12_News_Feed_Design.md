---
---

# 📘 LESSON 12: Instagram / Social Media Feed System Design

We will design a system like:

👉 Instagram

---

# 🧠 1. Problem Understanding

---

## Core idea:

When you open Instagram:

You see a **feed of posts from people you follow**

Example:

```text id="feed1"
Alice posted a photo
Bob posted a reel
Charlie posted a meme
```

---

# 🎯 2. Functional Requirements

---

We must support:

### 1. Users

- Sign up / login

### 2. Posts

- Upload image/video
- Like, comment

### 3. Follow system

- Follow / unfollow users

### 4. Feed

- Show posts from followed users

---

# ⚡ 3. Non-Functional Requirements

---

We design for:

- Millions of users
- Very fast feed loading (< 200 ms)
- High availability
- Real-time updates (optional)
- Infinite scroll

---

# 🧩 4. Core Challenge (VERY IMPORTANT)

---

👉 The hardest part is:

# “How do we generate the feed?”

Two approaches:

---

# 🧠 5. Approach 1: Pull Model (Simple)

---

## Idea:

When user opens feed:

👉 Fetch posts from all followed users in real-time

---

## Flow:

```text id="pull1"
User → Request Feed
     → Fetch all followings
     → Query posts from DB
     → Merge + sort by time
     → Return feed
```

---

## ❌ Problem:

If user follows 1000 people:

👉 TOO SLOW
👉 TOO MANY DB QUERIES

---

# 🚀 6. Approach 2: Push Model (Used in Real Systems)

---

## Idea:

When someone posts:

👉 Precompute feeds of followers

---

## Flow:

```text id="push1"
Alice posts photo
→ system pushes post to all followers' feeds
→ stored in feed cache
```

---

## ✔ Result:

- Fast feed loading
- Precomputed timelines

---

## ❌ Problem:

If user has 10M followers:

👉 too expensive to push

---

# ⚖️ 7. Hybrid Approach (REAL INDUSTRY SOLUTION)

---

Used by real systems like:

👉 Meta

---

## Idea:

| User type    | Strategy   |
| ------------ | ---------- |
| Normal users | Push model |
| Celebrities  | Pull model |

---

# 🏗️ 8. High-Level Architecture

---

```text id="arch1"
User
 ↓
API Gateway
 ↓
Feed Service
 ↓
Cache (Redis)
 ↓
Database (Posts + User graph)
```

---

# 🧾 9. Database Design

---

## Users Table

```text id="db1"
user_id | name
```

---

## Posts Table

```text id="db2"
post_id | user_id | content | timestamp
```

---

## Followers Table

```text id="db3"
user_id | follower_id
```

---

# ⚡ 10. Feed Storage (Important)

---

We store:

```text id="feedcache"
user_id → [post_id1, post_id2, post_id3]
```

👉 This is precomputed feed

---

# 🔁 11. Feed Generation Flow

---

## When user opens app:

```text id="flow2"
User → Feed Service
     → Redis cache check
     → If miss → DB fetch
     → Return sorted posts
```

---

# 📦 12. Post Upload Flow

---

```text id="postflow"
User uploads post
→ Store in DB
→ Push to followers' feed cache
→ Update timeline
```

---

# ⚡ 13. Scaling the System

---

## Problem:

- Millions of posts per second
- Billions of feed requests

---

## Solutions:

### 🔷 1. Cache (Redis)

- Store feed timelines

---

### 🔷 2. Sharding DB

- Split users across servers

---

### 🔷 3. Kafka Queue

```text id="queue1"
Post → Kafka → Feed Generator workers
```

---

### 🔷 4. CDN for images/videos

- Fast media delivery globally

---

# 🧠 14. Ranking Feed (VERY IMPORTANT)

---

We don’t just show latest posts.

We rank based on:

- Likes
- Comments
- Engagement
- Relationship strength

---

## Formula idea:

```text id="rank1"
score = likes + comments + recency + relationship_weight
```

---

# ⚠️ 15. Edge Cases

---

### ❗ Celebrity problem

1 user → 100M followers

👉 cannot push feed

✔ solution:

- Pull model only

---

### ❗ Cold start problem

New post has no engagement

✔ solution:

- time-based ranking boost

---

# 🎯 16. Final Architecture

---

```text id="final"
User
 ↓
API Gateway
 ↓
Feed Service
 ↓
Cache (Redis)
 ↓
DB + Kafka
 ↓
Ranking Service
 ↓
CDN (media)
```

---

# 🧠 17. Key Takeaways (EXAM + INTERVIEW)

---

## 🔥 Must remember:

### 1. Two models:

- Pull = real-time compute
- Push = precompute feeds

---

### 2. Real systems use HYBRID

---

### 3. Cache is EVERYTHING in feed systems

---

### 4. Ranking is separate service

---

# 🎯 18. Interview Questions

---

### ❓ Q1: How does Instagram feed work?

✔ Answer:
Hybrid push-pull system with caching and precomputed timelines.

---

### ❓ Q2: Why not pure push model?

✔ Answer:
Too expensive for users with millions of followers.

---

### ❓ Q3: How do you rank posts?

✔ Answer:
Based on engagement, recency, and user relationships.

---

### ❓ Q4: What happens when cache fails?

✔ Answer:
Fallback to database query and regenerate feed.

---

# 📚 References (Industry Standard)

- _Designing Data-Intensive Applications_ — Martin Kleppmann
- _System Design Interview_ — Alex Xu
- Facebook News Feed Engineering Blog

---
