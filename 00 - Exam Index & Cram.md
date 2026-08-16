---
tags: [aws, cloud-foundations, clf-c02, exam, moc, cram]
title: AWS Cloud Foundations - Exam Index & Cram
exam: AWS Certified Cloud Practitioner (CLF-C02)
---

# AWS Cloud Foundations - Index & Night-Before Cram

> [!info] What this is
> Map of content for all 10 modules + a one-page cram of the facts most likely to be tested. Course = **AWS Academy Cloud Foundations v2**, aligned to the **AWS Certified Cloud Practitioner (CLF-C02)** exam.

## Modules
1. [[01 - Cloud Concepts Overview]] - Domain 1: Cloud Concepts (24%)
2. [[02 - Cloud Economics and Billing]] - Domain 4: Billing, Pricing, Support (12%)
3. [[03 - AWS Global Infrastructure Overview]] - Domain 3: Cloud Tech & Services (34%)
4. [[04 - AWS Cloud Security]] - Domain 2: Security & Compliance (30%)
5. [[05 - Networking and Content Delivery]] - Domain 3: Cloud Tech & Services (34%)
6. [[06 - Compute]] - Domain 3: Cloud Tech & Services (34%)
7. [[07 - Storage]] - Domain 3: Cloud Tech & Services (34%)
8. [[08 - Databases]] - Domain 3: Cloud Tech & Services (34%)
9. [[09 - Cloud Architecture]] - Domain 1 + Domain 3
10. [[10 - Auto Scaling and Monitoring]] - Domain 3: Cloud Tech & Services (34%)

## Exam logistics
> [!note] CLF-C02 at a glance
> - **65 questions** (50 scored + 15 unscored), **90 minutes**.
> - Scaled score **100-1000**, **pass = 700**. Multiple choice + multiple response.
> - Domains: **Cloud Concepts 24% · Security & Compliance 30% · Cloud Tech & Services 34% · Billing/Pricing/Support 12%**.

---

## The Cram: 50 Facts Across All 10 Modules

> [!important] Concepts & Economics (M1, M2)
> - Cloud = **on-demand**, **pay-as-you-go**; infra **hardware -> software**. **AWS owns the hardware**.
> - Control: **IaaS > PaaS > SaaS**. 3 deployment models: **Cloud, Hybrid, On-premises**.
> - **6 advantages:** CapEx->OpEx, economies of scale, stop guessing, speed/agility, stop running DCs, go global.
> - **6 Well-Architected pillars:** Ops Excellence, Security, Reliability, Perf Efficiency, Cost Optimization, **Sustainability**.
> - **6 CAF perspectives:** Business, People, Governance, Platform, Security, Operations.
> - **AURI > PURI > NURI** (All/Partial/No upfront). Full upfront NOT required for RI discount.
> - **Spot** = up to 90% off, 2-min warning. **On-Demand** = no commit, most flexible/expensive.
> - **Cost Explorer** = past cost visualization. **Budgets** = threshold alerts. **CUR** = most detailed.
> - **Consolidated billing** via **Organizations** = one bill + volume discounts. **SCPs** = permission ceiling.
> - Support: **Basic/Developer/Business/Enterprise**. Enterprise = **designated TAM**, 15-min critical.

> [!important] Infrastructure (M3)
> - **Region -> AZ -> Data Center**. 1 DC ≠ shared across AZs.
> - Region = min **3 AZs**. Deploy across **multiple AZs** for HA.
> - **CloudFront uses edge locations** (700+). Choose Region by: compliance, latency, availability, cost.
> - **Outposts** = on-prem. **Wavelength** = 5G edge. **Local Zones** = metro.
> - **Config** = configs. **CloudTrail** = API logs. **CloudWatch** = metrics/monitoring.

> [!important] Security (M4)
> - **AWS = OF the cloud; Customer = IN the cloud.**
> - IAM = **global + free**. **Implicit deny** default; **explicit deny always wins**.
> - **Role** = temporary assumable credentials. **MFA** = extra login layer. **Delete root access keys.**
> - **KMS** = encryption keys. **Shield** = DDoS. **WAF** = web exploits. **GuardDuty** = threat detection. **Inspector** = vuln scan. **Macie** = sensitive data in S3. **Artifact** = compliance reports.

