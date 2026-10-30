# Database Concepts

There are mainly two Types of Databases:

- SQL (Relational DB)
- NoSQL (Non-Relational DB)

## Relational DB

- Structural, have pre-defined schemas
- Tabular form, particular no. of columns.

## Non-Relational DB

- unstructural
- No pre-defined schemas

### Examples

#### 1) Bank Transaction

| Sender_ID | Receiver_ID | UTR | Timestamp | Amount |
|---|---|---|---|---|
| 123 | 456 | A112 | 233939304 | 100 |

All Transactions have EXACTLY Same Structure: SQL DB

#### 2) Multiple Response Type

```json
req-1: {
  "resp": "..."
}

req-2: {
  "resp-1": "...",
  "resp-2": "..."
}
```

All Responses have Different Structure (un-structured responses): NoSQL DB

---

## SQL: to store the structured data

- Relational DB stores data in rows and columns
- Rows: Records, Columns: Fields
- Every Row has the same structure

Examples:

- MySQL
- OracleDB
- MS SQL Server
- Postgres
- SQLite
- Maria DB

---

## NoSQL: to store the un-structured data

### 1) Key-Value Store

Data is stored in an array of key-value pairs.

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

### 2) Document Database

- Data is stored in documents
- Documents are collected together as collections

Examples:

- MongoDB
- CouchDB

MongoDB Atlas:

- Collections
- Indexed Collections

### 3) Wide-Column Database

- Instead of tables, use columns as containers for rows
- Best Used for LARGE DATA SIZE

Examples:

- Cassandra
- HBase

| Transaction ID: 122 | Sender_ID: 2333 | Receiver_ID: 323344 |

### 4) Graph Database

- Databases where Relationships are the best represented in graphs (Vertices and Edges)
- Data is saved in graph structures with nodes and vertices

Examples:

- Neo4J
- InfiniteGraph

> Refer: Database CheatSheet Image (Sent in WhatsApp - uploaded in our drive)

---

## Horizontal Scaling

- Add more tables/resources
- Partitioning/Sharding across different Servers

## Vertical Scaling

- Add more data into same tables/resources
- Indexing

---

## GeoSpatial

Uber, Doordash, Zomato etc

Use Case:

- Need to find nearest restaurants

Static: Restaurants
Dynamic: Live Location of Zomato Rider

Data Structure: QuadTree
GeoSpatial DB: Store QuadTree

---

## CAP Theorem

CAP Theorem States That:

It is impossible for a "Distributed Software System" to simultaneously provide more than two out of three of the following scenarios:

CAP:

- Consistency (C)
- Availability (A)
- Partition Tolerance (P)

### Consistency

- All Nodes/Instances see the exact same data at the same time
- Consistency (Strong) is achieved by updating all nodes before allowing further reads

### Availability

- Every request gets a response: success/non-success
- Availability is achieved by having your system running up 24*7
- No 5xx Error
- 9 Times: 99.9999999

### Partition Tolerance

- System continues to work despite data loss or partial failures
- A System that is "Partition Tolerant" can sustain any amount of network failure that doesn't result in failure of Entire Network
- Data is sufficiently replicated across combination of Nodes to keep the system up from intermittent changes

Distributed Software System -> Partition Tolerant (By Default)

### Example: Amazon

Cart -> Buy -> Address -> Payment

If Payment Service is not working, (one component is not working), "Adding to Cart" Functionality is still working (another component is still working)

### Example: LinkedIn

- Updating DP, Writing Post, Likes, Comments
- If Updating DP is not working, Writing Post is still working

### DB

- Database Partitioning (DBP) & Replicas (R)

  - DBP-1:
    - Replicas: DBP-1_R1, DBP-1_R2

  - DBP-2:
    - Replicas: DBP-2_R1, DBP-2_R2

---

## Reasoning of CAP Theorem

### Consistency

- All Nodes should see the same set of updates in same order: Strong Consistency

### 2 Scenarios

#### 1) If we have Consistency

Means, all Nodes will be updated first before giving back response.
That means, my nodes will NOT be Available for that time being.

-- 2/3 [2 out of 3]

#### 2) If we have Availability

Means All Nodes will Not have Latest Data.
This will serve Stale Data.

That means, it will NOT be Consistent.

-- 2/3 [2 out of 3]

> Refer - Research Paper:
> - Google Spanner
> - Google Borg -> Kubernetes

---

## Data Partitioning

### What?

- Technique to break a big database (DB) into smaller parts
- Process of splitting up a DB across Multiple Machines/Servers

### Why?

After a certain scale, it is more feasible to scale horizontally.

Server-1: 100 Tables

- JOIN: Easy

