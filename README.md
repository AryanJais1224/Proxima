# Proxima

> **A Context-Aware, Proximity-Based Real-Time Networking System**

![Go](https://img.shields.io/badge/Go-1.24-00ADD8?logo=go)
![Redis](https://img.shields.io/badge/Redis-7.x-DC382D?logo=redis)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?logo=postgresql)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker)
![License](https://img.shields.io/badge/License-MIT-green)

---

## Table of Contents

- [Overview](#overview)
- [Application User Interface](#application-user-interface)
- [Demo Video](#demo-video)
- [Tech Stack](#tech-stack)
- [System Architecture](#system-architecture)
- [Engineering Decisions](#engineering-decisions)
- [License](#license)
- [Author](#author)

---

## Overview

Proxima is a real-time, proximity-based networking platform that enables people at the same physical location (hackathons, meetups, conferences, concerts, and other live events) to discover and connect with like-minded individuals nearby.

Traditional networking platforms rely on static profiles and manual searches, often resulting in missed face-to-face collaboration opportunities within shared physical spaces. Proxima bridges the gap between physical presence and digital networking by intelligently matching attendees based on real-time proximity, shared interests, and dynamic professional intent.

Built with a distributed microservice architecture, Proxima combines geospatial intelligence, semantic profile matching, and low-latency communication to deliver a seamless networking experience. By leveraging Redis GEO, PostGIS, pgvector, WebSockets, and NLP-based embeddings, the platform enables instant participant discovery, context-aware recommendations, and scalable real-time interactions while maintaining user privacy.

### Core Engineering Objectives

- Achieve **sub-second** nearby participant discovery using **Redis GEO**
- Enable **low-latency bi-directional communication** through persistent **WebSockets**
- Perform **context-aware matchmaking** using **NLP semantic embeddings** and **pgvector**
- Support **precise indoor and outdoor proximity detection** with a hybrid **GPS + BLE** architecture
- Build an **event-driven, horizontally scalable microservice ecosystem** using **Redis Streams**
- Ensure **privacy-conscious location sharing** with user-controlled visibility and secure authentication

### Core Features

- Real-time nearby participant discovery
- Smart matchmaking based on professional intent
- Instant messaging with persistent WebSockets
- Hybrid GPS + BLE proximity detection
- NLP-powered semantic recommendations
- Redis GEO & PostGIS-powered geospatial search
- pgvector-based semantic similarity search
- Event-driven matchmaking with Redis Streams
- Privacy-first networking
- Live participant presence
- Highly scalable microservice architecture

---

# Application User Interface

<table align="center">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/d37003ca-fe3e-4e80-ae88-3768fcb2b53b" width="320" height="620" alt="Proxima 1">
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/951cddf1-e832-4719-950b-c4d8121a7e1e" width="320" height="620" alt="Proxima 2">
    </td>
  </tr>

  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/5e9dd887-5a6b-409e-9522-033633e8cf18" width="320" height="620" alt="Proxima 3">
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/5044c676-ac05-4cd6-ba53-8e0566ac4dd9" width="320" height="620" alt="Proxima 4">
    </td>
  </tr>

  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/7f738e13-45f3-427c-a735-b3ed60fcf17e" width="320" height="620" alt="Proxima 5">
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/d2a256ec-54cb-4bd4-b8fd-c0d2b6f87d5d" width="320" height="620" alt="Proxima 6">
    </td>
  </tr>

  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/3edffdc2-e836-449e-8509-830373191cd6" width="320" height="620" alt="Proxima 7">
    </td>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/c29b604a-b1ad-40b4-bfae-40b4ff1ce24f" width="320" height="620" alt="Proxima 8">
    </td>
  </tr>
</table>

Note: All screenshots, user profiles, and contact details displayed in this documentation are mock data created for demonstration purposes only.

---

# Demo Video

<video src="https://github.com/user-attachments/assets/285cd759-6faf-452d-93d3-c6a2c862bc6f"
       controls
       width="900">
</video>

---

# Tech Stack

| Layer | Technologies | Purpose |
|-------|--------------|---------|
| **API Gateway** | Go, Gin, gorilla/websocket | REST API routing, WebSocket upgrades, JWT authentication, request validation, rate limiting |
| **Backend Services** | Go, Gin, sqlx | Core business logic, profile management, matchmaking orchestration, signed URL generation |
| **Worker Engine** | Go, Redis Streams | Asynchronous job processing, geospatial matchmaking, semantic search execution |
| **ML Inference Service** | Python, Flask, Sentence Transformers, PyTorch | Intent embedding generation |
| **Database** | PostgreSQL (Supabase), PostGIS, pgvector | Persistent storage, geospatial queries, vector similarity search (ANN/HNSW) |
| **Caching & Messaging** | Redis | GEO indexing, Pub/Sub messaging, Streams, caching, distributed event bus |
| **Authentication** | Firebase Authentication, Firebase Admin SDK | User authentication, JWT verification, secure session management |
| **Real-Time Communication** | WebSockets | Persistent bi-directional communication, live messaging, presence updates |
| **Storage** | Supabase Storage | Avatar uploads and signed asset URLs |
| **Deployment** | Docker, Docker Compose | Containerization, service orchestration, local development |

---

# System Architecture

<p align="center">
  <img src="https://github.com/user-attachments/assets/42efa0bf-2a77-445a-87ee-a596f087b338"
       alt="High Level Design"
       width="100%">
</p>

---

# Engineering Decisions

The architecture of **Proxima** was designed to prioritize **low latency**, **horizontal scalability**, and **real-time responsiveness** while maintaining a clean separation of concerns across services.

| Design Decision | Rationale |
|-----------------|-----------|
| **Why Microservices?** | Separates API Gateway, backend services, worker engine, and ML inference into independent services, allowing each component to scale, deploy, and fail independently without affecting the rest of the system. |
| **Why WebSockets instead of HTTP Polling?** | Persistent bi-directional connections eliminate repetitive polling, significantly reducing network overhead while enabling instant messaging, live presence updates, and real-time participant discovery. |
| **Why Redis GEO?** | Provides sub-second geospatial indexing and radius-based queries, enabling efficient discovery of nearby users without repeatedly scanning the primary database. |
| **Why Redis Streams?** | Implements an event-driven matchmaking pipeline with durable queues, backpressure handling, and asynchronous processing, preventing heavy discovery workloads from blocking API requests. |
| **Why PostgreSQL + PostGIS?** | PostGIS enables native spatial indexing and distance calculations while preserving ACID guarantees, removing the need for a dedicated geospatial database. |
| **Why pgvector?** | Performs Approximate Nearest Neighbor (ANN) search directly inside PostgreSQL, allowing semantic similarity and relational queries to coexist in a single transactional database. |
| **Why a Dedicated Python ML Service?** | Isolates PyTorch inference from Go services, preventing increased memory usage and garbage collection overhead while allowing ML models to be updated independently. |
| **Why Hybrid GPS + BLE Detection?** | GPS provides reliable outdoor positioning, while Bluetooth Low Energy improves indoor proximity detection where GPS accuracy is limited. |
| **Why Event-Driven Processing?** | Computationally intensive tasks such as matchmaking and semantic ranking are executed asynchronously, ensuring API endpoints remain responsive under high user concurrency. |
| **Why Supabase Storage?** | Offloads media storage from backend services while providing secure signed URLs for controlled access to user-uploaded assets. |
| **Why Firebase Authentication?** | Delegates authentication and JWT verification to a trusted identity provider, simplifying security while supporting scalable user management. |

---

# License

Distributed under the **MIT License**.

See the `LICENSE` file for more information.

---

# Author

**Aryan Jaiswal**

- GitHub: https://github.com/AryanJais1224
- LinkedIn: https://www.linkedin.com/in/aryan-jaiswal-618965256/

---