> [!important] Networking (M5)
> - VPC: single Region, multiple AZs. Max **/16**, smallest subnet **/28**. **5 reserved IPs** -> /24 = **251 usable**.
> - **Security Group** = instance, **stateful**, allow-only. **NACL** = subnet, **stateless**, allow+deny.
> - **IGW** = public both ways. **NAT Gateway** = private outbound only.
> - **Route 53** = DNS. **CloudFront** = CDN. **Direct Connect** = dedicated private line. **VPN** = encrypted over internet.

> [!important] Compute (M6)
> - EC2 naming: `t3.large` = family + generation + size. **5 families:** General, Compute, Memory, Storage, Accelerated.
> - **AMI** = root volume template + launch permissions + block device mapping. Must specify **AMI + instance type** at launch.
> - **Lambda** = serverless, event-driven, max **15 min**, max **10 GB** memory, pay per request + GB-sec.
> - **Beanstalk** = PaaS (upload code, AWS handles rest). **Fargate** = serverless containers.
> - Containers do **NOT** contain an entire OS. Steady base + spike = **Reserved + On-Demand**.

> [!important] Storage (M7)
> - **EBS** = block, same-AZ, persistent, snapshots. **S3** = object, 11 9s durability, 5 TB max, **private** by default.
> - S3 bucket name = **globally unique**. Replicates across **multiple AZs in same Region**.
> - **EFS** = NFS file, shared concurrent access, **Linux only**, auto-scales to petabytes.
> - **Glacier** = archive. Vault = container for archives. **Deep Archive** = cheapest storage in AWS.
> - Lifecycle: Standard -> IA -> Glacier (one-way). **Instance Store** = ephemeral (data lost on stop).

> [!important] Databases (M8)
> - **RDS** = managed relational (6 engines). Auto-patches, auto-backs up, Multi-AZ (HA), read replicas (perf).
> - **Aurora** = MySQL/PostgreSQL-compatible, 5x perf, up to 15 read replicas, 3-AZ replication.
> - **DynamoDB** = NoSQL, key-value/document, single-digit ms, flexible schema, virtually unlimited scale.
> - **Redshift** = petabyte data warehouse, SQL, BI/analytics, columnar.
> - Complex transactions -> **RDS/Aurora**. Simple GET/PUT, session state -> **DynamoDB**. Analytics/BI -> **Redshift**.

> [!important] Architecture + Scaling (M9, M10)
> - **"Assume everything will fail"** = key cloud design principle.
> - **Reliability** = functionality when desired (MTBF). **Fault tolerance** = stays operational via redundancy. **High availability** = minimal downtime, no human intervention.
> - **Trusted Advisor** 5 categories: **Cost, Performance, Security, Fault Tolerance, Service Limits**.
> - **ELB** distributes traffic: **ALB** (L7 HTTP/S) vs **NLB** (L4 TCP/UDP). Requires a **listener**.
> - **Auto Scaling Group** = min + desired + max. **Launch template** = AMI + type + storage.
> - Scale **out** = launch. Scale **in** = terminate. CloudWatch alarm -> Auto Scaling -> ELB registers.
> - **SNS** sends alert notifications from CloudWatch alarms.

> [!tip] Final exam tips
> - Watch for **"NOT"** / **"EXCEPT"** — read twice.
> - **"Most cost-effective"** -> think Spot/Reserved/Savings Plans appropriately.
> - **"Highly available"** -> multiple AZs + ELB + Auto Scaling.
> - **"Lowest latency to users"** -> edge locations / CloudFront / closer Region.
> - **"Which database?"** -> complex queries = RDS; fast + flexible = DynamoDB; analytics = Redshift.
> - **"Serverless"** -> Lambda (compute), Fargate (containers), DynamoDB (DB), S3 (storage).
