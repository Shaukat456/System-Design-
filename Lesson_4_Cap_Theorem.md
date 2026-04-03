---
---

# 📘 LESSON 4: CAP THEOREM (Consistency, Availability, Partition Tolerance)

---

# 🧠 1. What is CAP Theorem?

👉 CAP Theorem states:

> In a distributed system, you can only guarantee **2 out of 3**:

- **C → Consistency**
- **A → Availability**
- **P → Partition Tolerance**

👉 You CANNOT have all three at the same time ❌

---

## 🧠 First Understand the Words (VERY IMPORTANT)

---

# 🔷 2. CONSISTENCY (C)

---

## 📌 Definition

👉 All users see the **same data at the same time**

---

## 💻 Example

You send ₹100 to your friend:

- You see balance = 900
- Friend sees received = 100

👉 Data is consistent everywhere

---

## ❌ Inconsistency Problem

- You see 900
- Friend sees nothing

👉 System is inconsistent

---

## 🧠 Memory Trick

👉 Consistency = **same truth everywhere**

---

# 🟢 3. AVAILABILITY (A)

---

## 📌 Definition

👉 System **always responds**, even if data is not perfect

---

## 💻 Example

You open app:

- Server responds ✅ (even if slightly outdated data)

---

## ❌ Low Availability

- Server not responding ❌
- Timeout / crash

---

## 🧠 Memory Trick

👉 Availability = **system always replies**

---

# 🌐 4. PARTITION TOLERANCE (P)

---

## 📌 Definition

👉 System continues working **even if network fails between servers**

---

## 💥 Real Problem

In distributed systems:

Servers are in different locations

👉 Network can break!

---

## 💻 Example

Server A ↔ Server B

If connection breaks:

👉 System must still work

---

## 🧠 Memory Trick

👉 Partition = **network failure**

---

# ⚠️ 5. The Core Rule (THIS IS EVERYTHING)

---

👉 When a **network partition happens**, you must choose:

### Either:

- **Consistency (C)** ❗
  OR
- **Availability (A)** ❗

👉 You cannot have both

---

# 🧩 6. Real-Life Analogy (Never Forget)

---

Imagine:

Two bank branches:

- Karachi branch
- Lahore branch

Network breaks between them 💥

---

Now a user withdraws money:

### Option 1 → CONSISTENCY

- System blocks transaction ❌
- Waits until network restores

👉 ✔ Correct data
👉 ❌ Not available

---

### Option 2 → AVAILABILITY

- System allows transaction ✅
- Data may mismatch

👉 ✔ Always works
👉 ❌ Risk of inconsistency

---

👉 This is CAP theorem in real life.

---

# ⚖️ 7. The Three System Types

---

## 🔷 CP System (Consistency + Partition Tolerance)

👉 Sacrifice Availability

---

### Behavior:

- If network fails → system stops responding

---

### 💥 Example

Banking systems 💰

👉 Correct data is more important than availability

---

---

## 🟢 AP System (Availability + Partition Tolerance)

👉 Sacrifice Consistency

---

### Behavior:

- System always responds
- Data may be outdated

---

### 💥 Example

### Facebook

- You post something
- Friends may see it after delay

👉 Eventually consistent

---

---

## 🔴 CA System (Consistency + Availability)

👉 No partition tolerance

---

### ❗ Reality

👉 This is **not practical** in distributed systems

Because:

👉 Network failures are unavoidable

---

# 🧠 8. Golden Rule (INTERVIEW GOLD)

👉 In real-world distributed systems:

✔ Partition tolerance is **mandatory**

👉 So real choice is:

> **CP vs AP**

---

# 🔁 9. Eventual Consistency (IMPORTANT)

---

## 📌 Definition

👉 Data will become consistent **after some time**

---

## 💻 Example

You send message:

- You see it instantly
- Friend sees after 2 seconds

👉 Eventually consistent

---

## 💥 Real Example

### Instagram

- Likes/comments may appear with delay

---

# 🎯 10. Interview Questions (VERY IMPORTANT)

---

### ❓ Q1: What is CAP theorem?

✅ Answer:

CAP theorem states that a distributed system can only guarantee two out of consistency, availability, and partition tolerance.

---

### ❓ Q2: Why is partition tolerance mandatory?

✅ Answer:

Because network failures are inevitable in distributed systems, so systems must tolerate partitions.

---

### ❓ Q3: CP vs AP?

✅ Answer:

- CP ensures data consistency but may become unavailable
- AP ensures availability but may return inconsistent data

---

### ❓ Q4: What is eventual consistency?

✅ Answer:

A model where the system guarantees that data will become consistent over time.

---

# 🧠 FINAL INTUITION (Never Forget)

👉 You cannot have:

✔ Always correct data
✔ Always available
✔ Always working during failures

👉 You must sacrifice one

---

# 🧪 Mini Exercise (Think Like Engineer)

---

## Scenario 1:

Banking system

👉 Choose:

- Consistency OR Availability?

✔ Answer: **Consistency (CP)**

---

## Scenario 2:

Social media feed

👉 Choose:

- Consistency OR Availability?

✔ Answer: **Availability (AP)**

---

# 📚 Book References

- _Designing Data-Intensive Applications_ — Martin Kleppmann
- _System Design Interview_ — Alex Xu

---
