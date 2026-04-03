---
---

# 📘 CASE STUDY 4: DESIGN A YOUTUBE-LIKE VIDEO STREAMING SYSTEM

We are designing something like:

👉 YouTube

This system introduces **new hard concepts**:

- video storage at massive scale
- streaming (not downloading)
- CDN networks
- encoding/transcoding pipelines
- buffering + adaptive quality

---

# 🧠 1. PROBLEM STATEMENT

---

We need to build a system where users can:

## 📌 Functional Requirements:

- Upload videos
- Watch videos instantly
- Search videos
- Like / comment
- Subscribe to channels
- Stream in different qualities (144p → 4K)

---

## ⚙️ Non-Functional Requirements:

- Ultra low buffering
- Scalable to billions of videos
- High availability (24/7)
- Efficient bandwidth usage
- Fast global delivery

---

# 🧠 2. CORE PROBLEM

---

Video system is NOT like normal web apps.

👉 Videos are **huge files**
👉 Users are **global**
👉 Network speeds vary

So we must solve:

> “How do we deliver large video files smoothly to millions of users?”

---

# ⚡ 3. HIGH LEVEL ARCHITECTURE

---

```text id="yt1"
User → Load Balancer → API Server → Metadata DB
                         |
                         ↓
                    Video Storage (Blob Storage)
                         |
                         ↓
                        CDN
                         |
                         ↓
                   User Streaming Player
```

---

# 🧠 4. KEY COMPONENTS

---

## 1️⃣ Video Storage (Blob Storage)

We store raw video files in:

👉 Object Storage (like S3 style systems)

Stores:

- original video
- processed versions

---

## 2️⃣ CDN (Content Delivery Network) ⭐ MOST IMPORTANT

👉 This is the HEART of YouTube

Example:

👉 Cloudflare CDN

---

## Why CDN?

Without CDN:

❌ User in Pakistan downloads from US server → slow

With CDN:

✔ Video served from nearby server → fast

---

## 3️⃣ Transcoding Service

We convert video into multiple qualities:

```text id="t1"
Original Video → 1080p → 720p → 480p → 144p
```

---

✔ enables adaptive streaming

---

## 4️⃣ Metadata Database

Stores:

```sql id="db1"
VIDEO
-----
video_id
title
description
user_id
views
```

---

# 🔄 5. VIDEO UPLOAD FLOW

---

## STEP 1: Upload

```text id="u1"
User → API Server → Blob Storage
```

---

## STEP 2: Trigger processing

```text id="u2"
Storage → Queue → Transcoding Service
```

---

## STEP 3: Create multiple versions

```text id="u3"
Original → 1080p, 720p, 480p
```

---

## STEP 4: Store processed videos

```text id="u4"
Store all versions in storage + CDN cache
```

---

# 🎥 6. VIDEO WATCH FLOW

---

## STEP 1: User requests video

```text id="w1"
User → API Server → Metadata DB
```

---

## STEP 2: Get video URL

Returns CDN link:

```text id="w2"
cdn.youtube.com/video_720p.mp4
```

---

## STEP 3: Stream from nearest CDN

```text id="w3"
CDN → User Player
```

---

# ⚡ 7. ADAPTIVE BITRATE STREAMING

---

This is VERY important concept.

If network is slow:

```text id="a1"
Switch 1080p → 720p → 480p automatically
```

---

✔ prevents buffering
✔ improves user experience

---

# ⚙️ 8. BUFFERING SYSTEM

---

Player works like:

- downloads small chunks
- buffers ahead
- switches quality dynamically

---

```text id="b1"
Download → Buffer → Play → Preload next chunk
```

---

# 🧠 9. SEARCH SYSTEM

---

We need fast search:

- video titles
- tags
- descriptions

We use:

👉 inverted index (like Google search)

---

# 📊 10. SCALING PROBLEMS

---

## ❌ Problem 1: Huge video files

✔ Solution:

- chunk upload
- distributed storage

---

## ❌ Problem 2: Global traffic

✔ Solution:

- CDN caching

---

## ❌ Problem 3: Processing overload

✔ Solution:

- async queue (Kafka-style system)

---

## ❌ Problem 4: Viral videos

✔ Solution:

- replicate video across multiple CDN nodes

---

# ⚖️ 11. TRADEOFFS

---

## 1. Storage vs Cost

- storing multiple resolutions increases cost
- but improves performance

---

## 2. Speed vs Consistency

- CDN may serve slightly outdated version
- but improves speed massively

---

## 3. Preprocessing vs On-demand encoding

- preprocessing = faster playback
- on-demand = cheaper but slow

---

# 🧠 12. CONCEPTS USED

---

| Concept        | Usage                |
| -------------- | -------------------- |
| CDN            | video delivery       |
| Blob storage   | video storage        |
| Message queue  | transcoding pipeline |
| Load balancing | traffic handling     |
| Caching        | fast streaming       |
| Sharding       | metadata scaling     |

---

# 🔥 13. INTERVIEW QUESTIONS

---

### ❓ Q1: Why do we need CDN?

✔ Answer:
To reduce latency by serving content from nearby servers.

---

### ❓ Q2: How does video streaming work?

✔ Answer:
Video is broken into chunks and streamed progressively using buffer.

---

### ❓ Q3: Why multiple video resolutions?

✔ Answer:
To support adaptive bitrate streaming based on network speed.

---

### ❓ Q4: How do you handle viral videos?

✔ Answer:
Replicate content across multiple CDN nodes globally.

---

### ❓ Q5: How is video uploaded efficiently?

✔ Answer:
Chunked upload + async processing pipeline.

---

# 🧠 FINAL SUMMARY

---

YouTube-like system =

👉 A **massive distributed video storage + streaming + CDN system**

Core pillars:

- CDN (global delivery)
- Transcoding pipeline
- Blob storage
- Adaptive streaming
- Search indexing

---
