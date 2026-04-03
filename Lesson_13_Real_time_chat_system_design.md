---
---

# 📘 LESSON 13: WhatsApp / Real-Time Chat System Design

We will design a system like:

👉 WhatsApp

---

# 🧠 1. Problem Understanding

---

When you send a message:

```text id="chat1"
User A → "Hello"
User B receives instantly
```

So we need:

- Real-time delivery
- Reliability
- Message storage
- Offline support

---

# 🎯 2. Functional Requirements

---

We must support:

### 1. One-to-one chat

- Send messages instantly

### 2. Group chat

- Many users in same conversation

### 3. Message status

- Sent ✔
- Delivered ✔✔
- Read ✔✔ (blue ticks)

### 4. Offline messaging

- If user is offline → deliver later

---

# ⚡ 3. Non-Functional Requirements

---

We design for:

- Ultra low latency (< 100ms)
- High availability
- Massive scale (billions of messages/day)
- Message durability (no loss)

---

# 🧩 4. Core Idea (VERY IMPORTANT)

---

Unlike Instagram:

👉 Chat is NOT request-response
👉 Chat is REAL-TIME CONNECTION

We use:

# 🔌 WebSockets

---

# 🧠 5. What is WebSocket?

---

Instead of HTTP:

```text id="ws1"
Client ↔ Server (persistent connection)
```

✔ Server can push messages instantly
✔ No need to re-request

---

# 🏗️ 6. High-Level Architecture

---

```text id="arch1"
User
 ↓
Load Balancer
 ↓
Chat Server (WebSocket)
 ↓
Message Queue (Kafka)
 ↓
Database
```

---

# 🔁 7. Message Flow (VERY IMPORTANT)

---

## Step-by-step:

```text id="flow1"
User A sends message
→ Chat Server receives via WebSocket
→ Store in DB
→ Push to Kafka
→ Deliver to User B if online
```

---

# 🧾 8. Database Design

---

## Messages Table

```text id="db1"
message_id | sender | receiver | content | timestamp | status
```

---

## Conversation Table

```text id="db2"
chat_id | user1 | user2
```

---

# ⚡ 9. Online vs Offline Users

---

## Online user:

```text id="online1"
Server → instantly push message via WebSocket
```

---

## Offline user:

```text id="offline1"
Store message in DB → deliver when user comes online
```

---

# 📦 10. Message Queue (Kafka)

---

We use:

👉 Apache Kafka

---

## Why Kafka?

- Handles millions of messages/sec
- Decouples systems
- Ensures reliability

---

# 🔔 11. Delivery Guarantees

---

We define 3 levels:

---

## 1. At most once ❌

- Message may be lost

## 2. At least once ✔ (used in chat systems)

- Message always delivered (may duplicate)

## 3. Exactly once ❌ (hard to achieve)

---

# 📡 12. Read Receipts (✔✔ blue ticks)

---

Flow:

```text id="read1"
User B opens chat
→ sends "read event"
→ server updates DB
→ notifies User A
```

---

# ⚡ 13. Scaling the System

---

## Problem:

Billions of messages per day

---

## Solutions:

---

### 🔷 1. Horizontal scaling

- Multiple chat servers

---

### 🔷 2. Sharding DB

```text id="shard1"
Shard by user_id
```

---

### 🔷 3. WebSocket connection manager

Tracks:

- Who is online
- Which server they are connected to

---

### 🔷 4. CDN (for media)

Images/videos sent via CDN

---

# 🧠 14. Presence System (Online/Offline)

---

We maintain:

```text id="presence1"
user_id → online/offline + last seen
```

---

# ⚠️ 15. Edge Cases

---

## ❗ Message duplication

✔ Solution:

- message_id unique constraint

---

## ❗ Server crash

✔ Solution:

- Kafka replay

---

## ❗ User switching devices

✔ Solution:

- sync messages from DB

---

# 🚀 16. Final Architecture

---

```text id="final1"
Client
 ↓
Load Balancer
 ↓
WebSocket Chat Servers
 ↓
Kafka Queue
 ↓
Message DB
 ↓
Presence Service
```

---

# 🧠 17. Key Insights (VERY IMPORTANT)

---

## 🔥 Chat system = 4 core components:

### 1. WebSockets → real-time communication

### 2. Kafka → message reliability

### 3. DB → storage

### 4. Presence service → online tracking

---

# 🎯 18. Interview Questions

---

### ❓ Q1: Why WebSockets instead of HTTP?

✔ Answer:
Because chat requires persistent real-time connection.

---

### ❓ Q2: What happens if server crashes?

✔ Answer:
Kafka ensures messages are not lost and can be replayed.

---

### ❓ Q3: How do you ensure message delivery?

✔ Answer:
At-least-once delivery with acknowledgements.

---

### ❓ Q4: How do you scale WhatsApp?

✔ Answer:
Horizontal scaling + sharding + WebSocket clustering.

---

# 📚 References

- _Designing Data-Intensive Applications_ — Martin Kleppmann
- _System Design Interview_ — Alex Xu
- WhatsApp engineering blogs (Meta infrastructure)

---
