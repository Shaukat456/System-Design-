---
---

# 📘 CASE STUDY 2: DESIGN A WHATSAPP-LIKE MESSAGING SYSTEM

We are designing something like:

👉 WhatsApp

This is a **core interview system design question** because it tests:

- real-time systems
- networking concepts
- scalability
- distributed systems thinking

---

# 🧠 1. PROBLEM STATEMENT

---

We need to build a system that allows:

## 💬 Functional Requirements:

- Send messages instantly (1–1 chat)
- Group chats
- Online/offline messaging
- Message delivery status:
  - sent ✔
  - delivered ✔✔
  - read ✔✔ (blue ticks)

- Media sharing (images, files)

---

## ⚙️ Non-Functional Requirements:

- Extremely low latency (real-time)
- Highly available (always works)
- Works on slow networks
- Scales to billions of users
- Reliable message delivery (no loss)

---

# 🧠 2. CORE IDEA (VERY IMPORTANT)

---

Unlike URL shortener:

👉 This is NOT request-response
👉 This is REAL-TIME COMMUNICATION

So we need:

### 🔥 Persistent connection (not HTTP request/response)

We use:

👉 WebSockets (or long polling fallback)

---

# ⚡ 3. HIGH LEVEL ARCHITECTURE

---

```text id="wa1"
User A ↔ WebSocket Server ↔ Message Queue ↔ WebSocket Server ↔ User B
```

---

Or full system:

```text id="wa2"
Mobile App
   |
Load Balancer
   |
Chat Server (WebSocket)
   |
Message Queue (Kafka/RabbitMQ)
   |
Database + Cache
   |
Other Chat Server → Recipient
```

---

# 🧠 4. KEY COMPONENTS

---

## 1️⃣ WebSocket Server

👉 Maintains live connection between user and server

✔ No repeated HTTP requests
✔ Real-time messaging

---

## 2️⃣ Message Queue

Used for:

- decoupling sender & receiver
- handling spikes
- ensuring delivery

Example:
👉 Apache Kafka

---

## 3️⃣ Database

Stores:

- messages
- chats
- delivery status

We use:

- NoSQL (MongoDB / Cassandra style)
- for scalability

---

## 4️⃣ Cache

Used for:

- online users
- recent chats
- fast retrieval

---

# 🔄 5. HOW MESSAGE FLOW WORKS

---

## Step-by-step:

### 💬 Step 1: User sends message

```text id="m1"
User A → WebSocket Server
```

---

### 💬 Step 2: Server processes message

```text id="m2"
WebSocket Server → Message Queue
```

---

### 💬 Step 3: Message stored

```text id="m3"
Queue → Database (for persistence)
```

---

### 💬 Step 4: Delivery attempt

```text id="m4"
Server → checks if User B is online
```

---

### 💬 Step 5A: If ONLINE

```text id="m5"
Server pushes message instantly via WebSocket
```

---

### 💬 Step 5B: If OFFLINE

```text id="m6"
Message stays in DB → delivered when user comes online
```

---

# 📡 6. ONLINE/OFFLINE SYSTEM

---

We maintain:

```text id="status1"
User Status Table:
- user_id
- online/offline
- last_seen
```

---

✔ Updated via heartbeat signals

---

# 📲 7. DELIVERY ACK SYSTEM (VERY IMPORTANT)

---

We track message states:

## States:

```text id="state1"
SENT → DELIVERED → READ
```

---

## Flow:

- SENT → server received message
- DELIVERED → recipient got it
- READ → user opened chat

---

Each update is sent back through WebSocket.

---

# 🧠 8. DATABASE DESIGN

---

## Messages Table:

```sql id="db1"
MESSAGE
-------
message_id
sender_id
receiver_id
content
timestamp
status
```

---

## Chat Table:

```sql id="db2"
CHAT
----
chat_id
user_1
user_2
last_message
```

---

# ⚡ 9. SCALING PROBLEMS & SOLUTIONS

---

## ❌ Problem 1: Too many connections

✔ Solution:

- horizontal scaling of WebSocket servers
- load balancer

---

## ❌ Problem 2: Hot users (celebrity problem)

✔ Solution:

- partition chat servers
- sharding by user_id

---

## ❌ Problem 3: Message spikes

✔ Solution:

- message queue buffers load

---

## ❌ Problem 4: Offline users

✔ Solution:

- persistent storage (DB)

---

# 🔐 10. RELIABILITY STRATEGIES

---

We ensure:

- retry mechanism
- message acknowledgment
- idempotency (no duplicates)

---

# ⚖️ 11. TRADEOFFS

---

## 1. Real-time vs battery usage

- WebSockets consume battery
- but give instant messaging

---

## 2. Consistency vs speed

- slight delay allowed
- but no message loss allowed

---

## 3. Push vs Pull

- push = real-time (preferred)
- pull = fallback

---

# 🧠 12. CONCEPTS USED

---

| Concept          | Where used               |
| ---------------- | ------------------------ |
| WebSockets       | real-time messaging      |
| Load Balancer    | traffic distribution     |
| Message Queue    | reliability              |
| NoSQL DB         | scalable storage         |
| Cache            | fast access              |
| Heartbeat system | online/offline detection |

---

# 🔥 13. INTERVIEW QUESTIONS

---

### ❓ Q1: Why WebSockets instead of HTTP?

✔ Answer:
Because HTTP is request-response; WebSockets enable persistent real-time connection.

---

### ❓ Q2: How do you handle offline users?

✔ Answer:
Store messages in DB and deliver when user reconnects.

---

### ❓ Q3: What happens if message queue fails?

✔ Answer:
Use replication + persistent logs (Kafka durability).

---

### ❓ Q4: How do you ensure message order?

✔ Answer:
Use timestamps or sequence IDs per chat.

---

### ❓ Q5: How do you scale WhatsApp?

✔ Answer:

- shard by user_id
- horizontal scaling of servers
- message queues
- caching online users

---

# 🧠 FINAL SUMMARY

---

A WhatsApp-like system is:

👉 A **real-time distributed messaging system**

Core pillars:

- WebSockets (real-time)
- Message Queues (reliability)
- NoSQL DB (scale)
- Cache (speed)
- Horizontal scaling (massive users)

---
