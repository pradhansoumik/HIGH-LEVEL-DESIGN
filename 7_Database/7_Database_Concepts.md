# Database Concepts

There are mainly two types of databases:

- SQL (Relational DB)
- NoSQL (Non-Relational DB)

---

## 1) Relational DB

Relational databases are structured and have predefined schemas.

Characteristics:

- Data is stored in tables with rows and columns
- Every row follows the same structure
- Best suited for structured and consistent data

Examples:

- MySQL
- Oracle DB
- MS SQL Server
- Postgres
- SQLite
- MariaDB

### Example: Bank Transactions

| Sender_ID | Receiver_ID | UTR | Timestamp | Amount |
|---|---|---|---|---|
| 123 | 456 | A112 | 233939304 | 100 |

All transactions follow the same structure, so this is a good fit for a SQL database.

---

## 2) NoSQL DB

NoSQL databases are used for unstructured or semi-structured data. They do not require a fixed schema.

Characteristics:

- Flexible schema
- Better for varied and dynamic data types
- Common in modern distributed and large-scale systems

### Example: Multiple Response Types

```json
req-1: {
  "resp": "..."
}

req-2: {
  "resp-1": "...",
  "resp-2": "..."
}
```

These responses can have different structures, so this is a suitable use case for NoSQL.

---

## Types of NoSQL Databases

### A. Key-Value Store

Data is stored as a set of key-value pairs.

```json
{
  "req": "resp",
  "req2": ["resp1", "resp2"]
}
```

Examples:

- MongoDB
- Redis
- Voldemort
- Couchbase
- DynamoDB

### B. Document Database

- Data is stored in documents
- Documents are grouped into collections

Examples:

- MongoDB
- CouchDB

MongoDB Atlas concepts:

- Collections
- Indexed collections

### C. Wide-Column Database

- Instead of tables, columns act as containers for rows
- Best used for large data sizes

Examples:

- Cassandra
- HBase

### D. Graph Database

- Relationships are represented as graphs
- Data is stored in nodes and edges

Examples:

- Neo4j
- InfiniteGraph

> Reference: Database cheat sheet image shared earlier on WhatsApp / drive.

---

## Scaling

### Horizontal Scaling

- Add more machines or partitions
- Spread data and load across multiple servers
- Common in sharding or partitioning

### Vertical Scaling

- Add more power to the same server or database instance
- Usually done through indexing and bigger machines

---

## Geospatial Data

Use cases:

- Uber
- Doordash
- Zomato
- Finding the nearest restaurant

Concepts:

- Static data: restaurants
- Dynamic data: live rider location
- Data structure: QuadTree
- GeoSpatial DB: stores QuadTree data

---

## CAP Theorem

CAP theorem states that it is impossible for a distributed software system to simultaneously provide more than two out of the following three guarantees:

- Consistency (C)
- Availability (A)
- Partition Tolerance (P)

### Consistency

- All nodes or instances see the same data at the same time
- Strong consistency means updates are propagated before reads are allowed

### Availability

- Every request gets a response, whether success or failure
- The system keeps running and avoids 5xx failures
- Usually measured in uptime, such as 99.9999999% availability

### Partition Tolerance

- The system continues to work despite network failures or partial system outages
- A partition-tolerant system continues to operate even if some nodes cannot communicate
- Data is replicated to keep the system alive during intermittent failures

A distributed system is generally considered partition tolerant by default.

### Examples

#### Amazon

Cart -> Buy -> Address -> Payment

If the payment service is not working, the add-to-cart functionality can still continue working. The entire system does not fail because one component is down.

#### LinkedIn

- Updating profile
- Writing a post
- Likes and comments

If profile updates are not working, writing a post can still function.

### Database Partitioning and Replicas

- Database Partitioning (DBP)
- Replicas (R)

Example:

- DBP-1: Replicas DBP-1_R1, DBP-1_R2
- DBP-2: Replicas DBP-2_R1, DBP-2_R2

---

## Reasoning Behind CAP Theorem

### Scenario 1: If we choose Consistency

