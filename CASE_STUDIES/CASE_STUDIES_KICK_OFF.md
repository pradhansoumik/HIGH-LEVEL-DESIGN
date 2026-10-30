# Case Studies Kick Off

## System Design Interviews: A Step By Step Guide

Example:

- Design Instagram

---

## Pattern / Template

### 1) Requirements Clarification

- Functional Requirements: defines the functionality and feature aspects

  Example:

  - Uploading Pics
  - Like
  - Message
  - Comments
  - Stories

- Non-Functional Requirements

  Example:

  - Availability
  - Low Latency
  - Consistency
  - Scalability

- Additional Requirements

  Example:

  - Good to have
  - Support Payments on Instagram

### 2) Back of the Envelope Calculation

- Scale: DAU (Daily Active Users), MAU (Monthly Active Users)
- Load Balancing
- Caching
- Partitioning
- Memory

Example:

For Instagram:

- Total Active Users: 3 Bn
- Monthly Active Users: 1.5 Bn

### 3) System Interface Design / System API

Example: For Instagram, system API:

- POST: /instagram/post/v1/
- GET: /instagram/get/v1/{postID}

### 4) Defining the DB Schema

- Type of Data to Store
- Choice of DB
- Mapping across different tables/databases

### 5) High Level Design

- Client
- Server
- Application Server
- Database(s)
- Caching

### 6) Detailed Component Level Design

- Load Balancing
- Caching
- CDN
- DB Partitioning

### 7) Bottlenecks / Trade-offs

- Single Point of Failure?
- Enough Replicas to Store Data?
- Auto Scaling
- Error / Exception Scenarios

### 8) Telemetry / Analytics

- Monitoring
- Performance

---

## Interview Mindset

A good HLD interview answer is not just about naming technologies; it is about showing a structured thought process:

1. Clarify requirements
2. Estimate scale
3. Define API contracts
4. Decide database strategy
5. Design the high-level system
6. Deep dive into critical components
7. Discuss trade-offs and bottlenecks
8. Wrap up with observability and analytics

This is the standard way to approach any system design case study.
