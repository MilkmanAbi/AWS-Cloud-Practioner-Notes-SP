---
tags: [aws, cloud-foundations, clf-c02, module-1, exam]
module: 1
title: Cloud Concepts Overview
exam: AWS Certified Cloud Practitioner (CLF-C02)
domain: Cloud Concepts (24%)
---

# Module 1 - Cloud Concepts Overview

> [!info] Exam context
> Maps to **CLF-C02 Domain 1: Cloud Concepts (24%)**. This is the "why the cloud" module: value proposition, the 6 benefits, service + deployment models, the **Well-Architected Framework**, and the **AWS Cloud Adoption Framework (CAF)**.

> [!abstract] TL;DR Cram
> - **Cloud computing** = on-demand delivery of IT resources over the internet with **pay-as-you-go** pricing. Infra shifts from **hardware -> software**.
> - **6 advantages** of cloud (CapEx->OpEx, economies of scale, stop guessing capacity, speed/agility, stop running data centers, go global in minutes).
> - **3 service models:** IaaS / PaaS / SaaS (most -> least control).
> - **3 deployment models:** Cloud / Hybrid / On-premises (Private Cloud).
> - **6 Well-Architected pillars:** Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, **Sustainability** (newest).
> - **6 CAF perspectives:** Business, People, Governance | Platform, Security, Operations.
> - 3 ways to access AWS: **Console / CLI / SDK**.

---

## 1. What is Cloud Computing?

> [!definition] Cloud Computing
> The **on-demand delivery** of compute power, database, storage, applications, and other IT resources via the internet, with **pay-as-you-go pricing**. You stop thinking of infrastructure as **hardware** and start using it as **software**.

### Traditional vs Cloud

| Traditional (on-premises) | Cloud |
|---|---|
| Infrastructure as **hardware** | Infrastructure as **software** |
| Capital expenditure (**CapEx**), big upfront purchases | Variable/operational expense (**OpEx**) |
| Needs space, staff, physical security, planning | Provider handles facilities + hardware |
| Long hardware **procurement cycle** (weeks/months) | Provision in **minutes**, change quickly |
| Provision for **guessed peak** (over/under-provision) | **Scale on demand**, match real usage |
| You do "undifferentiated heavy lifting" | Heavy lifting eliminated |

> [!warning] Exam trap (ownership)
> "You own the network-connected hardware and AWS provisions what you need" -> **FALSE**. Reverse it: **AWS owns and maintains the hardware**; **you provision and use** what you need.

---

## 2. The Six Advantages of Cloud Computing

> [!important] Memorize all 6 (very high-yield)
> 1. **Trade capital expense for variable expense** - pay only for what you consume; no big upfront server purchases.
> 2. **Benefit from massive economies of scale** - aggregated usage of hundreds of thousands of customers -> lower pay-as-you-go prices.
> 3. **Stop guessing capacity** - scale up/down on demand; no over- or under-provisioning.
> 4. **Increase speed and agility** - new resources are a click away; experiment cheaply.
> 5. **Stop spending money running and maintaining data centers** - focus on customers, not racking/stacking/powering servers.
> 6. **Go global in minutes** - deploy in multiple Regions worldwide for lower latency.

> [!tip] Quiz-confirmed
> - "Economies of scale result from..." -> **hundreds of thousands of customers aggregated in the cloud**.
> - NOT benefits: **high latency**, **multiple procurement cycles**, **paying for racking/stacking/powering**.

---

## 3. Cloud Service Models (control spectrum)

> [!important] IaaS -> PaaS -> SaaS = MOST -> LEAST control
> | Model | You manage | Provider manages | Example feel |
> |---|---|---|---|
> | **IaaS** (Infrastructure as a Service) | OS, apps, runtime, data, networking config, security | Physical hardware, virtualization | EC2, raw VMs |
> | **PaaS** (Platform as a Service) | Your **code + data** | OS, DB patching, firewall, runtime, scaling | Elastic Beanstalk |
> | **SaaS** (Software as a Service) | Just **use** it (your data/usage) | Everything else | Email, web apps |