All nodes are updated first before returning a response.

This means the nodes are temporarily unavailable while data is being synchronized.

Result: strong consistency, but reduced availability.

### Scenario 2: If we choose Availability

All nodes may not have the latest data.

This means stale data may be returned to some clients.

Result: high availability, but reduced consistency.

So in practice, distributed systems must choose two of the three constraints depending on the business need.

> Reference research papers: Google Spanner, Google Borg / Kubernetes

---

## Data Partitioning

### What is it?

Data partitioning is a technique of breaking a large database into smaller parts and distributing them across multiple machines or servers.

### Why do we need it?

After a certain scale, horizontal scaling becomes more feasible than keeping everything in a single database.

Example:

- Server-1: 100 tables
- Join is easier

Then after split:

- Server-1: 50 tables
- Server-2: 50 tables

Join becomes more difficult across shards.

## Partitioning Methods

### A. Horizontal Partitioning / Data Sharding

Split different rows or tables across multiple servers.

Example:

- Data of restaurants across the world
- Partition based on ZIP code

```text
ZIP Code 0-9999 => Server-1
ZIP Code 10000-19999 => Server-2
```

This is called sharding by a partition key.

#### Disadvantage

Uneven distribution of load can happen if partition logic is poor.

Example:

- Server-1: 100 rows
- Server-2: 10,000 rows

Queries against Server-2 may become slower than Server-1.

### B. Vertical Partitioning

Split database tables by feature or use case into separate servers.

Example for Instagram:

- User data: DB-1
- Uploaded photos: DB-2
- Followers and following: DB-3

This design separates concerns and allows each service or database to be optimized independently.

#### Disadvantage

As the user base grows, further partitioning may become necessary.

Example:

- 1 billion users
- 100 billion photos

This cannot be kept in a single DB instance efficiently.

---

## Indexing

DB indexing allows us to reduce the number of records or rows that need to be searched when a query is executed.

In other words, indexing optimizes lookup time in a database table by reducing the number of operations needed to fetch a particular value.

> Reducing the number of operations it takes to get a particular value.

### Example

Table:

| ID | Name | Email | Phone | Address |
|---|---|---|---|---|
| 1 | ... | ... | ... | ... |

- Total records: 100 million

Query:

```sql
SELECT *
FROM Table
WHERE Name = 'Dev';
```

Behind the scenes:

- Every single row is checked to see whether `Name == 'Dev'`
- This means a full table scan is performed
- Full table scan: 100 million records

> An index is a data structure that stores values for a specific column or columns of a table and helps us avoid a full table scan.

### Indexing in a Book

#### 1. Original book

- Pages: 1000
- No chapters
- Full table scan: 1000 pages
- Lookup size: 1000 pages

#### 2. Indexing on chapters (Primary Indexing)

- Pages: 1000
- Chapter 1: Page 1-50
- Chapter 2: Page 51-100

If we need Chapter 2, the sample size becomes only 50 pages instead of 1000.

Lookup size: 50 pages

#### 3. Indexing on sub-chapters (Secondary Indexing)

- Chapter 1: Page 1-50
  - SubChapter 1.1: Page 1-10
  - SubChapter 1.2: Page 11-20
- Chapter 2: Page 51-100
  - SubChapter 2.1: Page 51-60
  - SubChapter 2.2: Page 61-70

Lookup size: 10 pages

The goal of creating an index is:

> To make it faster to search through the table and find the row(s) that we want.

---

## Create an Index

### 1) Index on a single column

```sql
CREATE INDEX index_name ON TABLE (COL_NAME)
```

### 2) Index on multiple columns

```sql
CREATE INDEX index_name ON TABLE (COL_NAME1, COL_NAME2....)
```

Note:

- Indexes should be created on columns that are frequently fetched
- Indexing has disadvantages as well

---

## How Indexing Works Internally

Table: BOOKS

