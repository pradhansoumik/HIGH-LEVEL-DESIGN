# CAP Theorem

- Consistency
- Availability
- Partition Tolerance: Distributed Systems (Fault Tolerance)

Out of 3, Any Distributed System can have only 2

- CP or AP

### Partition Tolerance

- No Single Point of Failure
- Given by Distributed System

### CP or, AP Systems

Talks about limitations on Distributed Databases.

- Consistency
- Availability
- Partition Tolerance: Distributed Systems (Fault Tolerance)

## Partition / Fault Tolerance

- No Single Point of Failure
- Given by Distributed System

## CAP Theorem

For any Database in a Distributed System, Out of 3, It can have only 2

- CP or AP

---

## CP Systems

- Consistency > Availability
- Strong Consistency
- My System goes down for sometime (non-available), I am fine with it
- No Stale Data

### Real Life Example

- Google Spanner
- Google BigTable

### Why is it a CP System?

- Strong Consistency because of synchronised clocks,
- and distributed across geo-locations
- Hence, its a CP System

### Examples

- Food Delivery Service
- Ride Finding Service

### Financial Systems Example

Financial (Banking/Insurance/Trading) Systems ----- Transactions

### ATM Example

Transaction: 100 USD

(from) 1000 USD ----> 900 USD (now)

- All ATM must show the updated balance
- If in meantime, Bank App/ATM is not up for that user, its fine
- No Stale Data to be Shown

This is the classic CP behavior: the system prefers correctness over time.

---

## AP Systems

- Availability > Consistency
- Eventual Consistency
- My System MUST NOT Go Down
- Stale Data

### Real Life Example

Amazon Dynamo DB: AP System

- Provides you High Availability (Distributed Across AWS Regions) and Partition Tolerance
- AZ (Availability Zones) - Replicated Data Stores for Dynamo DB to ensure data durability and Availability
- Not Strong Consistent by default
- AP System

### Examples

#### Social Media

- Homepage Service/News Feed: Always Available
- Priority: App should be UP and Available for All Users
- Eventually (TTL: 200-500 ms), you will see the latest posts
- Stale Data/Old Posts: Okay

This is the classic AP behavior: the system prioritizes availability and keeps serving users even if the latest data is slightly delayed.

---

## Quick Summary

- CP = Consistency + Partition Tolerance
- AP = Availability + Partition Tolerance
- Any distributed system has to sacrifice one of the three properties under network partition
- In simple words:
  - CP gives correct data, sometimes with downtime
  - AP gives uptime, sometimes with stale data

## Final Takeaway

CAP theorem explains that in distributed systems, you cannot have all three of Consistency, Availability, and Partition Tolerance at the same time.

So in practice:

- If correctness matters more, choose CP
- If uptime matters more, choose AP
