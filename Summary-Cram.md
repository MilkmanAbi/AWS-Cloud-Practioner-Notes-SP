---
tags: [aws, cloud-foundations, exam, final-cram]
title: FINAL CRAM - One Pager (All 10 Modules)
---

# AWS Cloud Foundations - FINAL CRAM

> [!danger] 75 Q · MAQ+MCQ · 2 hrs. Read "NOT/EXCEPT" twice. Eliminate, don't guess blind.

## M1 · Concepts
**Cloud** = on-demand, pay-as-you-go, hardware→software. **AWS owns hardware**, you provision.
**Models:** IaaS>PaaS>SaaS (control, high→low). **Deploy:** Cloud / Hybrid / On-prem.
**6 Advantages:** CapEx→OpEx · economies of scale · stop guessing capacity · speed/agility · stop running DCs · go global in minutes.
**Well-Architected 6 pillars:** Ops Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, **Sustainability**(newest).
**CAF 6 perspectives:** Business/People/Governance (biz) + Platform/Security/Operations (tech).
Access AWS: **Console, CLI, SDK**.

## M2 · Economics & Billing
**3 cost drivers:** Compute, Storage, Data Transfer (out=$, in=free).
**RI discount: AURI>PURI>NURI** (All/Partial/No upfront). Full upfront NOT required.
**EC2 pricing:** On-Demand(no commit) · Reserved(1-3yr) · Savings Plans(flexible) · **Spot(≤90% off, 2-min warning)** · Dedicated Host.
**Free services:** VPC, Beanstalk, Auto Scaling, CloudFormation, IAM.
**TCO** = compare on-prem vs AWS cost.
**AWS Organizations** = consolidated billing + volume discount + **SCPs (ceiling, never grants)**.
Tools: **Budgets**=alerts · **Cost Explorer**=visualize past · **CUR**=most detailed.
**Support:** Basic(no case)/Developer(email)/Business(24/7)/Enterprise(**designated TAM**, 15-min critical).

## M3 · Global Infrastructure
**Region→AZ→Data Center.** 1 DC = 1 AZ only. Region ≥3 AZs (v2 slides: "2+").
Choose Region: **compliance, latency, availability, cost**.
**CloudFront = edge locations** (low latency, 700+, way more than Regions).
**Outposts**=on-prem AWS · **Wavelength**=5G edge · **Local Zones**=metro low-latency.
**Config**=configs · **CloudTrail**=API logs · **CloudWatch**=metrics/monitoring.

## M4 · Security
**Shared Responsibility: AWS = OF the cloud (hardware); Customer = IN the cloud (data, OS, SG, encryption).**
**IAM** = global+free. **Implicit deny** default; **explicit deny always wins**.
User(programmatic=keys / console=pw+MFA) · Group(no nesting) · **Role**(temp, assumable) · Policy(JSON).
Root: use minimally, **delete access keys**, only root changes support plan.
**KMS**=encryption keys (≠Config!) · **Shield**=DDoS · **WAF**=web exploits · **GuardDuty**=threat detection · **Inspector**=vuln scan · **Macie**=sensitive data in S3 · **Artifact**=compliance reports.
Data at rest→KMS; in transit→TLS/HTTPS/ACM.

## M5 · Networking
**VPC**: 1 Region, multi-AZ. Max **/16**, min subnet **/28**. **5 reserved IPs**/subnet → /24=**251 usable**.
**Security Group**=instance,**stateful**,allow-only. **NACL**=subnet,**stateless**,allow+deny,numbered.
**IGW**=public both ways. **NAT Gateway**=private→internet outbound-only.
**Route 53**=DNS. **CloudFront**=CDN(edge locations). **Direct Connect**=dedicated line. **VPN**=encrypted tunnel over internet.

