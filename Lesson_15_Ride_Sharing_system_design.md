
---

# 📘 LESSON 15: Uber / Ride Sharing System Design

We will design a system like:

👉 Uber

---

# 🧠 1. Problem Understanding

---

When a rider requests a ride:

```text id="ride1"
Rider → “I need a ride”
Driver nearby gets request
Match happens instantly
```

---

# 🎯 2. Functional Requirements

---

We must support:

### 🚗 1. Ride booking

* Request a ride (pickup → destination)

### 📍 2. Driver matching

* Find nearest available driver

### 🧭 3. Live tracking

* Show driver movement in real-time

### 💳 4. Payments

* Pay after trip ends

---

# ⚡ 3. Non-Functional Requirements

---

We design for:

* Very low latency matching (< 2 seconds)
* High scalability (millions of rides)
* Real-time updates
* Fault tolerance
* Accurate location tracking

---

# 🧩 4. Core Challenge

---

👉 The hardest problem:

# “How do we match rider with nearest driver in real-time?”

---

# 🧠 5. Key Idea: Geo-Spatial Indexing

---

We need fast location search.

We use:

👉 grid-based system / geo-hashing

---

## Example:

```text id="grid1"
City divided into small grids

Each grid stores available drivers
```

---

# 🏗️ 6. High-Level Architecture

---

```text id="arch1"
Rider App
   ↓
API Gateway
   ↓
Ride Matching Service
   ↓
Geo Index (Drivers)
   ↓
Driver App (WebSocket updates)
```

---

# 📍 7. Driver Location Tracking

---

Drivers continuously send location:

```text id="loc1"
Driver → sends GPS every 2–5 seconds
```

Stored in:

* Geo database (Redis GEO / PostGIS)

---

# 🧠 8. Ride Matching Flow

---

## Step-by-step:

```text id="match1"
1. Rider requests ride
2. System finds nearby grid
3. Fetch available drivers
4. Rank drivers
5. Send request to top driver
```

---

# ⚡ 9. Driver Selection Strategy

---

We rank drivers using:

* Distance
* Traffic conditions
* Driver rating
* Availability

---

## Example scoring:

```text id="score1"
score = distance_weight + rating + acceptance_rate
```

---

# 🔁 10. Real-Time Communication

---

We use:

👉 WebSockets

```text id="ws1"
Rider ↔ Server ↔ Driver
```

---

# 🧾 11. Database Design

---

## Riders

```text id="db1"
rider_id | name | rating
```

---

## Drivers

```text id="db2"
driver_id | location | status (available/busy)
```

---

## Trips

```text id="db3"
trip_id | rider_id | driver_id | status | fare
```

---

# 📍 12. Geo-Spatial Indexing (IMPORTANT)

---

We use:

👉 Redis

---

## Why Redis GEO?

* Fast lookup of nearby drivers
* Supports radius queries

---

## Query example:

```text id="geo1"
Find drivers within 3 km radius
```

---

# 🚗 13. Ride Lifecycle

---

```text id="life1"
1. Request
2. Match driver
3. Driver accepts
4. Trip starts
5. Trip ends
6. Payment processed
```

---

# 💳 14. Payment System

---

We integrate:

* Payment gateway
* Post-trip billing

---

```text id="pay1"
Trip ends → calculate fare → charge user
```

---

# ⚡ 15. Scaling the System

---

## 🔷 1. Geo sharding

Split cities into regions

---

## 🔷 2. Driver caching

Store active drivers in memory

---

## 🔷 3. Event queue

Use message queue:

👉 Apache Kafka

---

## 🔷 4. Load balancing

Distribute requests across servers

---

# ⚠️ 16. Edge Cases

---

## ❗ No driver available

✔ show “try again” or increase radius

---

## ❗ Driver cancels

✔ re-match system triggered

---

## ❗ GPS delay

✔ fallback to last known location

---

# 🚀 17. Final Architecture

---

```text id="final1"
Rider App
   ↓
API Gateway
   ↓
Matching Service
   ↓
Geo Index (Redis)
   ↓
Driver WebSocket Service
   ↓
Trip + Payment Service
```

---

# 🧠 18. Key Insights (VERY IMPORTANT)

---

## 🔥 Uber system = 5 core ideas:

### 1. Geo-spatial indexing → fast matching

### 2. Real-time communication → WebSockets

### 3. Event-driven system → Kafka

### 4. Distributed architecture → scalability

### 5. State tracking → trip lifecycle

---

# 🎯 19. Interview Questions

---

### ❓ Q1: How do you find nearest driver?

✔ Answer:
Using geo-spatial indexing (grid system or Redis GEO queries).

---

### ❓ Q2: How do you ensure low latency?

✔ Answer:
Precomputed driver locations + in-memory caching.

---

### ❓ Q3: What happens if driver disconnects?

✔ Answer:
System re-matches rider with another driver.

---

### ❓ Q4: How do you scale globally?

✔ Answer:
Geo-sharding by city/region + distributed services.

---

# 📚 References

* *Designing Data-Intensive Applications* — Martin Kleppmann
* *System Design Interview* — Alex Xu
* Uber engineering blog (dispatch system)

---

