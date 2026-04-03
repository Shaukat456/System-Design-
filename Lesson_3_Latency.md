---
---

# 📘 LESSON 3: Latency, Throughput, Availability

---

# 🧠 1. Why This Lesson Matters

In interviews, when you design something like:

- WhatsApp
- Instagram
- Netflix

👉 You MUST talk about:

- How fast? (Latency)
- How much load? (Throughput)
- How reliable? (Availability)

---

## 🧩 Simple Analogy (Burn this in memory)

Think of a **highway 🚗**

| Concept      | Meaning                    |
| ------------ | -------------------------- |
| Latency      | Time for 1 car to travel   |
| Throughput   | Cars per second            |
| Availability | Is the road open or closed |

---

# ⚡ 2. LATENCY (Speed of response)

---

## 📌 Definition

👉 **Latency = time taken for a request to get a response**

---

## 💻 Example

You open a website:

- Click → response in 100 ms ✅ (fast)
- Click → response in 5 sec ❌ (slow)

---

## 🧠 Formula Thinking

```text
Latency = Response Time
```

---

## 📊 Types of Latency

- Network latency (internet delay)
- Processing latency (server time)
- Database latency

---

## 💥 Real Example

### Google

When you search something:

👉 If it takes 3 seconds → you feel it's slow
👉 If it takes 0.1 sec → feels instant

---

## 🎯 Goal

👉 Always minimize latency

---

# 🚀 3. THROUGHPUT (Capacity)

---

## 📌 Definition

👉 **Throughput = number of requests handled per second**

---

## 💻 Example

A server can handle:

```text
1000 requests/second
```

---

## 🧠 Intuition

- Latency = speed per request
- Throughput = total work per second

---

## 💥 Real Example

### YouTube

Millions of users watching videos simultaneously.

👉 System must handle:

- Millions of requests per second

---

## 🎯 Goal

👉 Increase throughput WITHOUT increasing latency too much

---

# ⚖️ 4. LATENCY vs THROUGHPUT (IMPORTANT TRADE-OFF)

---

## 🧠 Key Idea

👉 Increasing throughput can increase latency

---

### Example:

- Server handles too many requests
  → gets overloaded
  → response slows down

---

## 🧠 Interview Insight

👉 “We need to balance latency and throughput.”

---

# 🌍 5. AVAILABILITY (Reliability)

---

## 📌 Definition

👉 **Availability = system uptime (is it working or not)**

---

## 💻 Example

- Website always works → high availability
- Website crashes often → low availability

---

## 📊 Measured in %

| Availability      | Downtime per year |
| ----------------- | ----------------- |
| 99%               | ~3.6 days         |
| 99.9%             | ~8.7 hours        |
| 99.99%            | ~52 minutes       |
| 99.999% (5 nines) | ~5 minutes        |

---

## 💥 Real Example

### Amazon

Even **1 minute downtime = millions lost 💸**

---

## 🧠 Key Idea

👉 Big systems aim for **99.99%+ availability**

---

# 🔁 6. How to Improve These Metrics

---

## ⚡ Reduce Latency

- Use caching
- Use CDN
- Optimize queries

---

## 🚀 Increase Throughput

- Add more servers (horizontal scaling)
- Use load balancers

---

## 🌍 Improve Availability

- Use multiple servers
- Use failover systems
- Avoid single point of failure

---

# 💥 7. Real System Thinking

Let’s design:

👉 Video streaming app

---

### What matters?

- 🎬 Low latency → fast video start
- 📈 High throughput → many users
- 🌍 High availability → no crashes

---

# 🎯 8. Interview Questions (CRITICAL)

---

### ❓ Q1: What is latency?

✅ Answer:

Latency is the time taken for a request to travel from client to server and back.

---

### ❓ Q2: What is throughput?

✅ Answer:

Throughput is the number of requests a system can process per unit time.

---

### ❓ Q3: Difference between latency and throughput?

✅ Answer:

Latency measures time per request, while throughput measures number of requests per second.

---

### ❓ Q4: What is availability?

✅ Answer:

Availability is the percentage of time a system remains operational and accessible.

---

### ❓ Q5: Why can't we maximize everything?

✅ Answer:

Because improving one metric (like throughput) can negatively affect another (like latency), so trade-offs are required.

---

# 🧠 FINAL INTUITION (Never Forget)

👉 Latency = **How fast?**
👉 Throughput = **How much?**
👉 Availability = **How reliable?**

---

# 🧪 Mini Exercise

Think:

👉 WhatsApp message takes 10 seconds to send

Which metric is bad?

✔ Answer: **Latency**

---

👉 App crashes every hour

✔ Answer: **Availability**

---

👉 Server handles only 10 users at a time

✔ Answer: **Throughput**

---

# 📚 Book References

- _Designing Data-Intensive Applications_ — Martin Kleppmann
- _System Design Interview_ — Alex Xu

---
