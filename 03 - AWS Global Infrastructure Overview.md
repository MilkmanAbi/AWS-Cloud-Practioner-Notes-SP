---
tags: [aws, cloud-foundations, clf-c02, module-3, exam, infrastructure]
module: 3
title: AWS Global Infrastructure Overview
exam: AWS Certified Cloud Practitioner (CLF-C02)
domain: Cloud Technology and Services (34%)
---

# Module 3 - AWS Global Infrastructure Overview

> [!info] Exam context
> Maps mostly to **CLF-C02 Domain 3: Cloud Technology and Services (34%)** - the biggest domain. Know the **Region / AZ / edge** hierarchy and the major **service categories**.

> [!abstract] TL;DR Cram
> - Hierarchy: **Region -> Availability Zone (AZ) -> Data Center**.
> - **Region** = geographic area, **min 3 AZs** (older slides say "2+"). **AZ** = isolated partition (1+ data centers), connected by **low-latency links**.
> - **Edge locations** = used by **CloudFront / Route 53 / Global Accelerator** for **low-latency** delivery. **700+ globally**, far more than Regions.
> - **Choose a Region by:** Compliance/laws, Latency/proximity, Service availability, Cost.
> - 3 infra features: **Elasticity/scalability, Fault-tolerance, High availability**.
> - Specialized infra: **Local Zones, Wavelength, Outposts**.

---

## 1. AWS Regions

> [!definition] AWS Region
> A **named geographic area** containing multiple, isolated **Availability Zones**. Cross-Region traffic uses the AWS **backbone network**. **You** control whether data replicates across Regions.

### Current scale (approx, April 2025)
- **36 Regions**, **114 Availability Zones**, **700+ edge locations**, **13 regional edge caches**, serving 245+ countries/territories.
- (Singapore = ap-southeast-1, has **4 AZs** + a Wavelength Zone.)

### Choosing a Region - 4 factors
| Factor | Why it matters |
|---|---|
| **Compliance / laws** | Data governance + legal/residency requirements |
| **Proximity / latency** | Closer to users = **lower latency** |
| **Service availability** | Not every service is in every Region (some are Region-locked) |
| **Cost / pricing** | Pricing **varies by Region** |

> [!tip] Quiz-confirmed
> - A Region is a **physical location with multiple AZs**, each in a **separate geographic area**.
> - Running apps in a Region **closer to users** -> **decreases** latency.

---

## 2. Availability Zones (AZ)

> [!important] AZ key facts
> - Each AZ is a **fully isolated partition** of AWS infrastructure (its own power, cooling, physical security).
> - Made of **one or more discrete data centers**.
> - Interconnected via **high-speed, low-latency, redundant private links**.
> - Designed for **fault isolation**.
> - **Best practice: deploy across MULTIPLE AZs** for high availability + fault tolerance.

> [!warning] Big exam trap
> "A data center can be used for **more than one** AZ" -> **FALSE / NOT TRUE**. Each data center belongs to exactly **one** AZ.

> [!note] Number-of-AZs nuance
> AWS Academy slides often say a Region has **"two or more"** AZs. AWS's **current** standard is a **minimum of 3** AZs per Region. If a question quotes the old "2 or more," that's the legacy answer; current marketing = min 3.

---

## 3. Data Centers

- Designed for **security**; redundant power, networking, connectivity; each in a **separate facility**.
- A data center typically holds **50,000-80,000 physical servers**.
- AWS does not publish exact locations (security).

---

## 4. Edge Network: Points of Presence

