---
tags: [aws, cloud-foundations, clf-c02, exam, moc, cram]
title: AWS Cloud Foundations - Exam Index & Cram
exam: AWS Certified Cloud Practitioner (CLF-C02)
---

# AWS Cloud Foundations - Index & Night-Before Cram

> [!info] What this is
> Map of content for the 5 modules + a one-page cram of the facts most likely to be tested. Course = **AWS Academy Cloud Foundations**, aligned to the **AWS Certified Cloud Practitioner (CLF-C02)** exam.

## Modules
1. [[01 - Cloud Concepts Overview]] - Domain 1: Cloud Concepts (24%)
2. [[02 - Cloud Economics and Billing]] - Domain 4: Billing, Pricing, Support (12%)
3. [[03 - AWS Global Infrastructure Overview]] - Domain 3: Cloud Tech & Services (34%)
4. [[04 - AWS Cloud Security]] - Domain 2: Security & Compliance (30%)
5. [[05 - Networking and Content Delivery]] - Domain 3: Cloud Tech & Services (34%)

## Exam logistics
> [!note] CLF-C02 at a glance
> - **65 questions** (50 scored + 15 unscored), **90 minutes**.
> - Scaled score **100-1000**, **pass = 700**. Multiple choice + multiple response.
> - Domains: **Cloud Concepts 24% · Security & Compliance 30% · Cloud Tech & Services 34% · Billing/Pricing/Support 12%**.
> - Strategy: flag-and-return, eliminate wrong answers, watch for "NOT/EXCEPT" and "MOST/BEST" wording.

---

## The 30 facts most likely to be tested

> [!important] Concepts & economics
> - Cloud = **on-demand**, **pay-as-you-go**; infra **hardware -> software**.
> - **AWS owns the hardware**; you provision/use it.
> - Control: **IaaS > PaaS > SaaS**.
> - **6 Well-Architected pillars**: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, **Sustainability** (newest).
> - **6 CAF perspectives**: Business, People, Governance, Platform, Security, Operations.
> - **AURI > PURI > NURI** (All/Partial/No upfront = big/med/small discount). RI discount does **NOT** require full upfront.
> - **Spot** = up to 90% off, interruptible (2-min warning). **On-Demand** = no commit, priciest.
> - **Cost Explorer** = past/visualized cost; **Budgets** = alerts; **CUR** = most detailed.
> - **Consolidated billing** via **AWS Organizations** = one bill + volume discounts.
> - Support: **Basic/Developer/Business/Enterprise**; **Enterprise = designated TAM**, 15-min critical response.

> [!important] Infrastructure
> - Hierarchy **Region -> AZ -> Data Center**. 1 data center = **1 AZ** (a DC is NOT shared across AZs).
> - Region = min **3 AZs** (legacy slides: "2+"). Deploy across **multiple AZs** for HA.
> - **CloudFront uses edge locations** for low latency (700+ edges, far more than Regions).
> - Choose a Region by **compliance, latency, service availability, cost**.
> - **Outposts** = AWS on-prem; **Wavelength** = 5G edge; **Local Zones** = metro low-latency.
> - **Config** = configs; **CloudTrail** = API logs; **CloudWatch** = metrics/monitoring.

> [!important] Security
> - **AWS = security OF the cloud; Customer = security IN the cloud.**
> - IAM is **global + free**. **Implicit deny** default; **explicit deny always wins**.
> - **Role** = temporary, assumable credentials (not tied to a person).
> - **MFA** = extra login layer. After first login -> **delete root access keys**. Only **root** changes the support plan.
> - **SCPs never grant** - they set the max ceiling.
> - **KMS** = keys (NOT config auditing - that's **Config**). Data **at rest** (KMS) vs **in transit** (TLS/HTTPS/ACM).
> - **Shield** = DDoS · **WAF** = web exploits · **GuardDuty** = threat detection · **Inspector** = vuln scan · **Macie** = sensitive data in S3 · **Artifact** = compliance reports.

> [!important] Networking
> - VPC: single Region, multiple AZs. Max **/16**, smallest subnet **/28**.
> - **5 reserved IPs** per subnet (first 4 + last 1) -> /24 = **251 usable**.
> - **Security Group** = instance, **stateful**, allow-only. **NACL** = subnet, **stateless**, allow+deny.
> - **IGW** = public (both ways); **NAT Gateway** = private subnet outbound only.
> - **Route 53** = DNS (7 routing policies); **CloudFront** = CDN; **Global Accelerator** = non-HTTP over backbone.
> - **Direct Connect** = dedicated private line; **VPN** = encrypted tunnel over internet.

> [!tip] Final reminders
> - Watch for **"NOT"** / **"EXCEPT"** questions - read twice.
> - When a question says **"most cost-effective"**, think Spot/Reserved/Savings Plans appropriately.
> - When it says **"highly available"**, think **multiple AZs**.
> - When it says **"lowest latency to users"**, think **edge locations / CloudFront / closer Region**.
> - Sleep > cramming at 3am. You've got this.
