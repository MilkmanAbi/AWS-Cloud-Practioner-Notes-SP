---
tags: [aws, cloud-foundations, clf-c02, module-8, exam, databases]
module: 8
title: Databases
exam: AWS Certified Cloud Practitioner (CLF-C02)
domain: Cloud Technology and Services (34%)
---

# Module 8 - Databases

> [!info] Exam context
> Maps to **CLF-C02 Domain 3: Cloud Technology and Services (34%)**. The exam tests "given this scenario, which DB?" so memorize the **decision matrix**: relational (RDS/Aurora) vs NoSQL (DynamoDB) vs data warehouse (Redshift).

> [!abstract] TL;DR Cram
> - **Amazon RDS** = managed **relational** DB (6 engines: Aurora, MySQL, PostgreSQL, MariaDB, Oracle, SQL Server). Auto-patching, backups, Multi-AZ, read replicas.
> - **Amazon Aurora** = MySQL/PostgreSQL-compatible, **5x MySQL** performance, auto-scales to 64 TB, 15 read replicas, 3-AZ replication.
> - **Amazon DynamoDB** = **NoSQL** key-value/document, single-digit ms latency, **virtually unlimited** throughput/storage, serverless, auto-scales.
> - **Amazon Redshift** = petabyte-scale **data warehouse** (OLAP), SQL, columnar storage, BI/analytics.
> - **RDS** when: complex transactions, complex queries, medium-high I/O, high durability.
> - **DynamoDB** when: key-value/document, fast + flexible schema, massive scale, single-digit ms.
> - **Redshift** when: SQL analytics/BI over large datasets.

---

## 1. Managed vs Unmanaged Databases

> [!note] Why managed (RDS) over self-managed DB on EC2?
> | Unmanaged (DIY on EC2) | Managed (RDS) |
> |---|---|
> | You handle OS patching, DB patching, backups, HA, scaling, security | AWS handles **all of that** |
> | Full control but heavy operational burden | Focus on your app, not the infra |

---

## 2. Amazon RDS (Relational Database Service)

> [!definition] RDS
> Makes it easy to set up, operate, and scale a **relational database** in the cloud. Provides cost-efficient, resizable capacity while automating administration (patching, backups, HA).

### 6 Database Engines
**Amazon Aurora**, **MySQL**, **PostgreSQL**, **MariaDB**, **Oracle**, **Microsoft SQL Server**.

### Key RDS Features
- **DB Instance** = the basic building block (isolated DB environment with multiple user-created DBs).
- Choose **instance class** (CPU/RAM) + **storage type** (SSD/HDD).
- **Multi-AZ Deployment** = automatic standby in another AZ; synchronous replication; automatic failover if primary fails -> **high availability**.
- **Read Replicas** = asynchronous copies for **read-heavy** workloads; offload reads from the primary. (NOT the same as Multi-AZ — replicas are for performance, Multi-AZ is for HA.)
- **Automatic backups** + point-in-time recovery.
- **Automatic patching** of DB software.

> [!tip] Quiz-confirmed
> - RDS auto-patches + auto-backs up + enables point-in-time recovery? -> **TRUE**.
> - Factors when choosing a DB? -> **All of the above** (data size, data access period, query frequency, HA).
> - .NET app connecting to MySQL, wants HA + auto-backups? -> **Amazon Aurora** (MySQL-compatible, built-in HA).
> - Complex transactions? -> **RDS** (NOT DynamoDB).

### When to use RDS
- Complex **transactions** or complex **queries**.
- Medium-to-high query/write rate (up to 30,000 IOPS).
- **High durability** needed.

### When NOT to use RDS
- Massive read/write rates (150K writes/sec) -> consider DynamoDB.
- Simple GET/PUT requests -> DynamoDB.
- Need full RDBMS customization -> self-manage on EC2.

### RDS Pricing
- **Clock-hour billing** (resources incur charges when running).
- **DB purchase type:** On-Demand (hourly) or **Reserved** (1 or 3 yr, upfront discount).
- **Storage + additional backup** (GB/month).
- **Number of requests** (I/O).
- **Data transfer** (inbound free, outbound tiered).
- Costs vary by **deployment type** (Single-AZ vs Multi-AZ).

---

## 3. Amazon Aurora

> [!definition] Aurora
> A MySQL and PostgreSQL-compatible relational DB built for the cloud. Combines enterprise performance + availability with open-source simplicity + cost-effectiveness.

### Aurora highlights
- Up to **5x** the throughput of standard MySQL, **3x** of PostgreSQL.
- Distributed, **fault-tolerant, self-healing** storage that auto-scales up to **128 TB** (slides may say 64 TB - either is acceptable).
- Up to **15 low-latency read replicas** (vs 5 for standard RDS MySQL).
- Continuous backup to **S3** + point-in-time recovery.
- Replication across **3 AZs**.
- Automates provisioning, patching, backup, recovery, failure detection, repair.

