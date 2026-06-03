---
tags: [aws, cloud-foundations, clf-c02, module-2, exam, billing]
module: 2
title: Cloud Economics and Billing
exam: AWS Certified Cloud Practitioner (CLF-C02)
domain: Billing, Pricing, and Support (12%)
---

# Module 2 - Cloud Economics and Billing

> [!info] Exam context
> Maps to **CLF-C02 Domain 4: Billing, Pricing, and Support (12%)**. Know the **pricing models**, **TCO**, **AWS Organizations / consolidated billing**, the **cost-management tools**, and the **support plans** cold.

> [!abstract] TL;DR Cram
> - **3 cost drivers:** Compute, Storage, **Data Transfer** (outbound charged, inbound usually free).
> - **EC2 pricing models:** On-Demand, **Reserved Instances** (AURI/PURI/NURI), **Savings Plans**, **Spot** (up to 90% off, interruptible), **Dedicated Hosts**.
> - **TCO** = compare on-premises vs AWS cost.
> - **AWS Organizations** = consolidate accounts + **consolidated billing** + **SCPs** + volume discounts.
> - **Cost tools:** Budgets, Cost Explorer, Cost & Usage Report, Pricing Calculator, Cost Anomaly Detection.
> - **Support plans:** Basic, Developer, Business, (Enterprise On-Ramp), Enterprise.

---

## 1. AWS Pricing Philosophy + Fundamentals

AWS pricing principles: **pay as you go**, **pay less when you reserve**, **pay less per unit as you use more** (volume tiers), and **pay even less as AWS grows** (economies of scale).

### Three Fundamental Cost Drivers
| Driver | How it's charged |
|---|---|
| **Compute** | By time used; varies by instance type/size |
| **Storage** | Per **GB** stored |
| **Data Transfer** | **Outbound** to internet is aggregated + charged per GB. **Inbound** and **same-Region** transfers between services are usually **free** |

> [!tip] Data transfer trap
> - Inbound data transfer = usually **free** (with exceptions).
> - Outbound between AWS services **in the same Region** = **free**.
> - Outbound to the internet / across Regions = **charged**.

---

## 2. EC2 Pricing Models (know all five)

> [!important] The five ways to pay for EC2
> | Model | Commitment | Savings | Best for |
> |---|---|---|---|
> | **On-Demand** | None | Baseline (most expensive/hr) | Short-term, spiky, unpredictable, dev/test |
> | **Reserved Instances (RI)** | 1 or 3 years | Up to **~72%** | Steady-state, predictable usage |
> | **Savings Plans** | 1 or 3 yr $/hr commit | Up to **~72%** | Flexible steady usage across families/services |
> | **Spot Instances** | None (spare capacity) | Up to **~90%** | Fault-tolerant, interruptible, batch |
> | **Dedicated Hosts** | Optional | - | Compliance, **BYOL** licensing, isolation |

> [!warning] Spot detail
> Spot uses spare capacity and can be **reclaimed by AWS with a 2-minute warning**. Use for stateless/fault-tolerant work (batch, CI, big data) - NOT databases or critical state.

### Reserved Instances - AURI / PURI / NURI

> [!important] THE high-yield mnemonic (this WILL be on the exam)
> Available for **EC2** and **RDS** (and others). More paid upfront = bigger discount.
>
> | Acronym | Meaning | Discount |
> |---|---|---|
> | **AURI** | **A**ll **U**pfront RI | **Largest** |
> | **PURI** | **P**artial **U**pfront RI | Medium |
> | **NURI** | **N**o Upfront RI | **Smallest** |
>
> Hook: **A > P > N** (pay more upfront -> save more).

> [!warning] Quiz gotcha
> "Full upfront payment is REQUIRED for the RI discount" -> **FALSE** (NURI = no upfront still gets a discount).

### Savings Plans vs Reserved Instances
- **Compute Savings Plans** - up to ~66% off; flexible across **EC2, Fargate, Lambda**, any family/Region.
- **EC2 Instance Savings Plans** - up to ~72% off; locked to an instance **family + Region** but flexible on size/OS.
- Both offer **All / Partial / No upfront** payment, like RIs.

---

## 3. Free / no-cost AWS services

> [!note] These services are free to use (you pay for resources used WITH them)
> **Amazon VPC**, **AWS Elastic Beanstalk**, **Auto Scaling**, **AWS CloudFormation**, **AWS IAM**, **Consolidated Billing**, **AWS OpsWorks**.

> [!warning] Free Tier trap
> "**Unlimited** services free for 12 months" -> **FALSE**. The **AWS Free Tier** has limits, and comes in 3 flavors: **12-months free** (new accounts), **Always free**, and **Trials** (short-term).

---

## 4. Total Cost of Ownership (TCO)

> [!definition] TCO
> A **financial estimate** of direct + indirect costs of a system. Used to **compare on-premises vs AWS** and build the business case for migrating. (Historically the **TCO Calculator**; today AWS uses the **AWS Pricing Calculator** at calculator.aws.)

### Four TCO cost categories
1. **Server costs** - hardware (server, rack, PDUs, TOR switches), software (OS, virtualization licenses), facilities (space/power/cooling).
2. **Storage costs** - disks, SAN/FC switches, storage admin, facilities.
3. **Network costs** - LAN switches, load balancer, bandwidth, admin, facilities.
4. **IT labor costs** - server administration.

