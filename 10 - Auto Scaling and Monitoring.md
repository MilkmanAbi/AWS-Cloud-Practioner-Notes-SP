---
tags: [aws, cloud-foundations, clf-c02, module-10, exam, auto-scaling, monitoring]
module: 10
title: Auto Scaling and Monitoring
exam: AWS Certified Cloud Practitioner (CLF-C02)
domain: Cloud Technology and Services (34%)
---

# Module 10 - Auto Scaling and Monitoring

> [!info] Exam context
> Maps to **CLF-C02 Domain 3: Cloud Technology and Services (34%)**. The trio that delivers **high availability**: **ELB** (distributes traffic) + **CloudWatch** (monitors) + **Auto Scaling** (adjusts capacity). Know how they interconnect.

> [!abstract] TL;DR Cram
> - **Elastic Load Balancing (ELB):** distributes traffic across targets (EC2, containers, IPs). 3 types: **ALB** (HTTP/S, layer 7), **NLB** (TCP/UDP, layer 4), **Classic** (legacy, both layers).
> - **CloudWatch:** monitors resources + apps; collects **metrics**, triggers **alarms** -> sends **SNS** notifications or triggers Auto Scaling actions.
> - **EC2 Auto Scaling:** automatically adds/removes EC2 to maintain performance. **Launch template** (AMI + instance type + storage) + **Auto Scaling Group** (min/desired/max). 4 scaling types: **manual, scheduled, dynamic, predictive**.
> - ELB detects unhealthy -> stops routing; Auto Scaling detects unhealthy -> terminates + replaces.

---

## 1. Elastic Load Balancing (ELB)

> [!definition] ELB
> Automatically **distributes incoming traffic** across multiple targets in one or more AZs. Scales the load balancer as traffic changes. Monitored via CloudWatch, access logs, and CloudTrail.

### The 3 Load Balancer Types

> [!important] Know the table
> | | **Application LB (ALB)** | **Network LB (NLB)** | **Classic LB** (legacy) |
> |---|---|---|---|
> | Layer | **Layer 7** (Application) | **Layer 4** (Transport) | Both L4 + L7 |
> | Protocols | **HTTP / HTTPS** | **TCP / UDP / TLS** | HTTP, HTTPS, TCP, SSL |
> | Routing | By **content** of request (path, host, headers) | By **IP protocol data** | Across EC2 instances |
> | Best for | Modern apps, **microservices, containers** | **Extreme performance**, volatile traffic, static IP | Legacy apps |
> | Targets | Registered in **target groups** | Registered in **target groups** | Instances registered directly |

> [!note] Key ELB behaviors
> - **Health checks** — ELB periodically pings targets. If unhealthy:
>   1. **Stops routing** traffic to the unhealthy target.
>   2. **Resumes routing** when it detects the target is healthy again.
>   3. **Routes traffic** to remaining healthy targets in the meantime.

> [!tip] Quiz-confirmed
> - What must be configured on an ELB to expect incoming traffic? -> **A listener** (NOT a port, NOT a network interface, NOT an instance).
> - Tools for scaling based on demand? -> **Amazon EC2 Auto Scaling** + **Elastic Load Balancing**.
> - When ELB detects unhealthy? -> Stops routing, resumes when healthy, routes to healthy targets (NOT "triggers an alarm", NOT "requires manual restart").

---

## 2. Amazon CloudWatch

> [!definition] CloudWatch
> A **monitoring and observability** service for AWS resources + applications. Collects **metrics**, sets **alarms**, and can trigger **automated actions**.

### Core CloudWatch capabilities
- **Monitors:** AWS resources (EC2, RDS, S3, Lambda) + custom metrics.
- **Collects + tracks:** Standard metrics (CPU, disk, network) + custom metrics.
- **Alarms:** Based on static thresholds, anomaly detection, or metric math.
  - When an alarm triggers, it can: send an **SNS notification**, perform an **EC2 action** (stop/terminate/reboot), or trigger **Auto Scaling** (scale out/in).
- **Events / EventBridge:** Define rules matching changes in AWS environment -> route to targets for processing.
- **Dashboards:** Visualize metrics in real-time.
- **Logs:** Collect, monitor, and analyze log files.

### CloudWatch monitoring intervals
- **Basic monitoring** = free, metrics every **5 minutes**.
- **Detailed monitoring** = paid, metrics every **1 minute**.

> [!tip] Quiz-confirmed
> - Send alerts based on CloudWatch alarms? -> **Amazon SNS** (Simple Notification Service).
> - Collect important metrics from RDS + EC2? -> **Amazon CloudWatch** (NOT CloudFront, NOT CloudSearch, NOT CloudTrail).

### CloudWatch vs CloudTrail (don't confuse)
| **CloudWatch** | **CloudTrail** |
|---|---|
| **Performance metrics** (is it healthy?) | **API activity logs** (who did what?) |
| Alarms + dashboards | Audit + governance |
| "What's happening right now?" | "What happened / who called what?" |