Server-1: 50 tables

AND,

Server-2: 50 tables

- JOIN: Difficult

---

## Partitioning Methods

### A) Horizontal Partitioning / Data Sharding

- Put Different Tables / Rows into Different Servers

Example:

- We have a data of all the restaurants in the world
- Locations are based upon ZIP Codes in DB

*** Horizontal Partitioning: ***

Storing locations in DB as per ZIP Codes

| S No | Name | Contact | Zip Code |
|---|---|---|---|
| 1 | Mn Rows | ... | ... |

- ZIP Code is used as a Partition Key

ZIP Code > 0 and < 10000: Server-1
ZIP Code > 10000 and < 20000: Server-2

DB-1: Instance: ZIP Code > 0 and < 10000
DB-2: Instance: ZIP Code > 10000 and < 20000

#### Disadvantages

`Uneven Distributed Load`

- If Wrong Partition Logic is Used, it will lead to Un-Evenly Distributed Data

It might contain 100 Rows & 10k Rows:

- ZIP Code > 0 and < 10000: Server-1 -> 100 Rows
- ZIP Code > 10000 and < 20000: Server-2 -> (1 Mn - 10k) Rows

*** Query on DB-2 will be slower than Query on DB-1 ***

---

### B) Vertical Partitioning

- Divide our Database/Tables related to a specific feature in their own server.

Example:

Building Instagram:

- User Data: DB-1
- Photos they Upload: DB-2
- Followers and Following: DB-3

#### User Data: DB-1

> Probably a Relational DB

Schema:

| User_UUID | Name | Email | Phone |
|---|---|---|---|
| ... | ... | ... | ... |

#### Photos They Upload: DB-2

> Probably a No-SQL DB

Example: S3

#### Followers and Following: DB-3

User-1 -> PK
User-2

Defined Schema/Structure

- Relational DB can be choice
- Might become difficult for celebrity users (> 10 M Followers)

> Probably a Graph DB would be better.

#### Disadvantages

For Additional Application Users, it might require Further Partitioning

Example:

1 Bn Users -> 100 Bn Pics

Cannot be stored in 1 DB Instance

-> Divide across multiple DB Instances (Horizontal Partitioning)

---

## C) Directory Based Partitioning

- Loosely coupled approach to solve the issues from both Horizontal and Vertical Partitioning
- Create a lookup service which knows your current Partitioning and provides much easier access
- To Find a Particular Data Entry -> Directory Service -> (Req: Resp[DB-Partition])

Key: tuple
Value: DB Server

Example:

In Vertical Partitioning,

- If data for UUID_123 is stored in DB-189
- First, Directly lookup in Directory Service -> Go to DB-189

In Horizontal Partitioning,

- If data for ZIP 123456 in DB-289
- First, Directly lookup in Directory Service -> Go to DB-289

Hashmap:

- ZIP123456 -> DB-289: Horizontal
- ZIP123457 -> DB-290: Horizontal
- UUID_123 -> DB-189: Vertical
- UUID_457 -> DB-190: Vertical

### Disadvantages

1. For Any Update in Entries (Addition/Deletion) in DB, Directory Service needs to be updated
2. Huge Size of Multiple Key-Value Pairs - Might Need to Further Partition

---

## Partitioning Criteria / Implementation

> Which column should be used to determine the partition. It will be applicable to both horizontal and vertical partitioning.

### 1) Key Based / Hash Based Partitioning

- Apply a Hash Function to some key attributes (columns) to give a Partition Number
- Store data in corresponding database with Partition Number

Example:

1 Huge DB -> 100 DB Partitions (based on load - how much data I have, we can determine the partitions)

S_No: (Numeric Value)

Everytime a row is inserted, S_No is incremented by 1

"Hash Function: S_No % Size_of_Partitions"

>>> Hash Function: S_No % 100

115th Record -> 115 % 100 = 15th DB Partition
1085th Record -> 1085 % 100 = 85th DB Partition

#### Advantages

"Hash Function: S_No % Size_of_Partitions"

- Evenly Distributed Data

#### Disadvantages

Before: Size_of_Partitions = 100
After: Size_of_Partitions = 200 or Size_of_Partitions = 50

Add New Partition / Delete Partition -> Change the Hash Function -> Re-Hashing

#### Solution

>>> Consistent Hashing

---

### 2) List Based Partitioning

- Each DB Partition is assigned a list of values
- Whenever you want to insert a new record, we will see which partition contains our key and then store in that partition

Example:

Store Data of All Countries

Multiple Ways to do it:

1. Based Upon Countries - Total: 195 Partitions
   - Key: Country_ID (Key Based Partitioning)
2. Based Upon Regions:
   - Key: [List]