> [!tip] Quiz-confirmed
> - .NET + MySQL + HA + auto-backups -> **Aurora** is the ideal choice (MySQL-compatible, built-in HA/backups, cloud-native performance).

---

## 4. Amazon DynamoDB

> [!definition] DynamoDB
> A fast, flexible, fully managed **NoSQL** database. Supports **key-value** and **document** store models. Single-digit **millisecond** latency at any scale. Serverless — no servers to manage.

### Key DynamoDB facts
- **Virtually unlimited** storage and throughput.
- Items can have **differing attributes** (flexible schema).
- **Tables** contain **items** (rows); items contain **attributes** (columns/fields).
- An **attribute** = a **fundamental data element** (quiz-confirmed).
- **Query** = find items by **partition key** + optional **sort key** filter.
- **Scan** = reads every item in the table (use to find by **non-primary-key** attribute; less efficient).
- Automatic **replication** across multiple AZs in a Region.
- Supports **global tables** for multi-Region replication.
- Works well for: mobile, web, gaming, ad-tech, IoT.

> [!tip] Quiz-confirmed
> - E-commerce needing scale to 100K+ concurrent users, session state? -> **DynamoDB** (NOT RDS, NOT Redshift).
> - Find item by non-primary-key attribute? -> **Scan** (NOT Query, NOT GetItem).
> - Query enables? -> **All of the above** (query by partition key, secondary indexes, efficient retrieval).
> - Extremely fast, fast scalability, flexible schema? -> **DynamoDB** (NOT RDS, NOT ElastiCache, NOT Redshift).
> - Attribute is? -> **A fundamental data element**.

---

## 5. Amazon Redshift

> [!definition] Redshift
> A fully managed, **petabyte-scale data warehouse** service. For **analytics / BI** (OLAP), NOT for transactional workloads (OLTP).

### Key Redshift facts
- Uses **columnar storage** + **massively parallel processing (MPP)** architecture.
- Runs standard **SQL** queries. Integrates with existing BI tools (Tableau, QuickSight).
- Automatic monitoring, patching, backup.
- Encryption built-in.
- NOT for transactional workloads — that's RDS/Aurora.

> [!tip] Quiz-confirmed
> - Analyzing data using standard SQL + existing BI tools? -> **Amazon Redshift** (NOT RDS, NOT S3, NOT DynamoDB).

---

## 6. Other Database Services (brief)

| Service | What it is |
|---|---|
| **Amazon ElastiCache** | In-memory caching (Redis or Memcached); microsecond reads |
| **Amazon DocumentDB** | MongoDB-compatible document DB |
| **Amazon Neptune** | Graph database (social networks, fraud detection) |
| **Amazon Keyspaces** | Managed Cassandra-compatible |
| **Amazon QLDB** | Immutable, cryptographically verifiable ledger |
| **Amazon MemoryDB** | Redis-compatible, durable in-memory DB |

---

## 7. Database Decision Matrix (HIGH-YIELD)

> [!important] "Which DB?" decision tree
> | Need | Service |
> |---|---|
> | Complex transactions / complex queries / joins | **RDS** or **Aurora** |
> | MySQL/PostgreSQL + cloud-native HA + max perf | **Aurora** |
> | Key-value / document / flexible schema / massive scale / single-digit ms | **DynamoDB** |
> | SQL analytics / BI / data warehousing (petabyte) | **Redshift** |
> | Session state for high-concurrency web apps | **DynamoDB** (or ElastiCache) |
> | Simple GET/PUT, no complex queries | **DynamoDB** |
> | In-memory caching layer | **ElastiCache** |

---

## Rapid-Fire Recall

> [!question] Self-test
> - Managed relational DB? -> **RDS** (6 engines)
> - MySQL-compatible, 5x perf, 15 read replicas? -> **Aurora**
> - NoSQL, single-digit ms, flexible schema? -> **DynamoDB**
> - Petabyte data warehouse, SQL + BI? -> **Redshift**
> - Complex transactions? -> **RDS/Aurora** | Simple GET/PUT? -> **DynamoDB**
> - Session state at 100K+ users? -> **DynamoDB**
> - Multi-AZ for HA in RDS? -> synchronous standby, auto-failover
> - Read replicas? -> asynchronous, read offload (performance, NOT HA)
> - Scan vs Query? -> Scan = full table read by any attribute; Query = by partition key
> - RDS auto-patches/backs up? -> **TRUE**
> - Attribute in DynamoDB? -> **Fundamental data element**

**Prev:** [[07 - Storage]] | **Next:** [[09 - Cloud Architecture]]
