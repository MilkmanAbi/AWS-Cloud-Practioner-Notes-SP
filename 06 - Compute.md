---
tags: [aws, cloud-foundations, clf-c02, module-6, exam, compute]
module: 6
title: Compute
exam: AWS Certified Cloud Practitioner (CLF-C02)
domain: Cloud Technology and Services (34%)
---

# Module 6 - Compute

> [!info] Exam context
> Maps to **CLF-C02 Domain 3: Cloud Technology and Services (34%)**. Heavy focus on **EC2** (instance types, AMIs, pricing, the 9 launch decisions), **Lambda** (serverless), **Elastic Beanstalk** (PaaS), and **containers (ECS/EKS/Fargate)**.

> [!abstract] TL;DR Cram
> - **EC2** = resizable virtual servers. 9 launch decisions (AMI, type, network, IAM, user data, storage, tags, security group, key pair).
> - **Instance naming:** `t3.large` = family(T) + generation(3) + size(large). **5 families:** General Purpose, Compute Optimized, Memory Optimized, Storage Optimized, Accelerated Computing.
> - **AMI** = template (root volume + launch permissions + block device mapping).
> - **Pricing:** On-Demand, Reserved (AURI/PURI/NURI), Spot (up to 90%, interruptible), Dedicated, Savings Plans.
> - **Lambda** = serverless, event-driven, max **15 min**, max **10 GB** memory, pay per request + duration.
> - **Elastic Beanstalk** = PaaS, deploy code -> AWS handles the rest.
> - **Containers:** Docker on ECS/EKS; **Fargate** = serverless containers (no server management).

---

## 1. Compute Services Overview

AWS offers many compute services — choose based on **application design**, **usage patterns**, and **configuration needs**.

| Service | Model | Key characteristic |
|---|---|---|
| **Amazon EC2** | IaaS | Full control over virtual servers |
| **AWS Lambda** | Serverless (FaaS) | No servers, event-driven, auto-scales |
| **Elastic Beanstalk** | PaaS | Upload code, AWS handles infra |
| **Amazon ECS** | Containers | Docker orchestration |
| **Amazon EKS** | Containers (K8s) | Managed Kubernetes |
| **AWS Fargate** | Serverless containers | No cluster management |
| **Amazon ECR** | Container registry | Store/manage Docker images |
| **Amazon Lightsail** | Simple VPS | Easy entry-level virtual servers |

---

## 2. Amazon EC2 (Elastic Compute Cloud)

> [!definition] EC2
> Provides **virtual machines (instances)** in the cloud. Full control over the **guest OS** (Linux/Windows). Launch anywhere, ready in **minutes**. Resizable compute capacity.

### The 9 Launch Decisions

