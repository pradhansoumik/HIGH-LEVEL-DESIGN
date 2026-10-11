# High Level Design (HLD)

This repo contains a topic-wise High Level Design roadmap, conceptual notes, and case studies for system design interviews and scalable architecture preparation.

## Overview

The goal is to build a strong foundation in distributed systems design by understanding the core principles, trade-offs, and decision-making patterns that shape large-scale systems.

Follow this sequence for better understanding:

1. Start with the fundamentals of system design.
2. Understand the core building blocks and non-functional requirements.
3. Learn scalability, reliability, consistency, and load handling mechanisms.
4. Study databases, sharding, indexing, caching, and networking patterns.
5. Practice the HLD interview framework and real-world case studies.

---

## Core HLD Topics

### 1. Definition

- What is system design?
- Designing scalable, reliable distributed systems
- HLD vs LLD
- Overview of the Software Development Life Cycle (SDLC)

### 2. Components

- Client
- Server
- Load Balancer
- Web Server
- Application Server
- Cache
- Database

### 3. Key Characteristics

- Scalability
- Availability
- Consistency
- Load Balancing
- Caching
- CDN
- Data Partitioning
- CAP Theorem
- Choice of DB
- Reliability
- Efficiency
- Manageability
- Resiliency

### 4. Scalability

- Definition and real-life examples
- Horizontal scaling
- Vertical scaling
- Auto-scaling

### 5. Availability

- Definition and real-life examples
- Reliability benchmark (ideally 9s or 11s)

### 6. Consistency

- Definition and real-life examples
- Strong consistency
- Eventual consistency

### 7. CAP Theorem

- Consistency, availability, and partition tolerance trade-offs
- Reasoning behind CAP theorem
- CP vs AP systems and when to choose each

### 8. Load Balancing

- Definition and examples such as Nginx, AWS ELB, Citrix ADC, Google Cloud Load Balancer
- Placement of load balancers in different layers
- Benefits of load balancing
- Smart predictive load balancers
- Load balancing algorithms:
  - Least Connections
  - Least Response
  - Round Robin
  - IP Hashing
  - Least Bandwidth

### 9. Caching

- Definition and principle of caching
- Real-life examples: Redis, Memcached, Hazelcast
- Cache hit, cache miss, hit rate, miss rate
- Cache invalidation
- Cache write strategies:
  - Write-through
  - Write-around
  - Write-back
- Cache eviction policies:
  - FIFO
  - LIFO
  - LRU
  - LFU
  - MRU
- Cache avalanche

### 10. Content Delivery Network (CDN)

- Serving large amounts of static media
- Distributed across geographies
- Performance and latency improvements

### 11. Short Polling, Long Polling, WebSocket, and Server-Sent Events

- Polling technique
- Short polling / AJAX-based polling
- Long polling
- WebSockets for full-duplex communication
- Server-Sent Events (SSE)

### 12. Database, Data Partitioning / Sharding, and Indexing

- SQL and NoSQL databases
- MySQL, Postgres, Oracle, MongoDB, Cassandra, etc.
- Choice of DB and trade-offs
- Data partitioning
- Partitioning methods:
  - Horizontal / sharding
  - Vertical
  - Directory-based
- Partitioning criteria:
  - Key/hash-based
  - List-based
  - Round-robin
  - Composite/hybrid
- Problems with partitioning:
  - Joins
  - Referential integrity
  - Rebalancing
- Database indexing
- Impact of indexing on writes
- Indexing structures:
  - B+ Tree
  - Hash Tables
  - R-Tree
  - Bitmap Index

### 13. HLD Template — How to Approach an HLD Interview

- Requirements clarification
  - Functional
  - Non-functional
  - Extended requirements
- Back-of-the-envelope calculations
  - Capacity estimation
  - DAU / MAU
- System API
  - Endpoints and request/response models
- Defining DB schema
  - Data types and DB selection
- High-level design
  - Client, server, database, and caching
- Detailed component-level design
- Bottlenecks and trade-offs
- Telemetry and analytics

### 14. Case Studies

- URL Shortener, Pastebin
- Instagram
- Twitter
- WhatsApp
- Uber
- Google Drive
- YouTube
- Netflix etc.

---

## Suggested Learning Order

1. Definition and core system design fundamentals
2. Components and non-functional requirements
3. Scalability, availability, consistency, and CAP theorem
4. Load balancing, caching, and CDN
5. Communication patterns: polling, WebSockets, SSE
6. Databases, partitioning, sharding, and indexing
7. HLD interview framework
8. Real-world case studies

---

## Useful Mindset

A strong HLD understanding is not just about naming systems or components; it is about making the right trade-offs under constraints such as latency, consistency, cost, reliability, and operational complexity.

This repo aims to help you practice exactly that.