> [!tip] Quiz
> Tool that compares on-premises data center cost to AWS -> **TCO Calculator** (now part of the **AWS Pricing Calculator**).

---

## 5. AWS Organizations + Consolidated Billing

> [!definition] AWS Organizations
> An **account management service** that consolidates multiple AWS accounts into one **organization** you centrally manage. Provides **consolidated billing** + meets budgetary/security/compliance needs. **Free.**

### Key features + benefits
- **Consolidated billing** - one bill for all accounts; the **management/payer account** pays.
- **Volume pricing** - usage is **aggregated across accounts** to reach lower price tiers/RI sharing sooner.
- **Organizational Units (OUs)** - group accounts; attach policies per OU.
- **Service Control Policies (SCPs)** - allow/deny services/API actions per account (the permission **ceiling**).
- **APIs** to automate account creation/management.

### Setup order
1. Create organization -> 2. Create OUs -> 3. Attach SCPs -> 4. Test restrictions.

> [!note] Effective permissions = the **intersection** of what an SCP allows AND what [[04 - AWS Cloud Security#Identity and Access Management IAM|IAM]] grants. An SCP **never grants** access on its own.

---

## 6. Billing & Cost Management Tools

> [!important] Match the tool to the job
> | Tool | Purpose |
> |---|---|
> | **AWS Budgets** | Set **custom budgets**; alert when cost/usage **exceeds (or is forecast to exceed)** a threshold |
> | **AWS Cost Explorer** | **Visualize / understand / manage** costs + usage **over time**; forecast; find past spend (e.g. EC2 from 3 months ago) |
> | **AWS Cost and Usage Report (CUR)** | **Most comprehensive** cost + usage data set (line-item detail, RI/SP metadata); can deliver to S3 |
> | **AWS Pricing Calculator** | Estimate cost of a planned architecture **before** building |
> | **AWS Cost Anomaly Detection** | ML-based detection of unusual spend |
> | **AWS Billing Dashboard** | Central view of current charges + trends |

> [!tip] Classic quiz
> "Where to see EC2 billing from 3 months ago?" -> **AWS Cost Explorer**.

---

## 7. AWS Support Plans

All plans include **24/7** access to customer service, **documentation, whitepapers, and support forums**, plus core **Trusted Advisor** checks and the **Personal Health Dashboard**.

> [!important] The support plans (low -> high)
> | Plan | Who it's for | Case support | Trusted Advisor |
> |---|---|---|---|
> | **Basic** | Everyone (default, **free**) | **No** technical case support | Core checks only |
> | **Developer** | Early dev / experimentation | Business-hours **email** | Core checks |
> | **Business** | **Production** workloads | **24/7** phone/chat/email | **Full** checks + Support API |
> | **Enterprise On-Ramp** | Growing production | 24/7 + pool of TAMs | Full checks |
> | **Enterprise** | Business/mission-critical | 24/7 + **designated TAM** + Concierge | Full + **Trusted Advisor Priority** |

### Response time targets (high-yield)
| Severity | Developer | Business | Enterprise On-Ramp | Enterprise |
|---|---|---|---|---|
| General guidance | < 24 h | < 24 h | < 24 h | < 24 h |
| System impaired | < 12 h | < 12 h | < 12 h | < 12 h |
| Production impaired | - | < 4 h | < 4 h | < 4 h |
| Production down | - | < 1 h | < 1 h | < 1 h |
| Business-critical down | - | - | **< 30 min** | **< 15 min** |

> [!example] Premium support extras
> - **Technical Account Manager (TAM)** - proactive guidance (Enterprise On-Ramp = pool; **Enterprise = designated**).
> - **AWS Trusted Advisor** - real-time best-practice checks across **cost, performance, security, fault tolerance, service limits**.
> - **Concierge Support Team** - billing/account help (Enterprise).

> [!note] 2026-2027 transition (FYI only - your exam tests the classic plans above)
> AWS is phasing out Developer / Business / Enterprise On-Ramp by **Jan 1, 2027**, replacing them with **Business Support+** ($29/mo min) and a cheaper **Enterprise Support** ($5,000/mo min). For the CLF-C02 / AWS Academy exam, **stick to the classic Basic/Developer/Business/Enterprise model**.

---

## Rapid-Fire Recall

> [!question] Self-test
> - 3 RI payment options? -> **AURI, PURI, NURI** (All > Partial > No upfront)
> - Cheapest, interruptible EC2? -> **Spot** (up to 90%, 2-min warning)
> - No commitment, most flexible/expensive? -> **On-Demand**
> - Full upfront required for RI discount? -> **False**
> - Support plans? -> **Basic, Developer, Business, (On-Ramp), Enterprise**
> - On-prem vs AWS cost tool? -> **TCO / Pricing Calculator**
> - EC2 bill from 3 months ago? -> **Cost Explorer**
> - One bill for many accounts? -> **Consolidated billing (AWS Organizations)**
> - Designated TAM? -> **Enterprise** support
> - Free transfers? -> inbound (mostly) + same-Region between services

**Prev:** [[01 - Cloud Concepts Overview]] | **Next:** [[03 - AWS Global Infrastructure Overview]]