## M6 · Compute
EC2 launch: **AMI + instance type** required (+network,IAM,userdata,storage,tags,SG,key pair).
AMI = root vol template + launch permissions + block device mapping.
Naming `t3.large`=family+gen+size. **5 families:** General(T,M) / Compute(C) / Memory(R,X) / Storage(I,D) / Accelerated(P,G).
**Lambda**=serverless,event-driven,max **15min**,max **10GB** mem, pay/request+duration.
**Beanstalk**=PaaS(upload code). **Fargate**=serverless containers. **ECS**=Docker orchestration. **EKS**=Kubernetes. **ECR**=registry.
Containers ≠ full OS. Steady+spike → **Reserved+On-Demand**. Cost pillars: right-size→reserve, elasticity, pricing model, storage.

## M7 · Storage
**Block**(EBS,same-AZ,persistent,snapshot) / **Object**(S3,11-9s durability,5TB max,**private default**,globally-unique bucket name) / **File**(EFS,NFS,Linux-only,shared concurrent,petabyte scale) / **Archive**(Glacier,vault=container).
S3 replicates **within Region, multi-AZ**. Instance Store = ephemeral (lost on stop); EBS ≠ lost.
**S3 classes** (hi→lo cost): Standard→Intelligent-Tiering→Standard-IA(30d min)→One Zone-IA(30d)→Glacier Instant(90d)→Glacier Flexible(90d,mins-12hr)→**Deep Archive**(180d,12-48hr,**cheapest**).
Lifecycle = one-way only (can't go backward).

## M8 · Databases
**RDS**=managed relational(6 engines: Aurora/MySQL/PostgreSQL/MariaDB/Oracle/SQLServer). Multi-AZ=**HA**(sync,failover). Read replica=**perf**(async). Auto-patch+backup=TRUE.
**Aurora**=MySQL/PgSQL-compat,5x perf,15 read replicas,3-AZ.
**DynamoDB**=NoSQL,key-value/document,single-digit ms,flexible schema,unlimited scale. Query=by partition key; **Scan**=by non-key attribute.
**Redshift**=petabyte warehouse,SQL,BI/analytics,columnar.
Decision: complex transactions→**RDS/Aurora** · fast+flexible/session state→**DynamoDB** · SQL analytics→**Redshift**.

## M9 · Cloud Architecture
**"Assume everything will fail"** = core design principle.
**Reliability**=functionality when desired(MTBF=MTTF+MTTR). **Availability**=uptime%(9s). **Fault tolerance**=stays up via redundancy. **High availability**=min downtime+no human intervention.
Well-Architected Perf Efficiency principles: serverless, democratize tech, go global, experiment, mechanical sympathy (NOT traceability—that's Security).
**Trusted Advisor 5 categories: Cost, Performance, Security, Fault Tolerance, Service Limits.**

## M10 · Auto Scaling & Monitoring
**ELB**: **ALB**=HTTP/S,L7,content-based · **NLB**=TCP/UDP,L4,extreme perf · needs a **listener**.
ELB unhealthy→stop routing→resume when healthy. Auto Scaling unhealthy→terminate+replace.
**CloudWatch**=metrics/alarms→triggers **SNS** or Auto Scaling (≠CloudTrail=API logs).
**ASG** = min+desired+max. **Launch template**=AMI+type+storage+SG+key pair.
Scale **out**=launch. Scale **in**=terminate. 4 types: manual/scheduled/dynamic/predictive.

---
> [!tip] Pattern-match shortcuts
> "Serverless" → Lambda/Fargate/DynamoDB/S3 · "Cheapest archival" → Deep Archive · "Fastest EC2 discount, no commit needed" → Spot · "HA database" → Multi-AZ · "Faster reads" → Read Replica · "DDoS" → Shield · "Web exploit" → WAF · "Threat detection" → GuardDuty · "PII in S3" → Macie · "Audit who-did-what" → CloudTrail · "Is it healthy" → CloudWatch · "DNS" → Route53 · "CDN" → CloudFront · "Private subnet→internet" → NAT GW · "Public subnet→internet" → IGW