| Writer | Title | Date |
|---|---|---|
| JK Rowling | Harry Potter | 2007 |
| JK Rowling | Harry Potter | 2008 |
| JK Rowling | Harry Potter | 2009 |
| JK Rowling | Harry Potter | 2010 |
| JK Rowling | Harry Potter | 2007 |
| GRR Martin | Game of Thrones | 2010 |
| GRR Martin | Game of Thrones | 2010 |
| GRR Martin | Game of Thrones | 2010 |
| GRR Martin | Game of Thrones | 2010 |
| GRR Martin | Game of Thrones | 2010 |
| JK Rowling | Harry Potter | 2012 |

Query:

```sql
SELECT *
FROM BOOKS
WHERE Writer = 'JK Rowling';
```

Lookup size: 11 records

- Full table scan

After indexing on the `Writer` column:

```sql
CREATE INDEX writer_index ON BOOKS (Writer);
```

The internal index structure looks like this:

| Search Key | Pointer |
|---|---|
| JK Rowling | [1, 2, 3, 4, 5, 11] |
| GRR Martin | [6, 7, 8, 9, 10] |

Lookup size: 6

---

## How Indexes Decrease Write Performance

An index can speed up data retrieval (read performance), but the index itself may become very large, which slows down write operations such as insertion, updation, and deletion.

### Example

When adding rows or updating records, we do not only update the database; we also update the index.

For every new data entry, index pointers need to be re-evaluated, which means re-indexing.

| Search Key | Pointer |
|---|---|
| JK Rowling | [1, 2, 3, 4, 5, 11, 12, 13] |
| GRR Martin | [6, 7, 8, 9, 10] |

This decreases write performance.

Impact on write operations:

- Insertion
- Updation
- Deletion

Therefore:

- Adding unnecessary indexes on tables should be avoided

Use cases:

- For write-heavy applications: avoid heavy indexing
- For read-heavy applications: indexing is beneficial

---

## Example: Updating Index After New Record

Table: BOOKS

| Writer | Title | Date |
|---|---|---|
| JK Rowling | Harry Potter | 2007 |
| JK Rowling | Harry Potter | 2008 |
| JK Rowling | Harry Potter | 2009 |
| JK Rowling | Harry Potter | 2010 |
| JK Rowling | Harry Potter | 2007 |
| GRR Martin | Game of Thrones | 2010 |
| GRR Martin | Game of Thrones | 2010 |
| GRR Martin | Game of Thrones | 2010 |
| GRR Martin | Game of Thrones | 2010 |
| GRR Martin | Game of Thrones | 2010 |
| JK Rowling | Harry Potter | 2012 |
| JK Rowling | Harry Potter | 2015 |

Indexing on column: Writer

| Search Key | Pointer |
|---|---|
| JK Rowling | [1, 2, 3, 4, 5, 11, 12] |
| GRR Martin | [6, 7, 8, 9, 10] |

This shows that when a new row is inserted, the index must also be updated.

---

## Data Structures Used for Indexing

Different databases use different data structures to implement indexing.

### 1) B+ Trees (SQL-based)

- Efficient for quick lookup, deletion, and insertion
- Data stored is sorted
- All operations: O(log N)

### 2) Hash Tables

Indices that use hash tables are called hash indexes.

In a hash index:

- Key: column value
- Value: pointer to the rows

Characteristics:

- Best-case complexity: O(1)
- Worst-case complexity: O(log N)
- Data is not sorted, so it may not be efficient for sorted queries

Example:

- Results of a contest
- Rank is sorted in order

### 3) R-Tree

- Used in spatial databases
- Useful for storing locations such as longitude and latitude

Query example:

> "Cafes near me"

This means finding cafes within 5 miles.

### 4) Bitmap Index

Works for columns that have boolean values.

Example:

| Account_id | Is_Active |
|---|---|
| 123 | true |
| 456 | false |

This is useful when the column has a limited number of distinct values such as true/false.

---

## Final Note

Indexing is extremely useful for read-heavy systems because it reduces lookup time and avoids full table scans. However, it comes with a cost: write performance can degrade because the index also needs to be maintained on inserts, updates, and deletes.

That is why indexes must be chosen carefully based on the workload and access pattern of the system.