Security responsibility shifts with the model -> see [[04 - AWS Cloud Security#Responsibility shifts by service model]].

## 4. Cloud Deployment Models

1. **Cloud (all-in / "cloud-native")** - everything runs in the cloud.
2. **Hybrid** - mix of cloud + on-premises (connected, e.g. via VPN/Direct Connect).
3. **On-premises / Private Cloud** - deployed in your own data center using virtualization/resource-management tools.

> [!warning] Gotcha
> "System administration as a service" is **NOT** a deployment model.

---

## 5. Introduction to AWS

> [!definition] Web Service
> Any software made available over the internet that uses a standardized format like **XML** or **JSON** for the request/response of an **API** interaction.

**AWS** = a secure cloud platform offering a broad set of global **services** designed to work together. Choose services based on **business goals + technology requirements**.

### Three ways to interact with AWS
| Method | What it is | Who uses it |
|---|---|---|
| **AWS Management Console** | Graphical web UI | Beginners, visual tasks |
| **AWS CLI** (Command Line Interface) | Commands / scripts | Automation, scripting |
| **SDKs** (Software Development Kits) | Call AWS directly from code | Developers (Python boto3, JS, Java...) |

---

## 6. AWS Well-Architected Framework

> [!definition] Well-Architected Framework
> A set of **best practices + design principles** across **6 pillars** for building secure, high-performing, resilient, efficient, and sustainable workloads. Reviewed using the **AWS Well-Architected Tool**.

> [!important] The 6 Pillars (Sustainability is the newest, added Dec 2021)
> 1. **Operational Excellence** - run + monitor systems, continuously improve processes (automation, IaC).
> 2. **Security** - protect data, systems, assets ([[04 - AWS Cloud Security]]).
> 3. **Reliability** - recover from failure, scale to meet demand (redundancy, backups).
> 4. **Performance Efficiency** - use resources efficiently as demand/tech changes (right-sizing, serverless).
> 5. **Cost Optimization** - deliver value at the lowest price point ([[02 - Cloud Economics and Billing]]).
> 6. **Sustainability** - minimize environmental impact of workloads.
>
> Memory hook: **"Ops Sec Rel Perf Cost Sust"** -> O-S-R-P-C-S.

---

## 7. AWS Cloud Adoption Framework (AWS CAF)

> [!definition] AWS CAF
> Guidance + best practices to help organizations build a comprehensive plan for cloud adoption across the org and IT lifecycle, to **accelerate successful adoption**. Organized into **6 perspectives**, each a set of **capabilities**. Helps identify and close capability **gaps**.

> [!important] The 6 Perspectives - split into Business + Technical
> **Business capabilities (people/process side):**
> 1. **Business** - ensure IT aligns with business needs (IT finance, IT strategy, benefits realization, business risk mgmt).
> 2. **People** - staffing, training, org change (resource/incentive/career/training/change mgmt).
> 3. **Governance** - align IT strategy with business strategy (portfolio mgmt, program/project mgmt, license mgmt).
>
> **Technical capabilities (tech side):**
> 4. **Platform** - architecture of the target-state environment (compute/network/storage/DB provisioning, app dev).
> 5. **Security** - meet security objectives (IAM, detective control, infra security, data protection, incident response).
> 6. **Operations** - run + monitor day-to-day (service/app monitoring, change mgmt, BC/DR, IT service catalog).
>
> Memory hook: **"B-P-G then P-S-O"** (Business/People/Governance, Platform/Security/Operations).

> [!note] Don't confuse the two frameworks
> - **Well-Architected** = how to **build a good workload** (6 pillars).
> - **CAF** = how an **organization adopts the cloud** (6 perspectives).

---

## Rapid-Fire Recall

> [!question] Self-test
> - Pricing for as-needed resources? -> **Pay as you go**
> - Who owns the hardware? -> **AWS**
> - Not a deployment model? -> **System administration as a service**
> - Most -> least control? -> **IaaS -> PaaS -> SaaS**
> - How many Well-Architected pillars + newest? -> **6**, newest = **Sustainability**
> - How many CAF perspectives? -> **6** (Business, People, Governance, Platform, Security, Operations)
> - 3 ways to access AWS? -> **Console, CLI, SDK**
> - CapEx vs OpEx: cloud favors? -> **variable/operational expense (OpEx)**

**Next:** [[02 - Cloud Economics and Billing]]
