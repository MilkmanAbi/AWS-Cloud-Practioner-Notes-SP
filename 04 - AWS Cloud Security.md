---
tags: [aws, cloud-foundations, clf-c02, module-4, exam, security]
module: 4
title: AWS Cloud Security
exam: AWS Certified Cloud Practitioner (CLF-C02)
domain: Security and Compliance (30%)
---

# Module 4 - AWS Cloud Security

> [!info] Exam context
> Maps to **CLF-C02 Domain 2: Security and Compliance (30%)** - the **second-largest** domain (grew from 25% in CLF-C01). The **Shared Responsibility Model** and **IAM** are the two heaviest topics. This module is where you cannot afford to be vague.

> [!abstract] TL;DR Cram
> - **Shared Responsibility:** AWS = security **OF** the cloud; Customer = security **IN** the cloud.
> - **IAM:** Users, Groups, Roles, Policies. **Least privilege**. **Implicit deny** by default; **explicit deny always wins**. IAM is **global** + **free**.
> - **Root user:** use only when required; enable **MFA**, remove root access keys.
> - **SCPs** never grant - they set the permission ceiling.
> - **Encryption:** KMS (keys), data **at rest** vs **in transit (TLS/HTTPS)**, ACM (certs).
> - **Security services:** Shield (DDoS), WAF (web exploits), GuardDuty (threat detection), Inspector (vuln scan), Macie (sensitive data in S3), Config, CloudTrail, Artifact.

---

## 1. AWS Shared Responsibility Model

> [!important] The single most important security concept on the exam
> - **AWS = security OF the cloud** (the infrastructure that runs everything).
> - **Customer = security IN the cloud** (everything you put in + how you configure it).

| AWS responsible for (OF the cloud) | Customer responsible for (IN the cloud) |
|---|---|
| **Physical security** of data centers | **Customer data** |
| Hardware + global infrastructure | **EC2 guest OS** - patching, maintenance |
| Compute, storage, DB, networking (managed) | **Applications** - IAM passwords, role-based access |
| **Virtualization** layer (instance isolation) | **Security group** + firewall config |
| Storage decommissioning, host OS logging | **Network/VPC config**, encryption settings |
| | **Client + server-side encryption**, data integrity |

> [!tip] Quiz-confirmed
> - "Security IN the cloud" examples -> **security group configuration** + **encryption of data at rest/in transit**.
> - AWS responsibility -> **maintaining physical hardware**.

