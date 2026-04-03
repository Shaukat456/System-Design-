---
---

# 📘 LESSON 16: Amazon / E-Commerce System Design

We will design a system like:

👉 Amazon

---

# 🧠 1. Problem Understanding

---

When you open Amazon:

You can:

- Search products
- View product details
- Add to cart
- Place orders
- Track delivery

---

# 🎯 2. Functional Requirements

---

We must support:

### 🛍️ 1. Product catalog

- List millions of products

### 🔍 2. Search system

- Fast product search

### 🛒 3. Cart system

- Add/remove items

### 📦 4. Order system

- Place orders

### 🚚 5. Delivery tracking

- Track shipment

---

# ⚡ 3. Non-Functional Requirements

---

We design for:

- Massive scale (millions of users)
- High availability
- Low latency search
- Consistency in orders
- Fault tolerance

---

# 🧩 4. Core Challenge

---

👉 Biggest problems:

### 1. Inventory consistency

### 2. Search at scale

### 3. Order correctness (no double-selling)

---

# 🏗️ 5. High-Level Architecture

---

```text id="arch1"
User
 ↓
API Gateway
 ↓
Microservices Layer
   ├── Product Service
   ├── Search Service
   ├── Cart Service
   ├── Order Service
   ├── Payment Service
 ↓
Databases + Cache + Queue
```

---

# 🛍️ 6. Product Service

---

Stores product details:

```text id="db1"
product_id | name | price | stock | category
```

---

# 🔍 7. Search System

---

We use:

👉 Elasticsearch

---

## Why?

- Fast full-text search
- Filtering (price, category)
- Ranking results

---

# 🛒 8. Cart System

---

Cart is temporary:

```text id="cart1"
user_id → [product_id, quantity]
```

---

👉 Stored in:

- Redis (fast access)
- Optional DB backup

---

# 📦 9. Order System (MOST IMPORTANT)

---

## Step-by-step:

```text id="order1"
1. User places order
2. System checks stock
3. Reserve inventory
4. Create order
5. Process payment
6. Confirm order
```

---

# ⚠️ 10. Inventory Problem (CRITICAL)

---

👉 Problem:

Two users buy last item at same time

---

## Solution: LOCKING

We use:

- DB transactions
- or distributed locks

---

## Example:

```text id="lock1"
Check stock → lock product → reduce stock → release lock
```

---

# 💳 11. Payment System

---

We integrate:

- Payment gateway
- Retry mechanism
- Failure handling

---

```text id="pay1"
Order → Payment → Success → Confirm
```

---

# 📦 12. Order Status Flow

---

```text id="status1"
Created → Paid → Packed → Shipped → Delivered
```

---

# 🚚 13. Delivery System

---

We track:

- Warehouse dispatch
- Courier tracking
- Real-time updates

---

---

# ⚡ 14. Scaling the System

---

## 🔷 1. Microservices

Each service independent:

- Order service
- Payment service
- Product service

---

## 🔷 2. Caching

👉 Redis for:

- product pages
- search results

---

## 🔷 3. Database scaling

- Sharding by user_id or product_id

---

## 🔷 4. Event system

👉 Apache Kafka

Used for:

- order events
- payment events
- delivery updates

---

# 🧠 15. Consistency vs Availability

---

## Important tradeoff:

| System | Priority             |
| ------ | -------------------- |
| Orders | Strong consistency   |
| Search | Eventual consistency |
| Cart   | Fast + flexible      |

---

# ⚠️ 16. Edge Cases

---

## ❗ Payment fails

✔ rollback inventory

---

## ❗ Stock mismatch

✔ re-check before final order

---

## ❗ High traffic sale (Black Friday)

✔ queue requests + rate limiting

---

# 🚀 17. Final Architecture

---

```text id="final1"
User
 ↓
API Gateway
 ↓
Microservices
   ├── Product
   ├── Search (Elastic)
   ├── Cart (Redis)
   ├── Order
   ├── Payment
   ├── Delivery
 ↓
Databases + Kafka + Cache
```

---

# 🧠 18. Key Insights (VERY IMPORTANT)

---

## 🔥 Amazon system = 6 core ideas:

### 1. Microservices architecture

### 2. Inventory consistency (hardest part)

### 3. Search engine (ElasticSearch)

### 4. Event-driven system (Kafka)

### 5. Cache everywhere (Redis)

### 6. Order correctness (critical logic)

---

# 🎯 19. Interview Questions

---

### ❓ Q1: How do you handle inventory consistency?

✔ Answer:
Using DB transactions or distributed locking before confirming order.

---

### ❓ Q2: Why microservices?

✔ Answer:
To scale different components independently.

---

### ❓ Q3: How does search work?

✔ Answer:
Using Elasticsearch with indexing and ranking.

---

### ❓ Q4: What happens if payment fails?

✔ Answer:
Order is cancelled and inventory is released.

---

# 📚 References

- _Designing Data-Intensive Applications_ — Martin Kleppmann
- _System Design Interview_ — Alex Xu
- Amazon architecture engineering blogs

---

# 🧠 FINAL SUMMARY (THIS IS IMPORTANT)

---

Now you have learned **real-world system design foundations**:

- URL Shortener → basics of scaling
- Instagram → feed systems
- WhatsApp → real-time systems
- YouTube → CDN + streaming
- Uber → geo-distributed systems
- Amazon → microservices + consistency

---