List Partition: DB-1: Shenghen Countries - [France, Germany, Italy, Switzerland]
List Partition: DB-2: Asian Countries - [India, China, Sri Lanka]

Record:

- 1 India GDP -> DB-2
- 2 Italy GDP -> DB-1

---

### 3) Round-Robin Partitioning

Req 1 -> DB Partition - 1
Req 2 -> DB Partition - 2

- Uniform Data Distribution across Instances
- With N Partitions, ith row (row counter) is assigned to (i%n) partition
- Req_ID % n

---

### 4) Composite/Hybrid Partitioning

- Combine Any of the Above Partitioning Criterias

Example:

Problem:

- If you need to store country wise data and then city wise data

Solution:

For Country Wise Queries:

- First Apply List Based Partitioning

For City Wise Queries:

- Apply Hash Based Partitioning

---

## Problems of Data Partitioning

- Operations (Queries) across Multiple Tables or Multiple Servers Across Different Servers/DB

### JOIN

Table-1:

- User_Information

| User_UUID | Name | Email | Address |
|---|---|---|---|
| 123 | DEV | dev@gmail.com | Menlo Park |

Table-2:

- User_Data

| User_UUID | Pic_ID | Likes | Comments |
|---|---|---|---|
| 123 | abc | 2000 | 1000 |

>>> Common Key: User_UUID -> JOIN is Possible

Use Case:

Tell me the name, pic, likes, comments of a Particular User

Answer: JOIN

Query:

```sql
SELECT User_UUID, Name, Email, Address, Pic_ID, Likes, Comments
FROM User_Information
JOIN User_Data
WHERE User_Information.User_UUID = User_Data.User_UUID;
```

Output:

| User_UUID | Name | Email | Address | Pic_ID | Likes | Comments |
|---|---|---|---|---|---|---|
| 123 | DEV | dev@gmail.com | Menlo Park | abc | 2000 | 1000 |

#### A) JOINS

- Performing joins across multiple tables in 1 machine: Easier
- Performing joins across multiple tables in different machine: Difficult
- Such joins will NOT be Performance Efficient: Why?
  - Data needs to be fetched across multiple machines/servers: Slow

Solution:

- Indexing
- Choosing the correct partitioning key

#### B) Referential Integrity

- Performing a cross-partitioning query on a partitioned database is NOT Easy
- Includes enforcing Integrity Constraints as Foreign Keys in partitioned database can be difficult

Integrity Constraints:

1. Foreign Key MUST be present
2. Should match the Type Across All Tables - (Int-Int, Long-Long, String-String)

Problems:

- Leads to Dangling Pointers (it was referring but there is no data now / was referring to a data which was present but not present anymore.)

Solution:

- Use Batch/Cron Jobs to clean up dangling references

Example:

Table-1: Partition-1

User_Information:

| User_UUID | Name | Email | Address |
|---|---|---|---|
| 123 | DEV | dev@gmail.com | Menlo Park |

PK: User_UUID

Table-2: Partition-2

User_Data:

| User_UUID | Pic_ID | Likes | Comments |
|---|---|---|---|
| 123 | abc | 2000 | 1000 |

PK: Pic_ID
FK: User_UUID

Primary Key:

- Should not be NULL
- Should not have duplicates
- Can be Composite as Well

Purpose:

- Uniquely Identify a Row in a Table

Foreign Key:

- Not a Primary Key for current table
- But is a Primary Key for some other table

#### C) Rebalancing

- There can be many reasons to rebalance our partitioned schema

Original: 1 DB

- Partition: 100 Partitions

Change:

- Partition: 100 -> 200
- Partition: 100 -> 50

*** Data Load determines the partition.***

---

## Indexing

DB Indexing allows us to cut down the number of records/rows that needs to be searched/queried when a query is executed.

OR,

Indexing in a DB just optimises my lookup time in a DB/Table.

*** Reducing the number of operations it takes to get a particular value. ***

### Example

Table:

| ID | Name | Email | Phone | Address |
|---|---|---|---|---|
| ... | ... | ... | ... | ... |

- 100 Mn Records

Query:

```sql
SELECT *
FROM Table
WHERE Name = 'Dev';
```

Behind the Scenes:

Every Single Row/Record is checked to see if Name == 'Dev'

- Whole Table is scanned (Sequential Search) -> Full Table Scan: 100 Mn Records

*** "An Index is a Data Structure that stores the values for a certain specific column/columns of a table and helps us to avoid a full table scan" ***

### Example: Indexing in a Book

#### 1) Original

- Pages: 1000 (Records)
- There are No Chapters
- Full Table Scan: 1000 Pages
- Sample Size: 1000
- Lookup Size: 1000 Pages

