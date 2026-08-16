---
tags: [aws, cloud-foundations, clf-c02, module-9, exam, architecture]
module: 9
title: Cloud Architecture
exam: AWS Certified Cloud Practitioner (CLF-C02)
domain: Cloud Concepts (24%) + Cloud Technology and Services (34%)
---

# Module 9 - Cloud Architecture

> [!info] Exam context
> Maps to **CLF-C02 Domain 1 (Cloud Concepts, 24%)** and **Domain 3 (Cloud Tech & Services, 34%)**. This module goes deep into the **Well-Architected Framework pillars + design principles**, **reliability vs availability vs fault tolerance**, and **AWS Trusted Advisor**.

> [!abstract] TL;DR Cram
> - **6 Well-Architected pillars:** Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, **Sustainability**.
> - Design principle: **"Assume everything will fail"** — design for failure, not prevention.
> - **Reliability** = system provides functionality when desired (MTBF).
> - **Availability** = % of uptime (99.999% = "five 9s").
> - **Fault tolerance** = system stays operational even when components fail (built-in redundancy).
> - **High availability** = withstands degradation, minimal downtime, minimal human intervention.
> - **Trusted Advisor** = best-practice checks across **5 categories**: Cost, Performance, Security, Fault Tolerance, Service Limits.

---

## 1. AWS Well-Architected Framework (Deep Dive)

> [!note] Pillar count discrepancy
> The v2 course slides say **5 pillars** (pre-2021 content). The current framework has **6 pillars** (Sustainability added Dec 2021). Your quiz may test either number — the quiz answer repo shows **5** for the v2 quiz, but the CLF-C02 exam tests **6**. Know both.

### Pillar 1: Operational Excellence

**Focus:** Run + monitor systems to deliver business value; continually improve processes.

> [!example] Design principles
> - **Perform operations as code** (IaC)
> - Annotate documentation
> - Make frequent, small, **reversible** changes
> - Refine operations procedures frequently
> - **Anticipate failure**
> - Learn from all operational events and failures

### Pillar 2: Security

**Focus:** Protect information, systems, assets via risk assessment + mitigation.

> [!example] Design principles
> - Implement a **strong identity foundation** (least privilege)
> - **Enable traceability** (log everything)
> - Apply security at **all layers**
> - **Automate** security best practices
> - Protect data **in transit and at rest**
> - **Keep people away from data**
> - **Prepare for security events**

See [[04 - AWS Cloud Security]] for full security details.

### Pillar 3: Reliability

**Focus:** Prevent + quickly recover from failures to meet demand.

> [!example] Design principles
> - **Test recovery procedures**
> - **Automatically recover** from failure
> - **Scale horizontally** to increase aggregate availability
> - **Stop guessing capacity**
> - **Manage change in automation**

### Pillar 4: Performance Efficiency

**Focus:** Use resources **efficiently** as demand and technologies evolve.

> [!example] Design principles
> - **Democratize advanced technologies** (use managed services instead of DIY)
> - **Go global in minutes**
> - **Use serverless architectures**
> - **Experiment more often**
> - Have **mechanical sympathy** (understand how services are consumed, pick the right tool)

