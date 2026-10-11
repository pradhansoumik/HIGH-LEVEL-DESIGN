# System Design

Software System interacting with different components

## Components

- Load Balancers
- Client
- Server
- DB
- Cache
- Web Server
- Application Server

## HLD

- High Level Design
- Overview of different components of system
- And how they interact with each other

## LLD

- Low Level Design
- UML Diagrams: Class Diagrams, Activity Diagrams, Use Case Diagrams
- Design Principles and Best Practises: SOLID, KISS, YAGNI, DRY etc
- Design Patterns: 23 (Gang of Four)
- Machine Coding Round

## Software Development Life Cycle/SDLC

1. Requirements
2. Solution/Design
3. HLD Level Design - Architect
4. LLD: Classes, Interfaces, Abstraction, Service Layer, Controller etc - CODE
5. Unit Testing/Integration Testing
7. Release

---

# High Level Design

## Key Characteristics of a Distributed System:

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

## (1) Scalability

### Definition

Scalability is the capability of a system, process or a network - to grow and manage the increasing demands

### Why to Scale

#### Increased Demands

- Increased Users
- Increased Requests
- Increased Dataload

### Ways to Scale

#### (A) Horizontal Scaling

- Scale by adding more servers into pool of resources
- Easier to scale dynamically by adding more machines, no downtime
- Costly

Existing: S1 S2

After Load Increased:

New: S1 S2 S3 S4 S5

Eg:

Requests will be Distributed:

10k Requests (Before):
5K- S1, 5K- S2

10k Requests (After):
2K For Each Server

#### (B) Vertical Scaling

- Scale by adding more computational power (CPU, Storage, RAM) to an Existing Server.
- Limited to capacity of a single server and involves downtime
- Cheaper

"Scheduled Maintainance: Website will be down at 1 AM - 3 AM on 29th Sept 2024"

Eg:

Horizontal Scaling:
- Apache Cassandra or MongoDB

Vertical Scaling:
- MySQL, Sqlite

#### (C) Auto-Scaling (internally using Horizontal Scaling)

Upscaling + Downscaling

- Configuration
- Memory/CPU/RAM Usage: Crossed 80% for current Instance -----> Get Another Instance
- Memory/CPU/RAM Usage: Reaches less than 10% for current Instance -----> Turn Off that Instance

Pagerduty/Pager: On Call

Error rate: API ---> 10% Spike

Monitoring Tools - Eg: Kibana, Grafana, Dynatrace etc

Config:

On Prem (On Premises):
- config.yaml

Cloud:
- AWS: SQS (Trigger Event) etc
- AWS: App Config

### On-Premises Server

- (Servers are owned by own premises of the company)

Govt Office:
- "Server Rooms"

On Premises:
- Military Stations
- Electric Supply
- Banks

Cost:
- Cleaning
- Maintainance Servers

### Cloud Computing

- Physical Server ------> Virtual Address Administration. Means, physical servers are provided to you virtually.

Cloud Providers - AWS, GCP, Azure

Data Centres:

- Netherlands
- Mumbai, India
- US (us-east-1, us-east-2)

Managing: Subscription

- IAAS
- PAAS
- SAAS

Hybrid Cloud: Different services with different cloud providers

Service-1: AWS
Service-2: Oracle

## (2) Availability

- Based on how many requests are coming & how many requests are failing to serve.

### Definition

- Availability is the time a system remains operational to its requests

### Benchmark

Usually - measured in terms of 9
Ideally - 9 9s or 11 9s

90% up & running, you can call it as ➜ 1 9s. Similarly below is 2 9s, 3 9s etc.
- 99% ➜ 2 9s
- 99.99999% ➜ 7 9s
- 99.9999999% ➜ 9 9s

................... Closer to 100% Available

### Real Life Example

#### (A) WhatsApp

- Single Tick: Send from your End
- Double Tick: Received by Receiver
- Blue Tick: Read (Read Receipts On)

Availability Example:
 - Send a Message - Single Tick

#### (B) Opened any app: Works as expected

- Eg: Zomato - Homepage Service: Available

## (3) Consistency

### definition

When there is an update in the system, all nodes are updated with latest information (Consistency)

#### Strong Consistency

- All Nodes would be updated at the SAME TIME, even if there has to be non-availability,
- No Stale Data Possible

#### Eventual Consistency

- All Nodes will EVENTUALLY be Consistent
- Can have Stale Data for very short time
- System should be Available 24/7

### Real Life Example

#### (A) Strong Consistency:

Food Delivery Service: Zomato/Swiggy/Uber Eats

Homepage-Service:

Strong Consistency:

- There would be slight delay when app opens
- No Stale Data: Exactly updated with latest information for all the Restaurants - which are open, which are not.

Eventual Consistency:

- Homepage will immediately load without any delay/pause (Available)
- Stale data: Restaurants - which are open, which are not - Not immediately updated

Zomato:

- Shows some food information/vlogs
- In meantime, fetched the latest data for Restaurants etc

#### (B) Strong Consistency:

Cab Service: Uber, Grab, Ola, Lyft etc.

Ride Finding Service:

- Search for nearby drivers
- Latest Data ONLY
- Some delay is fine

Ans: Strong Consistency

#### (C) Eventual Consistency:

Eg: Social Media Application

Instagram:

- Homepage Service/News Feed: Latest data vs Availability?

  - Priority: Instagram app should be UP and Available for All users
  - Availability > Consistency

- Eventually, you will see the updated posts
- Stale Data/Old Posts: Okay

---

Next Topic ⟶

## CAP Theorem

- Consistency
- Availability
- Partition Tolerance: Distributed Systems (Fault Tolerance)

Out of 3, Any Distributed System can have only 2

- CP or AP

### Partition Tolerance

- No Single Point of Failure
- Given by Distributed System

### CP Systems

### AP Systems