#### 2) Indexing on Chapters - [PRIMARY INDEXING]

- Pages: 1000 (Records)
- Chap-1: Page: 1 - 50
- Chap-2: Page: 51 - 100

You need to lookup Chap-2.
Sample Size Becomes: 51-100 Pages: 50 Pages

Lookup Size: 50 Pages

#### 3) Indexing on Sub-Chapters - [SECONDARY INDEXING]

- Chap-1: Page: 1 - 50
  - SubChapter-1.1: Page: 1 - 10
  - SubChapter-1.2: Page: 11 - 20
- Chap-2: Page: 51 - 100
  - SubChapter-2.1: Page: 51 - 60
  - SubChapter-2.2: Page: 61 - 70

Lookup Size: 10 Pages
Sample Size Becomes: 1-10 Pages: 10 Pages

The Goal of Creating an Index is that:

> "TO MAKE IT FASTER TO SEARCH THROUGH THE TABLE AND FIND THE ROW(S) THAT WE WANT"

---

## Create an Index

### 1) On 1 single Column

```sql
CREATE INDEX index_name ON TABLE (COL_NAME)
```

### 2) On Multiple Columns

```sql
CREATE INDEX index_name ON TABLE (COL_NAME1, COL_NAME2....)
```

Note:

Index should be created on a table if the data in the indexed column(s) is frequently fetched, because Indexing has disadvantages as well.

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

Lookup Size: 11 [total 11 records in the above table]

- Full Table Scan

Indexing:

On Column - Writer

```sql
CREATE INDEX writer_index ON BOOKS (Writer);
```

Index: writer_index

| Search Key | Pointer |
|---|---|
| JK Rowling | [1,2,3,4,5,11] |
| GRR Martin | [6,7,8,9,10] |

Lookup Size: 6

---

## How do Indexes Decrease Write Performance?

(1) An Index Can be used to speed up data Retreival (Read Performance),
BUT may itself be VERY LARGE to too Large Size, which will slow down the Write Operations - Insertion, Updation and Deletion

Data Insertion: POST
Updation: PUT

While Adding rows or making updates to existing rows,
We NOT ONLY have to update database, but also have to update the index.

Like, for new data entry index pointer needs to be re-evaluated -> re-indexing

| Search Key | Pointer |
|---|---|
| JK Rowling | [1,2,3,4,5,11,12,13] |
| GRR Martin | [6,7,8,9,10] |

-> This DECREASES the WRITE Performance <-

This Performance Degradation applied to ALL WRITE Operations:

- Insertion
- Updation
- Deletion

For this reason,

Adding Unnecessary indexes on tables should be avoided.

Case Not to Use Indexed Heavily?

- For Write Heavy Applications

Case to Use Indexed Heavily?

- For Read Heavy Applications

---

## Example: add a new row

Task:

- Add 1 more row to the table, Need to Update in the Index as well

```sql
INSERT INTO BOOKS VALUES (,,,,,,);
UPDATE writer_index ADD VALUES(,,,,,,,)
```

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
| JK Rowling | Harry Potter | 2015 -> NEW RECORD |

Indexing:

On Column -> Writer

| Search Key | Pointer |
|---|---|
| JK Rowling | [1,2,3,4,5,11,12] |
| GRR Martin | [6,7,8,9,10] |

---

## Data Structures Used for Indexing

Different Databases have different data structures to implement indexing.

### 1) B+ Trees: SQL Based

- Efficient for quick lookup, deletion and insertion
- Data Stored: Sorted
- All Operation: O(log N)

### 2) Hash Tables

Indices which use Hash Tables are called Hash Index.

In Hash Index (in above example):

- Key: Column Value
- Value: Pointer to the Rows

- Best Case Complexity: O(1)
- Worst Case Complexity: O(log N)
- Not Sorted Data, might not be Efficient for certain type of queries (for sorted data)

Example:

Result of Contest:

- Rank - Sorted Order

### 3) R-Tree

- Spatial Databases (to store the locations)
- Longitudes and Latitudes

Queries Like:

> "Cafes Near Me"

-> Find Cafe within 5 Miles

### 4) Bitmap Index

Works for Columns that have boolean values.

Example: Column having boolean values

True/False

| Account_id | Is_Active |
|---|---|
| 123 | true |
| 456 | false |

---

## Final Note

The Goal of Creating an Index is that:

> TO MAKE IT FASTER TO SEARCH THROUGH THE TABLE AND FIND THE ROW(S) THAT WE WANT

This helps in reducing lookup time and avoiding full table scans, but it can also reduce write performance because indexes must be maintained during insert, update, and delete operations.