> [!tip] Quiz-confirmed
> - Audit trail of all access to AWS resources? -> **AWS CloudTrail** (NOT CloudWatch).

---

## 3. Amazon EC2 Auto Scaling

> [!definition] EC2 Auto Scaling
> Automatically **adds or removes** EC2 instances to maintain steady, predictable performance at the **lowest possible cost**. Detects + replaces **impaired** instances without your intervention.

### Two key components

> [!important] Launch Template/Config + Auto Scaling Group

**Launch Template** (or legacy Launch Configuration) — the **blueprint** for instances:
- **AMI** (which image)
- **Instance type** (which size)
- **Security groups**
- **Key pair**
- **EBS volumes**
- (Templates also support multiple instance types, Spot, versioning — preferred over Launch Configurations.)

**Auto Scaling Group (ASG)** — the **fleet controller**:
- **Minimum size** — lowest number of instances (never goes below).
- **Desired capacity** — the target number Auto Scaling maintains.
- **Maximum size** — upper limit (never exceeds).
- ASG launches/terminates instances to keep running count = desired.

> [!tip] Quiz-confirmed
> - Elements of an ASG? -> **Minimum size, desired capacity, maximum size** (NOT health checks as an element — those are configured separately).
> - Launch configuration elements? -> **AMI, instance type, EBS volumes** (NOT load balancer, NOT VPC/subnets — those go in the ASG).
> - Auto Scaling characteristics? -> **Responds to conditions by adding/removing instances** + **Launches from a specified AMI** + **Enforces minimum running instances** (NOT "only supports dynamic scaling", NOT "delivers push notifications").

### Scaling Types

> [!example] 4 scaling approaches
> 1. **Manual** — you change the desired capacity by hand.
> 2. **Scheduled** — scale at specific times (e.g. ramp up at 8 AM every weekday).
> 3. **Dynamic (on-demand)** — respond to **CloudWatch alarms** in real time.
>    - **Target tracking** — keep a metric at a target (e.g. CPU at 50%).
>    - **Step scaling** — add/remove in steps based on alarm breach magnitude.
>    - **Simple scaling** — add/remove based on a single alarm.
> 4. **Predictive** — uses ML to forecast demand and pre-scale.

### Terminology
- **Scale out** = **launch** instances (add capacity).
- **Scale in** = **terminate** instances (reduce capacity).

---

## 4. How ELB + CloudWatch + Auto Scaling Work Together

> [!important] The HA trinity (understand the flow)
> ```
> User traffic
>     |
>     v
> [Elastic Load Balancer] -- distributes traffic across AZs
>     |          |
>     v          v
>  [EC2-A]    [EC2-B]    ...managed by [Auto Scaling Group]
>     |          |
>  metrics    metrics
>     \        /
>      v      v
>   [CloudWatch] -- monitors CPU, requests, etc.
>       |
>       v
>   Alarm triggers -> Auto Scaling scales out/in
>                  -> SNS sends notification
> ```
>
> 1. **ELB** distributes incoming requests across healthy instances.
> 2. **CloudWatch** monitors metrics (CPU, request count, etc.).
> 3. When a metric crosses a **threshold**, CloudWatch triggers an **alarm**.
> 4. The alarm invokes an **Auto Scaling policy** to **scale out** (add) or **scale in** (remove).
> 5. New instances register with **ELB** automatically.
> 6. **ELB health checks** detect unhealthy instances -> stop routing to them.
> 7. **Auto Scaling health checks** detect unhealthy instances -> terminate + replace them.

---

## Rapid-Fire Recall

> [!question] Self-test
> - Distributes traffic? -> **Elastic Load Balancing**
> - HTTP/S routing by content, layer 7? -> **ALB** | TCP/UDP, layer 4, extreme perf? -> **NLB**
> - ELB requires which config to accept traffic? -> **A listener**
> - Monitors AWS resources + metrics? -> **CloudWatch**
> - Send alert from CloudWatch alarm? -> **Amazon SNS**
> - API audit log (who did what)? -> **CloudTrail** (NOT CloudWatch)
> - Automatically add/remove EC2? -> **EC2 Auto Scaling**
> - ASG three capacity settings? -> **Minimum, desired, maximum**
> - Scale out = ? -> launch instances | Scale in = ? -> terminate instances
> - Launch template includes? -> AMI, instance type, EBS, security groups, key pair
> - 4 scaling types? -> Manual, Scheduled, Dynamic, Predictive
> - ELB detects unhealthy -> ? -> stops routing, resumes when healthy
> - Auto Scaling detects unhealthy -> ? -> terminates + replaces

**Prev:** [[09 - Cloud Architecture]] | **Back to start:** [[01 - Cloud Concepts Overview]]
