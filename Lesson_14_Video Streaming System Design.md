# A Very high-scale distributed system design problem\*\*.

---

# 📘 LESSON 14: YouTube / Video Streaming System Design

We will design a system like:

👉 YouTube

---

# 🧠 1. Problem Understanding

---

When a user uploads or watches a video:

### Two main actions:

## 📤 Upload

- User uploads video

## 📺 Watch

- Millions of users stream video smoothly

---

# 🎯 2. Functional Requirements

---

We must support:

### 1. Video Upload

- Upload large files (GBs)

### 2. Video Playback

- Stream instantly (no full download)

### 3. Search videos

- Find content quickly

### 4. Like / Comment / Subscribe

---

# ⚡ 3. Non-Functional Requirements

---

We design for:

- Massive scale (billions of users)
- Low latency streaming
- High availability
- Efficient bandwidth usage
- Smooth playback (no buffering)

---

# 🧩 4. Core Challenge

---

👉 Videos are HUGE files

So problem is:

> How do we stream video without downloading full file?

---

# 🧠 5. Core Solution: Streaming + CDN

---

We use:

👉 Content Delivery Network

---

## Idea:

Instead of serving video from one server:

👉 store video in multiple edge servers worldwide

---

# 🏗️ 6. High-Level Architecture

---

```text id="arch1"
User
 ↓
Load Balancer
 ↓
Video API Server
 ↓
Storage (S3-like system)
 ↓
CDN (Edge Servers)
 ↓
User Playback
```

---

# 📤 7. Video Upload Flow

---

## Step-by-step:

```text id="upload1"
User uploads video
→ API server receives file
→ Store in cloud storage
→ Create video metadata
→ Send to CDN for distribution
```

---

## Storage example:

- AWS S3-like object storage
- Chunked upload for large files

---

# 📺 8. Video Playback Flow

---

```text id="play1"
User clicks video
→ CDN checks nearest server
→ If cached → serve instantly
→ If not → fetch from origin server
→ Stream to user
```

---

# ⚡ 9. Why CDN is IMPORTANT

---

Without CDN:

❌ High latency
❌ Buffering
❌ Server overload

With CDN:

✔ Fast playback
✔ Reduced load
✔ Global scalability

---

# 🧠 10. Video Streaming Technique

---

We do NOT send full video.

We use:

## 🎬 Chunk-based streaming

```text id="chunk1"
Video → split into small chunks (2–10 seconds)
```

---

## Protocols:

- HLS (HTTP Live Streaming)
- DASH (Dynamic Adaptive Streaming)

---

# 📊 11. Adaptive Bitrate Streaming

---

👉 Quality changes based on internet speed:

| Internet | Quality |
| -------- | ------- |
| Fast     | 1080p   |
| Medium   | 720p    |
| Slow     | 360p    |

---

# 🧾 12. Database Design

---

## Video Metadata

```text id="db1"
video_id | title | user_id | url | views | likes
```

---

## User Table

```text id="db2"
user_id | name | subscribers
```

---

# 🔍 13. Search System

---

We use:

- Indexing system
- Search engine (Elasticsearch-like)

---

# ⚡ 14. Scaling the System

---

## 🔷 1. Storage scaling

- Object storage (S3)
- Multi-region replication

---

## 🔷 2. CDN scaling

- Global edge servers

---

## 🔷 3. Database scaling

- Sharding by video_id

---

## 🔷 4. Streaming optimization

- Pre-generated video chunks

---

# 📦 15. Video Encoding Pipeline

---

When video is uploaded:

```text id="encode1"
Upload → Raw video
       → Transcoding service
       → Multiple resolutions (360p, 720p, 1080p)
       → Store in CDN
```

---

# ⚠️ 16. Edge Cases

---

## ❗ High traffic video (viral video)

✔ CDN handles load

---

## ❗ Upload failure

✔ retry + chunk upload

---

## ❗ Buffering issues

✔ adaptive bitrate streaming

---

# 🚀 17. Final Architecture

---

```text id="final1"
User
 ↓
Load Balancer
 ↓
API Servers
 ↓
Storage (S3)
 ↓
Encoding Service
 ↓
CDN (Global Edge Servers)
 ↓
Playback
```

---

# 🧠 18. Key Insights (VERY IMPORTANT)

---

## 🔥 YouTube system = 5 core ideas:

### 1. Object storage → video saving

### 2. CDN → fast delivery

### 3. Encoding → multiple qualities

### 4. Chunk streaming → no full download

### 5. Adaptive bitrate → smooth playback

---

# 🎯 19. Interview Questions

---

### ❓ Q1: Why use CDN?

✔ Answer:
To reduce latency and serve video from nearest location.

---

### ❓ Q2: How do you stream large videos?

✔ Answer:
By chunking video and using HLS/DASH protocols.

---

### ❓ Q3: How do you handle viral videos?

✔ Answer:
CDN replication + caching popular content.

---

### ❓ Q4: Why not store video in database?

✔ Answer:
Databases are not optimized for large binary files.

---

# 📚 References

- _Designing Data-Intensive Applications_ — Martin Kleppmann
- _System Design Interview_ — Alex Xu
- Netflix / YouTube engineering blogs

---
