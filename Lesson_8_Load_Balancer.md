---
👉 **Scaling + Traffic + Caching → all depend on this component**
---

# 📘 LESSON 8: Load Balancers (The Backbone of Distributed Systems)

---

# 🧠 1. What is a Load Balancer?

👉 A **Load Balancer** is a system that distributes incoming requests across multiple servers.

---

## 🧩 Simple Analogy (Never Forget)

Think of a **traffic police officer 🚦**

- Many cars (requests) coming
- Officer sends them to different roads (servers)

---

## 💻 Without Load Balancer

```text
User → Server 1 💥 (overloaded)
```

---

## 💻 With Load Balancer

```text
User → Load Balancer → Server 1 / Server 2 / Server 3
```

👉 Load is evenly distributed ✅

---

# 💥 2. Why Do We Need Load Balancers?

---

## ⚠️ Problem Without It

- One server gets all traffic
- Crashes 💥
- System goes down

---

## ✅ With Load Balancer

- Traffic distributed
- No overload
- High availability

---

## 🧠 Key Idea

👉 Load balancer = **enabler of horizontal scaling**

---

# 🌍 3. Real Example

---

### YouTube

Millions of users watching videos.

👉 Requests go to:

- Load balancers
- Then distributed to thousands of servers

---

# ⚙️ 4. How Load Balancer Works

---

## Step-by-step flow:

1. User sends request
2. Load balancer receives it
3. Chooses a server
4. Forwards request
5. Returns response

---

# 🔁 5. Load Balancing Algorithms (VERY IMPORTANT)

---

## 🔷 1. Round Robin ⭐

👉 Requests distributed one by one

---

### Example:

```text
Req1 → Server1
Req2 → Server2
Req3 → Server3
Req4 → Server1
```

---

### ✅ Pros

- Simple
- Fair distribution

---

### ❌ Cons

- Ignores server capacity

---

---

## 🔷 2. Least Connections ⭐

👉 Send request to server with least active connections

---

### ✅ Pros

- Smart distribution

---

### ❌ Cons

- Slightly complex

---

---

## 🔷 3. IP Hash

👉 Same user → same server

---

### ✅ Pros

- Good for session-based systems

---

---

## 🔷 4. Weighted Round Robin

👉 Stronger servers get more traffic

---

---

# ⚠️ 6. Health Checks (CRITICAL)

---

## 📌 Problem

What if a server dies?

---

## ✅ Solution

Load balancer performs **health checks**

- If server fails → remove from pool

---

## 🧠 Insight

👉 Prevents sending requests to dead servers

---

# 🔁 7. Types of Load Balancers

---

## 🔷 1. L4 Load Balancer (Transport Layer)

- Works on IP + Port
- Fast

---

## 🔷 2. L7 Load Balancer (Application Layer)

- Works on HTTP/HTTPS
- Smarter routing

---

### 💻 Example:

- Route `/login` → auth server
- Route `/video` → media server

---

## 🧠 Interview Insight

👉 L7 is more flexible but slower than L4

---

# 🌐 8. DNS Load Balancing

---

👉 Multiple IPs for one domain

Example:

```text
google.com → IP1 / IP2 / IP3
```

---

## 🧠 Insight

👉 First level of load balancing

---

# 💥 9. Real System Thinking

---

## 🛒 Amazon

When millions of users visit:

- DNS distributes globally
- Load balancers distribute regionally
- Servers handle requests

---

# ⚠️ 10. Common Problems (INTERVIEW TRAPS)

---

## ❗ 1. Single Point of Failure

If load balancer fails → system down

---

### ✅ Solution:

👉 Use multiple load balancers

---

## ❗ 2. Session Problem

User request goes to different servers

---

### ✅ Solution:

- Sticky sessions
- OR store session in cache (Redis)

---

## ❗ 3. Latency

Extra hop adds delay

---

---

# 🎯 11. Interview Questions (VERY IMPORTANT)

---

### ❓ Q1: What is a load balancer?

✅ Answer:

A load balancer distributes incoming requests across multiple servers to ensure efficient utilization and high availability.

---

### ❓ Q2: Why is it needed?

✅ Answer:

To prevent server overload, enable scalability, and improve system reliability.

---

### ❓ Q3: Round Robin vs Least Connections?

✅ Answer:

Round Robin distributes requests equally, while Least Connections sends requests to the least busy server.

---

### ❓ Q4: What happens if a server fails?

✅ Answer:

Load balancer detects failure via health checks and stops routing traffic to that server.

---

### ❓ Q5: L4 vs L7?

✅ Answer:

L4 works at transport level (faster), L7 works at application level (more flexible).

---

# 🧠 FINAL INTUITION (Never Forget)

👉 Without load balancer = system crashes
👉 With load balancer = system scales

---

👉 It is the **bridge between users and servers**

---

# 🧪 Mini Exercise

Think:

👉 If 1 million users hit ONE server:

What happens?

✔ Crash 💥

---

👉 If 1 million users hit 100 servers via load balancer:

✔ Smooth system ✅

---

# 📚 Book References

- _Designing Data-Intensive Applications_ — Martin Kleppmann
- _System Design Interview_ — Alex Xu

---