> [!important] Edge infrastructure
> - **Edge locations** - data centers at the network edge, **closest to end users**; serve cached/popular content fast. **700+ worldwide** - many more than Regions.
> - **Regional edge caches** - sit **between** the origin and edge locations; cache **less-popular** content (larger cache, longer TTL).
> - Used by **Amazon CloudFront** (CDN), **Amazon Route 53** (DNS), and **AWS Global Accelerator**. (Details: [[05 - Networking and Content Delivery#Amazon CloudFront]].)

> [!tip] THE most repeated Module 3 question
> "Which infra component does **CloudFront** use for **low-latency** delivery?" -> **Edge locations** (NOT Regions, NOT AZs, NOT VPC).

> [!warning] Trap
> "Edge locations are only in the same general area as Regions" -> **FALSE**. They are far more numerous and widely spread than Regions.

---

## 5. Specialized / Hybrid Infrastructure

> [!example] Know these three (CLF-C02 likes them)
> - **AWS Local Zones** - place compute/storage/select services **close to large population/industry centers** for single-digit-ms latency (extends a Region).
> - **AWS Wavelength** - embeds AWS compute/storage **inside 5G carrier networks** for ultra-low-latency mobile/edge apps (AR/VR, gaming).
> - **AWS Outposts** - runs **native AWS infrastructure on-premises** (your data center) for a consistent **hybrid** experience; for low-latency or **data-residency** needs.

---

## 6. Infrastructure Features

> [!important] Memorize these 3 definitions
> 1. **Elasticity & scalability** - resources **dynamically adjust** to capacity/growth.
> 2. **Fault-tolerance** - keeps operating through a failure (**built-in component redundancy**).
> 3. **High availability** - high performance, **minimized downtime**, **no human intervention**.

> [!tip] Quiz fill-in
> "___ = built-in component redundancy; ___ = resources dynamically adjust" -> **Fault-tolerant; elastic and scalable**.

---

## 7. Service Categories Overview

> [!note] Foundation services
> **Compute**, **Networking**, **Storage**. AWS has ~**23 categories**, each with many services. (These map to **CLF-C02 Domain 3**.) Brief intro here; depth comes in later modules.

### Compute
| Service | One-liner |
|---|---|
| **Amazon EC2** | Resizable virtual servers (IaaS) |
| **EC2 Auto Scaling** | Add/remove EC2 automatically |
| **AWS Lambda** | Run code **serverless** (no servers to manage) |
| **Elastic Beanstalk** | Deploy/scale web apps (PaaS-like) |
| **Amazon ECS / EKS / Fargate** | Containers (orchestration / Kubernetes / serverless) |
| **Amazon ECR** | Docker container registry |

### Storage
| Service | One-liner |
|---|---|
| **Amazon S3** | **Object** storage, highly durable/scalable |
| **Amazon EBS** | **Block** storage for EC2 |
| **Amazon EFS** | Managed elastic **file** (NFS) storage |
| **S3 Glacier** | Low-cost **archival** storage |

### Database
| Service | One-liner |
|---|---|
| **Amazon RDS** | Managed **relational** DB |
| **Amazon Aurora** | MySQL/PostgreSQL-compatible cloud DB |
| **Amazon DynamoDB** | **NoSQL** key-value/document, single-digit ms |
| **Amazon Redshift** | Petabyte-scale **data warehouse** |

### Networking & Content Delivery
**Amazon VPC**, **Elastic Load Balancing (ELB)**, **CloudFront**, **Route 53**, **Direct Connect**, **AWS VPN**, **Transit Gateway**. -> [[05 - Networking and Content Delivery]]

### Security, Identity & Compliance
**IAM**, **AWS Organizations**, **Cognito**, **KMS**, **Shield**, **WAF**, **GuardDuty**, **Inspector**, **Macie**, **Artifact**. -> [[04 - AWS Cloud Security]]

### Management & Governance (know the easily-confused trio)
| Service | Purpose |
|---|---|
| **AWS Config** | **Assess/audit/evaluate resource configurations** + config history |
| **Amazon CloudWatch** | **Monitoring/observability** - metrics, alarms, logs, dashboards |
| **AWS CloudTrail** | **Logs all API calls** (who did what, when) - governance/audit |
| **AWS Trusted Advisor** | Real-time **best-practice** recommendations |
| **AWS Well-Architected Tool** | Review workloads vs the 6 pillars |
| **AWS CLI** | Manage services from the command line |

> [!warning] The classic confusable trio
> - **Config** = **configuration** state (is my resource set up correctly?).
> - **CloudTrail** = **API activity log** (who called what?).
> - **CloudWatch** = **performance metrics/monitoring** (is it healthy? alarms).

---

## Rapid-Fire Recall

> [!question] Self-test
> - CloudFront low latency uses? -> **Edge locations**
> - Region = ? -> physical location, **multiple AZs**, separate geographic area
> - Two AZs share a data center? -> **No** (1 DC = 1 AZ)
> - Closer Region -> latency? -> **decreases**
> - Recommended AZ spread? -> **multiple**
> - On-prem AWS hardware? -> **Outposts** | 5G edge? -> **Wavelength** | metro low-latency? -> **Local Zones**
> - Logs API calls? -> **CloudTrail** | configs? -> **Config** | metrics? -> **CloudWatch**
> - 4 Region-choice factors? -> **compliance, latency, availability, cost**

**Prev:** [[02 - Cloud Economics and Billing]] | **Next:** [[04 - AWS Cloud Security]]