> [!tip] Quiz-confirmed
> - Performance Efficiency design principles? -> **Use serverless architectures** + **Democratize advanced technologies** (NOT "enable traceability" — that's Security; NOT "analyze expenditure" — that's Cost Optimization).
> - NOT a Performance Efficiency area? -> **Traceability** (that's Security).

### Pillar 5: Cost Optimization

**Focus:** Deliver business value at the **lowest price point**.

> [!example] Design principles
> - **Adopt a consumption model** (pay only for what you use)
> - **Measure overall efficiency**
> - **Stop spending money on data center operations**
> - **Analyze and attribute expenditure**
> - Use **managed and application-level services** to reduce cost of ownership

### Pillar 6: Sustainability (newest)

**Focus:** Minimize the **environmental impact** of cloud workloads.

> [!tip] Quiz-confirmed
> - Focus of Sustainability pillar? -> **Minimize the environmental impacts of running cloud workloads** (NOT recover from failure, NOT avoid costs, NOT automate updates).

---

## 2. Reliability, Availability, and Fault Tolerance

> [!important] These three are different concepts — the exam tests the distinction

### Reliability
- The **probability** your entire system functions as intended for a specified period.
- Measured by **MTBF** (Mean Time Between Failures) = MTTF + MTTR.
  - **MTTF** = Mean Time **To** Failure (how long until it breaks).
  - **MTTR** = Mean Time **To** Repair (how long to fix it).

> [!tip] Quiz-confirmed
> - "System's ability to provide functionality when desired by the user" = ? -> **Reliability**.

### Availability
- **Uptime percentage** = normal operation time / total time.
- Expressed in **nines**: 99.9% = "three 9s" -> ~8.76 hrs downtime/yr. 99.999% = "five 9s" -> ~5.26 min downtime/yr.

### Fault Tolerance
- The built-in **redundancy** enabling the system to **remain operational** even when components fail.

> [!tip] Quiz-confirmed
> - "Ability to remain operational even if some components fail" = ? -> **Fault tolerance**.
> - "System withstands degradation, minimal downtime, minimal human intervention" = ? -> **Highly available**.

### Availability Factors
1. **Fault tolerance** — built-in redundancy.
2. **Scalability** — accommodate increased capacity without changing design.
3. **Recoverability** — policies/procedures for restoring service after a catastrophic event.

### Design Principles for Resilience

> [!important] Key cloud architecture principle
> **"Assume everything will fail"** — design systems to handle failure gracefully, not to prevent it entirely.

> [!tip] Quiz-confirmed
> - Design principle for cloud-based systems? -> **Assume everything will fail** (NOT tightly-coupled, NOT infrequent large changes, NOT "use as many services as possible").

---

## 3. AWS Trusted Advisor

> [!definition] Trusted Advisor
> An online tool providing **real-time guidance** to provision resources following AWS **best practices**. Inspects your AWS environment and gives recommendations across **5 categories**.

> [!important] The 5 Trusted Advisor categories (memorize)
> 1. **Cost Optimization** — idle resources, reserved capacity, right-sizing
> 2. **Performance** — service limits, over-utilized instances, CDN config
> 3. **Security** — security groups, IAM, MFA on root, S3 permissions
> 4. **Fault Tolerance** — backups, Multi-AZ, redundancy
> 5. **Service Limits** — approaching quotas
>
> Memory hook: **"C-P-S-F-S"** (Cost, Performance, Security, Fault tolerance, Service limits).

> [!tip] Quiz-confirmed
> - Trusted Advisor 5 categories? -> **Performance, cost optimization, security, fault tolerance, service limits** (NOT "high availability", NOT "connectivity", NOT "access control").
> - Security compliance tool after moving to cloud? -> **AWS Trusted Advisor**.

> [!note] Access levels (see [[02 - Cloud Economics and Billing#AWS Support Plans]])
> - **Basic + Developer** = core checks only (security + service limits subset).
> - **Business + Enterprise** = **full** Trusted Advisor checks (500+ checks across all 5 categories).

---

## Rapid-Fire Recall

> [!question] Self-test
> - How many Well-Architected pillars? -> **6** (CLF-C02) / **5** (v2 quiz without Sustainability)
> - Newest pillar? -> **Sustainability** (minimize environmental impact)
> - Performance Efficiency principles? -> serverless, democratize advanced tech, go global, experiment, mechanical sympathy
> - "Assume everything will fail" is a? -> **Cloud architecture design principle**
> - MTBF = ? -> MTTF + MTTR
> - Functionality when desired? -> **Reliability** | Uptime %? -> **Availability** | Redundancy staying operational? -> **Fault tolerance** | Minimal downtime + minimal human? -> **High availability**
> - Trusted Advisor 5 categories? -> Cost, Performance, Security, Fault Tolerance, Service Limits
> - Full Trusted Advisor access? -> **Business** support or higher

**Prev:** [[08 - Databases]] | **Next:** [[10 - Auto Scaling and Monitoring]]
