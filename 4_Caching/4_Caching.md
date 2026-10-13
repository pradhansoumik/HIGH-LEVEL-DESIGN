# Caching

Real Life Example:

- Redis
- Memcache
- Hazelcaste

Load Balancing helps you scale horizontally across the increasing number of servers.

Caching helps you to make better use of resources you already have.

OR

Attain the Responses Quickly.

---

## Principle of Caching

### Locality of Reference

Recently requested data is likely to be requested again.

```text
Request ------>

Client  ------- Cache -------- Server
          <------- Response <-----
```

Browser                       Google Server

Request: www.google.com
Response: Google Home Page

Lookup in Cache: 1000x faster than lookup in Server.

### Real Life Examples

- OS:
  - iOS, Android, Mac - M1, M2, M3, Windows, Blackberry - Cache
- Application:
  - Cache
- Whatsapp:
  - Cache

> Note:
> - Cache is NOT a permanent Memory
> - Cache is a short term memory (TTL: Time to Live)

### Browser Example

- Google Chrome Browser (Memory -> Cache, Network)

---

## Cache Server

```text
Request -------> Cache --------> Application Database
```

1. www.google.com: In Cache
2. www.medium.com: Not in Cache

### 1) Cache Hit (Req-1)

- Find the response for request in the cache layer
- Quick response time
- No need to lookup in Database

### 2) Cache Miss (Req-2)

- Look in Cache ----> Not Found: Cache Miss
- Go to Database, fetch the response
- Put it in the Cache, so next time it can be directly fetched from Cache (Faster Response)

HashMap:

- req & resp

### 3) Cache Hit Rate

### 4) Cache Miss Rate

### Monitoring and Observation Tools

Metrics:

- AWS, Dynatrace, Kibana, Grafana, Datadog

### Live Example Demo

Browser:

- www.google.com: Faster
- New Website: Slower
- www.apple.com: 86 ms
- After Refresh: 46 ms

---

## Cache Invalidation / Eviction

```text
Request --------> Cache --------> Application Database
```

- Cache must be coherent/consistent with database.
- If the data is modified in the database, it should be invalidated in cache, and updated with the new values.

> Source of truth / Book of Records = Database, NOT Cache

> This is Called Cache Invalidation

### Example

Before:

- reqABC: respABC
  - database
  - cache
- Returning Response from Cache: Cache HIT
- Response: respABC - Correct

In DB:

- reqABC: respPQR
- Not updated in Cache

After, a New Request Comes:

- DB: reqABC: respPQR
- Cache: reqABC: respABC
- Returning Response from Cache: Cache HIT
- Response: respABC - Incorrect

---

## 3 Ways to Resolve Cache Invalidation

### 1) Write-Through Cache

```text
Req -------> Cache --------> DB
```

- Data is written into the cache and corresponding database at SAME TIME
- Cached data allows for fast retrieval, since the same data gets written in DB
- Ensures Data Consistency between Cache and DB
- Any Crash, System Disruptions, Cache is Always Updated
- Minimise risk of data loss because every write operation is saved/updated in cache

Disadvantages:

- Every Write is happening Twice (Cache + DB)
- Higher Latency for Write Operations

Example:

Twitter:

- R/W Ratio: 100:1 -> Read-Heavy Application
- Twitter Architecture: 3 Cluster of Redis

### 2) Write-Around Cache

- Similar to Write-Through Cache but data is directly written to DB, bypassing the Cache and later update the cache.
- Reduce the risk of cache being flooded with write Operations.
- In case of cache Miss, read from DB, read-write into cache and then return response
- Reduce Write Latency, but later cache needs to be updated

Example: Cron Jobs / Batching

Real Life Example:

Financial Institutions:

- Update Cache with DB Data: Cron Jobs (30 Mins, Hourly etc)

- Event Triggered Update:
  - Anything gets updated in DB, will raise a CRON Job to update in the Cache

```text
Req -------> DB --------> Cache
      <-------
```

### 3) Write-Back Cache

You will be writing back to the database.

- Data is written to cache alone and response is given back to the Client
- The Write to DB is done after that

Example: Cron Jobs / Batching

- Low Latency for Write-Intensive Applications
- Risk of Data Loss in case of crash because it is momentarily only in Cache. To Solve that, Use Replicas of Cache

Example: Used for Write Heavy Applications
Example: Loggers

```text
Req -------> Cache --------> DB
      <-----
```

- Event Triggered Update:
  - Anything gets updated in Cache, will raise a CRON Job to update in the DB

---

## Cache Eviction Policies

- Remove the existing values in the cache so that we can update new values in the cache.

Capacity of Cache: 5

Cache: [1 2 3 4 5]

When a new request comes, there should be space in my cache. Hence, Cache Eviction.

### 1) FIFO

- Cache Evicts the first block accessed

### 2) LIFO

- Cache evicts the last block accessed

### 3) LRU

- Cache evicts the least recently used block

### 4) LFU

- Cache evicts the least frequently used block

### 5) MRU

- Cache evicts the most recently used block

Example:

Cache Size: 5

Cache: [1 2 3 4 5]

- req = get(5) - Earliest
- req = get(4)
- req = get(3)
- req = get(2)
- req = get(1) - Latest

Since 1 is the Most recently Used:

- It will be removed from the Cache

Cache: [1 2 3 4 _]

---

## Cache Avalanche

TTL:

When a lot of data expires at the same time.

This causes a sudden increase in read calls to DB.

```text
------ effects -------> Sudden Increase in Read Calls to DB
```