### Responsibility shifts by service model
- **IaaS** (e.g. EC2) -> customer manages the **most** (OS, patching, firewall, app, data).
- **PaaS** (e.g. Beanstalk, RDS) -> AWS handles OS/DB patching/firewall; customer manages **code + data + access**.
- **SaaS** -> customer manages the **least** (mostly just data + user access). (See [[01 - Cloud Concepts Overview#Cloud Service Models control spectrum]].)

---

## 2. Identity and Access Management (IAM)

> [!definition] IAM
> A **free, global** web service to manage **who can do what** in your AWS account - users, credentials (access keys, passwords, MFA), and permissions controlling resource access.

> [!note] IAM is **global** (not Region-specific) and is **free**.

### Four core components

> [!important] Users / Groups / Roles / Policies
> **IAM User** - a person or application that authenticates with the account. Two access types:
> - **Programmatic access** -> **Access Key ID + Secret Access Key** (CLI/SDK/API).
> - **Console access** -> **username + password** (+ optional **MFA**).
>
> **IAM Group** - a collection of users with the **same permissions**.
> - A user **can be in multiple groups**.
> - **No default group.** **Groups cannot be nested.** Groups can't contain other groups.
>
> **IAM Role** - a set of permissions that is **assumable** (not tied to one person); provides **temporary credentials** via **STS**. Used for cross-account access, EC2 instances accessing S3, federated users, services calling services.
>
> **IAM Policy** - a **JSON** document defining permissions (Effect: Allow/Deny, Action, Resource).
> - All permissions are **implicitly denied** by default.
> - An **explicit Deny always overrides** any Allow.
> - **Identity-based** (attach to user/group/role) vs **Resource-based** (attach to a resource e.g. S3 bucket policy).

> [!important] Evaluation logic (memorize)
> 1. **Default = deny** (implicit).
> 2. An explicit **Allow** grants access...
> 3. ...unless an explicit **Deny** exists -> **Deny always wins**.

> [!tip] Quiz-confirmed
> - Two access types -> **Programmatic Access** + **AWS Management Console Access**.
> - IAM best practices -> **manage access to AWS resources** + **define fine-grained access rights** (NOT default admin, NOT leaving unused creds).

### Principle of Least Privilege (POLP)

> [!definition] Least Privilege
> Grant only the **minimum permissions** needed to do the job - read/write/execute only the required resources. Start minimal, add as needed.

### IAM Identity Center (formerly AWS SSO)
- Central place to manage **single sign-on (SSO)** access across multiple accounts + business apps; integrates with external identity providers.

---

## 3. Securing a New AWS Account (best practices)

> [!important] The root user has UNLIMITED access - protect it
> Root logs in with the account's **email + password**. Use it only for the handful of tasks that **require** it (e.g. closing the account, changing the support plan, changing account settings).

### Steps to secure a new account
1. **Stop using root for daily work:**
   1. Create an **IAM user** for yourself.
   2. Create an **IAM group** with admin permissions; add your user.
   3. **Delete/disable root access keys**.
   4. Enable a **strong password policy**.
   5. Sign in with the IAM user from now on.
2. **Enable MFA** (multi-factor authentication) on root + privileged users - an extra login layer.
3. **Enable AWS CloudTrail** - logs all API calls; free event history = last **90 days**.
4. **Enable a billing report** (e.g. Cost & Usage Report). (See [[02 - Cloud Economics and Billing#Billing Cost Management Tools]].)

> [!tip] Quiz-confirmed
> - Add a login security layer -> **enable MFA**.
> - Best practice after first root login -> **delete the root access keys**.
> - Only the **root user** can -> **change the AWS support plan** (and close the account, change account name/email).

---

## 4. Organizations + Account-level Security

> [!note] AWS Organizations security features
> - Group accounts into **OUs**; apply different policies per OU.
> - Effective permissions = **intersection** of SCP allow AND IAM grant.
> - **SCPs** = central control over which services/API actions an account may use.

> [!important] SCP rule (repeat until automatic)
> **Service Control Policies NEVER grant permissions** - they only set the **maximum** (ceiling). You still need IAM to grant.

---

## 5. Data Protection + Encryption

> [!example] At rest vs in transit
> **Data at rest** = stored physically (disk, tape, S3, EBS).
> - **AWS KMS** creates + manages **encryption keys**; integrates with most services + **CloudTrail** logs key usage; uses **FIPS 140-2 validated HSMs**.
> - **AWS CloudHSM** = dedicated, single-tenant hardware security module.
>
> **Data in transit** = moving across a network.
> - **TLS** (formerly **SSL**) encrypts in transit. **AWS Certificate Manager (ACM)** provisions/renews TLS/SSL certs.
> - **HTTPS** = secure tunnel over TLS.

| Service | Use |
|---|---|
| **AWS KMS** | Create/manage encryption keys |
| **AWS CloudHSM** | Dedicated hardware key storage |
| **AWS ACM** | Manage/renew TLS/SSL certificates |
| **AWS Secrets Manager** | Store + **rotate** secrets (DB passwords, API keys) |

> [!warning] Classic exam trap
> "**KMS** lets you assess/audit/evaluate the **configurations** of your resources" -> **FALSE**. That's **AWS Config**. KMS = **encryption keys**. ([[03 - AWS Global Infrastructure Overview#Management Governance know the easily-confused trio]])

---

## 6. Key Security & Identity Services (CLF-C02 expanded these)

> [!important] Know what each one does + the difference
> | Service | What it does |
> |---|---|
> | **AWS Shield** | **DDoS** protection. **Standard = free/always-on**; **Advanced = paid** |
> | **AWS WAF** | **Web Application Firewall** - blocks common web exploits (SQL injection, XSS) at layer 7 |
> | **Amazon GuardDuty** | **ML-based threat detection** - continuously analyzes logs (CloudTrail, VPC, DNS) for malicious activity |
> | **Amazon Inspector** | Automated **vulnerability scanning** of EC2, containers, Lambda |
> | **Amazon Macie** | Discovers + protects **sensitive data (PII)** in **S3** using ML |
> | **AWS Security Hub** | **Aggregates** findings from GuardDuty/Inspector/Macie/Config into one dashboard (CSPM) |
> | **Amazon Detective** | Investigate + find **root cause** of suspicious activity |
> | **Amazon Cognito** | User **sign-up/sign-in/access control** for web + mobile apps (social + SAML 2.0 providers) |
> | **AWS IAM Identity Center** | Centralized **SSO** across accounts/apps |

> [!tip] Easy mix-ups
> - **GuardDuty** = detects threats. **Inspector** = finds vulnerabilities. **Macie** = finds sensitive data. **Detective** = investigates root cause. **Security Hub** = combines them all.
> - **Shield** = DDoS (network). **WAF** = web exploits (application).

---

## 7. Compliance

> [!note] AWS engages independent auditors + certifying bodies. 3 broad categories:
> 1. **Certifications & attestations** (e.g. ISO 27001, SOC, PCI DSS).
> 2. **Laws, regulations & privacy** (e.g. GDPR, HIPAA).
> 3. **Alignments & frameworks** (industry/function-specific).

| Service | Purpose |
|---|---|
| **AWS Artifact** | On-demand access to AWS **compliance reports** + agreements (from the Console) |
| **AWS Config** | Assess/audit/evaluate resource configs + history |
| **AWS Audit Manager** | Automate evidence collection for audits |

---

## Rapid-Fire Recall

> [!question] Self-test
> - AWS responsible for? -> security **OF** the cloud (physical hardware/infra)
> - Customer responsible for? -> security **IN** the cloud (data, OS, security groups, encryption)
> - Default permission state? -> **implicit deny**; **explicit deny wins**
> - IAM Role gives? -> **temporary**, **assumable** credentials, not tied to a person
> - Is IAM Regional? -> **No, it's global** (and free)
> - Extra login layer? -> **MFA**
> - After first root login? -> **delete root access keys**
> - Who can change the support plan? -> **root user only**
> - Do SCPs grant access? -> **No** (ceiling only)
> - KMS audits configs? -> **No -> AWS Config**
> - DDoS? -> **Shield** | Web exploits? -> **WAF** | Threat detection? -> **GuardDuty** | Sensitive data in S3? -> **Macie** | Vuln scan? -> **Inspector** | Compliance reports? -> **Artifact**

**Prev:** [[03 - AWS Global Infrastructure Overview]] | **Next:** [[05 - Networking and Content Delivery]]