> [!important] Know all 9 steps (quiz-confirmed)
> 1. **Select an AMI** (Amazon Machine Image)
>    - Template with OS + pre-installed software.
>    - Sources: **Quick Start** (AWS-provided), **My AMIs**, **AWS Marketplace** (3rd-party), **Community AMIs** (use at own risk).
>    - Contains: **root volume template** + **launch permissions** + **block device mapping**.
> 2. **Select an Instance Type** (CPU/RAM/storage/network profile)
> 3. **Specify Network Settings** (VPC, subnet, public IP)
> 4. **Attach IAM Role** (optional - lets EC2 talk to other AWS services)
> 5. **User Data Script** (optional - runs on **first boot** to customize)
> 6. **Specify Storage** (root volume + additional EBS/instance store)
> 7. **Add Tags** (key-value metadata for filtering, billing, access control)
> 8. **Configure Security Group** (firewall rules - see [[05 - Networking and Content Delivery#VPC Security - Security Groups vs Network ACLs]])
> 9. **Identify/Create Key Pair** (public key on AWS + private key you keep for SSH/RDP)

> [!tip] Quiz-confirmed
> - What must be specified at launch? -> **AMI** + **instance type**.
> - What's in an AMI? -> **All of the above** (root volume template, launch permissions, block device mapping).

### Instance Type Naming Convention

> [!example] Reading `t3.large`
> - **t** = family (burstable, general purpose)
> - **3** = generation (higher = newer hardware, better price/perf)
> - **large** = size (nano < micro < small < medium < large < xlarge < 2xlarge...)

### The 5 Instance Families

> [!important] Memorize the families + letter hints
> | Family | Letter(s) | Optimized for | Use case |
> |---|---|---|---|
> | **General Purpose** | T, M, A | Balanced CPU/memory/network | Web servers, dev, small DBs |
> | **Compute Optimized** | C | High-performance CPUs | Batch, HPC, ML inference, gaming |
> | **Memory Optimized** | R, X, z | Large in-memory datasets | In-memory DBs, real-time analytics |
> | **Storage Optimized** | I, D, H | High sequential read/write to local storage | Data warehousing, log processing |
> | **Accelerated Computing** | P, G, Inf, Trn | GPU/FPGA hardware | ML training, video encoding, 3D rendering |

---

## 3. EC2 Cost Optimization

### Pricing Models (revisited with compute context)
See [[02 - Cloud Economics and Billing#EC2 Pricing Models know all five]] for the full pricing breakdown.

> [!tip] Quiz-confirmed scenarios
> - Why is AWS more economical for varying workloads? -> **EC2 instances can be launched on-demand when needed**.
> - Monthly batch reports over large data? -> **Scheduled Reserved Instances**.
> - Long-term predictable workload? -> **Reserved Instances**.
> - 4 steady + 8 spike on last day of month? -> **4 Reserved + 8 On-Demand** (RI for base, OD for spike).
> - Instance not sharing physical hardware? -> **Dedicated Instances**.

### Four Pillars of EC2 Cost Optimization

> [!example] The 4 pillars
> 1. **Right-size** - match instance to actual CPU/memory/storage need (use **CloudWatch** to check idle %). Best practice: **right-size first, then reserve**.
> 2. **Increase elasticity** - stop/hibernate when not in use; use Auto Scaling.
> 3. **Optimal pricing model** - mix RI (steady) + On-Demand (variable) + Spot (fault-tolerant). Consider **Lambda** for event-driven.
> 4. **Optimize storage choices** - resize EBS, change volume types, delete unneeded snapshots.

---

## 4. Containers on AWS

> [!definition] Containers
> A method of **OS-level virtualization** — lighter than VMs, **don't contain an entire OS**, share the host kernel. Repeatable, self-contained, faster to start/stop.

> [!warning] Quiz-confirmed
> "Containers contain an entire OS" -> **FALSE**. They are smaller than VMs and share the host OS kernel.

### Docker
- Platform to **build, test, deploy** apps quickly.
- Containers are created from **images** (templates).

### Container Services
| Service | What it does |
|---|---|
| **Amazon ECS** | Managed **Docker** container orchestration (AWS-native) |
| **Amazon EKS** | Managed **Kubernetes** (open-source, portable) |
| **AWS Fargate** | **Serverless** compute for ECS/EKS - no cluster management |
| **Amazon ECR** | **Docker container registry** - store, manage, deploy images |

> [!note] ECS cluster backing
> - Want to manage the cluster? -> ECS backed by **EC2** (more control).
> - Don't want to manage? -> ECS backed by **Fargate** (easier, focus on apps).

---

## 5. AWS Lambda (Serverless)

> [!definition] Lambda
> **Serverless compute** - upload code, it runs **only when triggered** by an event. No servers to provision/manage. Auto-scales. **Built-in fault tolerance.** Pay-per-use.

### Key facts and limits
| Property | Value |
|---|---|
| Max memory | **10,240 MB** (10 GB) - note: v2 slides may say 3,008 MB (old limit, raised in 2020) |
| Max execution time | **15 minutes** (900 seconds) - hard limit |
| Deployment package | 250 MB unzipped (including layers) |
| Pricing | Per **request** + per **duration** (GB-seconds) |
| Free tier | 1M requests + 400,000 GB-seconds/month |
| Languages | Python, Node.js, Java, Go, .NET, Ruby, custom runtimes |

> [!tip] Quiz-confirmed
> - Serverless compute service? -> **AWS Lambda**.
> - Lambda is event-driven — an **event source** (S3 upload, API Gateway request, DynamoDB stream, schedule) triggers the function.

---

## 6. AWS Elastic Beanstalk (PaaS)

> [!definition] Elastic Beanstalk
> An easy way to deploy/scale **web applications**. Upload your code and it automatically handles **provisioning, deployment, load balancing, auto-scaling, health monitoring, logging**. **No additional charge** - you pay only for underlying resources.

> [!tip] Quiz-confirmed
> - Service that enables easy deployment + management of apps in the cloud? -> **AWS Elastic Beanstalk** (NOT ECS, NOT OpsWorks, NOT CloudFormation).

> [!note] Beanstalk vs CloudFormation
> - **Beanstalk** = PaaS for **web apps** (you give code, it deploys).
> - **CloudFormation** = IaC (Infrastructure as Code) - provision any AWS resource via **templates** (JSON/YAML). More powerful, more manual.

---

## Rapid-Fire Recall

> [!question] Self-test
> - Virtual servers in the cloud? -> **EC2**
> - What's in an AMI? -> root volume template + launch permissions + block device mapping
> - Instance naming `c5.xlarge`? -> Compute optimized, gen 5, xlarge
> - 5 instance families? -> General Purpose, Compute, Memory, Storage, Accelerated
> - Serverless compute? -> **Lambda** (max 15 min, max 10 GB, event-driven)
> - PaaS deploy-your-code? -> **Elastic Beanstalk**
> - Containers without an OS? -> **True** (they share host kernel)
> - Serverless containers? -> **Fargate**
> - Docker registry? -> **ECR** | Docker orchestration? -> **ECS** | Kubernetes? -> **EKS**
> - Steady base + spike traffic? -> **Reserved + On-Demand**
> - Right-size first, then? -> **Reserve**

**Prev:** [[05 - Networking and Content Delivery]] | **Next:** [[07 - Storage]]
