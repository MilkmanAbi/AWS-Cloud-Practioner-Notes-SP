---
tags: [aws, cloud-foundations, clf-c02, module-7, exam, storage]
module: 7
title: Storage
exam: AWS Certified Cloud Practitioner (CLF-C02)
domain: Cloud Technology and Services (34%)
---

# Module 7 - Storage

> [!info] Exam context
> Maps to **CLF-C02 Domain 3: Cloud Technology and Services (34%)**. Know the **3 storage types** (block / object / file), the **4 services** (EBS / S3 / EFS / S3 Glacier), the **S3 storage classes**, and the comparison table.

> [!abstract] TL;DR Cram
> - **Block** (EBS) = attached to EC2, persistent, replicated within AZ, snapshots to S3.
> - **Object** (S3) = buckets, 5 TB max object, **11 9s durability**, bucket names **globally unique**, default **private**.
> - **File** (EFS) = managed NFS, elastic, shared across EC2 (Linux), scales to petabytes.
> - **Archive** (S3 Glacier) = low-cost archival, vault-based, retrieval: Expedited (1-5 min) / Standard (3-5 hr) / Bulk (5-12 hr).
> - **S3 storage classes:** Standard, Intelligent-Tiering, Standard-IA, One Zone-IA, Glacier Instant, Glacier Flexible, Glacier Deep Archive.
> - **Lifecycle policies** automate transitioning objects between classes.

---

## 1. Storage Types

> [!important] Block vs Object vs File
> | Type | How data is stored | AWS service | Best for |
> |---|---|---|---|
> | **Block** | Split into fixed-size **blocks**, each with its own address | **Amazon EBS** | Boot volumes, databases, apps needing low-latency I/O |
> | **Object** | Whole file stored as an **object** (data + metadata + unique ID) | **Amazon S3** | Media, backups, static web hosting, data lakes |
> | **File** | Hierarchical file system (folders/files) over **NFS** | **Amazon EFS** | Shared storage, content management, home directories |

> [!note] Block vs Object key difference
> With **block** storage you can update a **single block** without rewriting the whole file. With **object** storage, any change replaces the **entire object**.

---

## 2. Amazon EBS (Elastic Block Store)

> [!definition] EBS
> **Block-level** storage volumes for EC2. Like a virtual hard drive. Persists **independently** from the instance (survives stop/start). Automatically replicated **within its AZ**.

### Key EBS facts
- HDD and SSD types available.
- Can be backed up to S3 via **snapshots** (point-in-time, incremental).
- Supports **encryption** (no additional cost, uses KMS).
- **Elastic** — can increase capacity and change types on the fly.
- **Cannot** be attached to instances in a different AZ (same AZ only).

### EBS use cases
Boot volumes, data storage with a file system, database hosts, enterprise applications.

### EBS volume types (high level)
| Type | Category | Use |
|---|---|---|
| **gp3 / gp2** | General Purpose **SSD** | Most workloads, boot volumes |
| **io2 / io1** | Provisioned IOPS **SSD** | I/O-intensive, databases |
| **st1** | Throughput Optimized **HDD** | Big data, data warehouses, log processing |
| **sc1** | Cold **HDD** | Infrequent access, lowest cost HDD |

> [!tip] Quiz-confirmed
> - EBS features? -> **encrypted transparently** + **replicated within an AZ** (NOT "data lost when instance stopped", NOT "backed up to tape").
> - EBS recommended when data **must be quickly accessible + requires long-term persistence** and **requires encryption**.

### EC2 Instance Store (contrast with EBS)
- Storage on disks **physically attached to the host**.
- **Ephemeral** — data is **deleted** when instance stops/terminates.
- Very high I/O; good for temp data (caches, buffers, scratch).

---

## 3. Amazon S3 (Simple Storage Service)

> [!definition] S3
> **Object** storage with virtually **unlimited** capacity. Data stored as objects in **buckets**. Designed for **11 9s of durability** (99.999999999%). A single object can be up to **5 TB**.

### Core S3 facts
- Data is redundantly stored across **multiple AZs within the Region** (except One Zone classes).
- Bucket names must be **globally unique across all AWS accounts**.
- **Default = private** — NOT publicly viewable.
- Access via Console, CLI, SDK, or REST API.
- Supports **versioning**, **lifecycle policies**, **cross-region replication**, **event notifications**.

> [!warning] Quiz-confirmed traps
> - S3 replicates objects -> **in multiple AZs within the same Region** (NOT across Regions by default).
> - Bucket names unique -> **worldwide across all AWS accounts** (NOT just within your account/Region).
> - Default public? -> **FALSE**. All data is **private** by default.
> - S3 bucket associated with a specific Region? -> **TRUE**.
> - S3 is object storage suitable for flat files (Word docs, photos)? -> **TRUE**.

### S3 Storage Classes

