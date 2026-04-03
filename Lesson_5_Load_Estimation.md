---
---

# 📘 LESSON 5: Load & Traffic Estimation

---

# 🧠 1. Why This is IMPORTANT

Before designing ANY system, you must answer:

👉 **“How big is the system?”**

Because:

- Wrong estimate → wrong design ❌
- Overestimate → waste money 💸
- Underestimate → system crashes 💥

---

## 🧠 Interview Reality

If you skip estimation:

👉 Interviewer thinks: _“This person designs blindly”_

---

# 🧩 2. What Do We Estimate?

Every system design starts with:

---

## 📊 Core Things to Estimate

1. **Number of users**
2. **Requests per second (RPS)**
3. **Data storage**
4. **Bandwidth**

---

# 🏠 3. Real-Life Example (Lock This)

Let’s design:

👉 A mini version of **Instagram**

---

# 🔢 STEP-BY-STEP ESTIMATION

---

## 🧑‍🤝‍🧑 Step 1: Number of Users

Assume:

- Total users = **1 million**
- Daily active users (DAU) = **10%**

👉 DAU =

```text
100,000 users/day
```

---

## ⚡ Step 2: Requests Per User

Assume:

- Each user opens app **10 times/day**

👉 Total requests/day:

```text
100,000 × 10 = 1,000,000 requests/day
```

---

## 🚀 Step 3: Requests Per Second (RPS)

👉 Convert to seconds:

```text
1,000,000 / 86,400 ≈ 12 requests/sec
```

---

## 💥 Add Peak Factor (IMPORTANT)

Traffic is not uniform.

👉 Peak = ~10× average

```text
12 × 10 = 120 RPS
```

---

## 🧠 FINAL RPS:

👉 Design for **~120 requests/sec**

---

# 💾 4. Storage Estimation

---

## 📸 Assume:

- Each user uploads 2 photos/day
- Photo size = 2 MB

---

### Total uploads per day:

```text
100,000 × 2 = 200,000 photos
```

---

### Storage per day:

```text
200,000 × 2MB = 400,000 MB = 400 GB/day
```

---

### Storage per year:

```text
400 GB × 365 ≈ 146 TB/year
```

---

## 🧠 Insight

👉 Storage grows FAST in real systems

---

# 🌐 5. Bandwidth Estimation

---

## Assume:

- Each image = 2 MB
- Each photo viewed 10 times

---

### Daily reads:

```text
200,000 × 10 = 2,000,000 views
```

---

### Bandwidth per day:

```text
2,000,000 × 2MB = 4 TB/day
```

---

## 🧠 Insight

👉 Reads >> Writes in most systems

---

# ⚠️ 6. Key Assumptions (INTERVIEW SECRET)

---

👉 You are NOT expected to be exact

👉 You ARE expected to:

- Make reasonable assumptions
- Explain them clearly

---

## 🎯 Interview Line

👉 “Let me assume X users and Y behavior to estimate scale.”

---

# 💥 7. Real Example Thinking

---

## 🛒 Amazon

Think:

- Millions of users
- Millions of products
- Heavy read traffic

👉 Requires:

- Huge databases
- Massive caching
- CDN

---

# 🧠 8. Common Patterns (MEMORIZE)

---

## 📊 Rule of Thumb

- DAU = 10–30% of total users
- Peak traffic = 5–10× average
- Reads > Writes

---

# 🎯 9. Interview Questions (VERY IMPORTANT)

---

### ❓ Q1: Why do we estimate traffic?

✅ Answer:

To understand system scale and design appropriate architecture.

---

### ❓ Q2: What is RPS?

✅ Answer:

Requests Per Second — number of requests handled by system per second.

---

### ❓ Q3: Why consider peak traffic?

✅ Answer:

Because systems fail during peak load, not average load.

---

### ❓ Q4: What grows faster: compute or storage?

✅ Answer:

Storage often grows faster, especially in media-heavy systems.

---

# 🧠 FINAL INTUITION (Never Forget)

👉 First estimate → then design

NOT:

❌ Design first
❌ Guess later

---

# 🧪 Mini Exercise (Think Like Engineer)

---

Design:

👉 A chat app with 1M users

Think:

- Messages per day?
- Storage needed?
- RPS?

👉 Try rough estimation in your head

---

# 📚 Book References

- _Designing Data-Intensive Applications_ — Martin Kleppmann
- _System Design Interview_ — Alex Xu

---