> [!important] Know the hierarchy (most -> least accessed, most -> least expensive storage)
> | Class | Access frequency | Retrieval | Min duration | Notes |
> |---|---|---|---|---|
> | **S3 Standard** | Frequent | Instant (ms) | None | Default, 4 9s availability |
> | **S3 Intelligent-Tiering** | Unknown/changing | Instant (ms) | None | Auto-moves between tiers, monitoring fee |
> | **S3 Standard-IA** | Infrequent | Instant (ms) | **30 days** | Lower storage cost, retrieval fee |
> | **S3 One Zone-IA** | Infrequent, reproducible | Instant (ms) | **30 days** | Single AZ, ~20% cheaper than Standard-IA |
> | **S3 Glacier Instant Retrieval** | Rarely (quarterly) | Instant (ms) | **90 days** | Archive pricing, millisecond access |
> | **S3 Glacier Flexible Retrieval** | Rarely | Min to hrs | **90 days** | Expedited (1-5 min), Standard (3-5 hr), Bulk (5-12 hr) |
> | **S3 Glacier Deep Archive** | Very rarely | 12-48 hrs | **180 days** | **Lowest cost** storage in AWS |

> [!tip] Quiz-confirmed
> - Storage classes for lifecycle policies? -> **S3 Standard, S3 Infrequent Access, Glacier** (NOT DynamoDB, NOT Storage Gateway).
> - Lifecycle policies let you **delete or move objects based on age**.
> - Transition order is one-way: Standard -> IA -> Glacier (cannot go backward).

### S3 Pricing
- Pay for: **GB/month stored**, **transfer OUT** to other Regions, **PUT/COPY/POST/LIST/GET requests**.
- Free: transfers **IN** to S3, transfers **OUT** from S3 to CloudFront or EC2 **in the same Region**.

### Common S3 use cases
Static web hosting, backup/DR, storing app assets, media hosting, staging area for big data, software delivery.

---

## 4. Amazon EFS (Elastic File System)

> [!definition] EFS
> Fully managed, elastic **NFS file system**. Scales automatically from zero to **petabytes** without provisioning. Shared storage — multiple EC2 instances can access **simultaneously**.

### Key EFS facts
- Supports **NFS v4.0 / v4.1**.
- Compatible with **Linux-based** AMIs for EC2 (NOT Windows).
- Elastic capacity — grows/shrinks automatically.
- Use cases: big data, analytics, media processing, content management, web serving, home directories.

> [!tip] Quiz-confirmed
> - EFS is used to -> **implement storage for EC2 instances that multiple virtual machines can access at the same time** (shared, concurrent access).

---

## 5. Amazon S3 Glacier

> [!definition] S3 Glacier
> A data **archiving** service designed for security, durability (11 9s), and **extremely low cost**. Data is stored in **vaults** (containers for **archives**).

### Glacier features
- Encryption at rest (AES-256) + in transit (SSL/TLS).
- **Vault Lock** — enforce compliance via immutable policies (WORM - write once, read many).
- 3 retrieval speeds: **Expedited** (1-5 min), **Standard** (3-5 hr), **Bulk** (5-12 hr).
- Access control via IAM.
- **Lifecycle archiving** from S3 -> Glacier.

> [!tip] Quiz-confirmed
> - What is a vault? -> **A container for storing archives** (NOT the rules, NOT the objects, NOT a policy).
> - Glacier Deep Archive retrieval = **12-48 hours**.

---

## 6. The 4-Service Comparison (high-yield)

> [!important] Know when to use each
> | | **EBS** | **S3** | **EFS** | **S3 Glacier** |
> |---|---|---|---|---|
> | Storage type | Block | Object | File (NFS) | Object (archive) |
> | Capacity | Up to 64 TiB/volume | Virtually unlimited | Scales to petabytes | Virtually unlimited |
> | Access | Single EC2 (same AZ) | Anywhere (API/HTTP) | Multiple EC2 (shared) | Archive retrieval |
> | Persistence | Independent of EC2 | Independent | Independent | Independent |
> | Best for | Databases, boot vol | Media, backups, lakes | Shared files, CMS | Long-term archival |
> | Durability | Replicated in AZ | 11 9s (multi-AZ) | Multi-AZ | 11 9s |

---

## Rapid-Fire Recall

> [!question] Self-test
> - Block storage for EC2? -> **EBS** | Object storage? -> **S3** | File storage (NFS)? -> **EFS** | Archive? -> **Glacier**
> - S3 durability? -> **11 9s** (99.999999999%)
> - S3 default access? -> **Private** (not public)
> - Bucket name scope? -> **Globally unique across all accounts**
> - S3 replicates within? -> **Multiple AZs in the same Region**
> - Max S3 object size? -> **5 TB**
> - EBS data lost on stop? -> **No** (persists) | Instance Store on stop? -> **Yes** (ephemeral)
> - EFS supports Windows? -> **No** (Linux NFS only)
> - Multiple EC2 concurrent access? -> **EFS**
> - Glacier vault? -> **Container for archives**
> - Cheapest storage in AWS? -> **S3 Glacier Deep Archive**
> - Lifecycle direction? -> **Standard -> IA -> Glacier** (one-way)

**Prev:** [[06 - Compute]] | **Next:** [[08 - Databases]]
