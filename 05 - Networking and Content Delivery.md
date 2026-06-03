---
tags: [aws, cloud-foundations, clf-c02, module-5, exam, networking]
module: 5
title: Networking and Content Delivery
exam: AWS Certified Cloud Practitioner (CLF-C02)
domain: Cloud Technology and Services (34%)
---

# Module 5 - Networking and Content Delivery

> [!info] Exam context
> Maps to **CLF-C02 Domain 3: Cloud Technology and Services (34%)** with security overlap into Domain 2. The big four: **VPC structure**, **Security Groups vs NACLs**, **Route 53**, **CloudFront**.

> [!abstract] TL;DR Cram
> - **VPC** = your isolated virtual network in a **single Region**, can span **multiple AZs**. Max VPC CIDR = **/16**, smallest subnet = **/28**.
> - **5 IPs reserved per subnet** (first 4 + last 1) -> a /24 (256) gives **251 usable**.
> - **Security Groups** = instance level, **stateful**, allow-only.
> - **Network ACLs** = subnet level, **stateless**, allow + deny, numbered order.
> - **Internet Gateway** = public access (in+out); **NAT Gateway** = private subnet -> internet (**outbound only**).
> - **Route 53** = DNS; **CloudFront** = CDN (edge); **Global Accelerator** = non-HTTP over AWS backbone.

---

## 1. Networking Basics

> [!definition] Core terms
> - **Network** - 2+ machines connected to communicate; divided into **subnets**; needs a router/switch.
> - **IP Address** - unique device label. **IPv4 = 32-bit**, **IPv6 = 128-bit**.
> - **CIDR** (Classless Inter-Domain Routing) - `IP/prefix`. The number = count of leading 1-bits in the subnet mask. **Bigger number = smaller network.**

> [!example] CIDR sizing
> - `/16` = 65,536 addresses (largest VPC). `/24` = 256. `/28` = 16 (smallest subnet).
> - Each `-1` to the prefix **doubles** the addresses.
> - `192.168.100.0/22` = **1024 addresses** (192.168.100.0 -> 192.168.103.255).

### OSI Model (7 layers) - "Please Do Not Throw Sausage Pizza Away"

| # | Layer | Function | Protocols |
|---|---|---|---|
| 7 | **Application** | App access to network | HTTP(S), FTP, DHCP, LDAP |
| 6 | **Presentation** | Format + **encryption** | ASCII, ICA |
| 5 | **Session** | Orderly data exchange | NetBIOS, RPC |
| 4 | **Transport** | Host-to-host | **TCP, UDP** |
| 3 | **Network** | Routing / packet forwarding (**routers**) | **IP** |
| 2 | **Data Link** | Same-LAN transfer (switches/hubs) | **MAC** |
| 1 | **Physical** | Raw bits over a medium | Signals (1s/0s) |

---

## 2. Amazon VPC

> [!definition] Amazon VPC
> A **logically isolated** section of the AWS Cloud where you launch resources in a **virtual network you define** (IP range, subnets, route tables, gateways). Resembles a traditional data-center network with AWS scalability.

### VPC facts
- **Logically isolated** from other VPCs; **dedicated to your account**.
- Belongs to a **single Region**; can **span multiple AZs**.
- Assign an **IPv4/IPv6 CIDR block** at creation - **cannot change** the primary range afterward.
- A **default VPC** + **main route table** are created automatically.

> [!important] Memorize the CIDR limits (HIGH-YIELD)
> - **Largest** VPC CIDR = **/16** (65,536 addresses).
> - **Smallest** subnet = **/28** (16 addresses).

### Subnets
- A range of IPs dividing a VPC; lives in a **single AZ**.
- **Public** (route to internet via IGW) or **Private** (no direct internet).
- Subnet CIDRs **cannot overlap**.

> [!warning] The 5 reserved IPs (classic math question)
> AWS reserves the **first 4 + last 1** IP in every subnet:
> - `.0` network address, `.1` VPC router, `.2` DNS, `.3` reserved for future use, `.255` (last) broadcast.
> - So a **/24** = 256 total -> **251 usable**. (e.g. `10.0.1.0/24` -> **251 available**.)

### Elastic IP + ENI
- **Elastic IP** = a **static public IPv4** tied to your account; remap to another instance to mask failures.
- **Elastic Network Interface (ENI)** = virtual NIC you can detach/reattach to redirect traffic; its attributes follow it.

### Route Tables
- Control routing for a subnet; each route = **destination + target**.
- Every route table has a **local route** for VPC-internal traffic.
- Each subnet **must be associated with exactly one** route table.

> [!tip] Quiz
> Creating a new VPC creates a **main route table by default** (NOT 3 subnets, NOT an IGW).

---

## 3. VPC Connectivity Components

> [!important] Internet Gateway vs NAT Gateway (KNOW THE DIFFERENCE)
> **Internet Gateway (IGW)** - connects a VPC to the **public internet** (both directions). To make a subnet **public**: attach IGW + add a `0.0.0.0/0` route to it.
>
> **NAT Gateway** - lets **private-subnet** instances reach the internet **outbound only**; blocks unsolicited **inbound**. Lives in a **public subnet**, needs an **Elastic IP**; update private route tables to point to it. One per AZ for HA.

> [!tip] Quiz
> "Allow private-subnet resources to access the internet" -> **NAT gateway**.

### Connecting VPCs + on-premises
| Component | Purpose | Key restriction |
|---|---|---|
| **VPC Peering** | Private routing between 2 VPCs | **No overlapping CIDRs**, **no transitive peering**, one peering per pair |
| **VPC Sharing** | Share subnets with accounts in same Org | - |
| **VPC Endpoints** | Private connection to AWS services (no internet) | **Gateway** (S3, DynamoDB) vs **Interface** (PrivateLink) |
| **AWS Direct Connect** | **Dedicated private physical** line on-prem -> AWS | More bandwidth/consistency; not encrypted by default |
| **AWS VPN (Site-to-Site)** | **Encrypted tunnel** over the public internet | Cheaper, quick to set up |
| **Transit Gateway** | **Hub-and-spoke** hub interconnecting many VPCs/on-prem | Reduces connection sprawl |

> [!note] Direct Connect vs VPN
> - **Direct Connect** = dedicated private link (consistent, high bandwidth, not over internet).
> - **VPN** = encrypted tunnel **over** the internet (cheaper, variable latency).

---

## 4. VPC Security - Security Groups vs Network ACLs

> [!important] THE most-tested comparison in this module
> | | **Security Group** | **Network ACL** |
> |---|---|---|
> | Operates at | **Instance / ENI** level | **Subnet** level |
> | State | **Stateful** (return traffic auto-allowed) | **Stateless** (return must be explicitly allowed) |
> | Rules | **Allow only** | **Allow AND deny** |
> | Evaluation | **All rules** evaluated together | **In number order**, lowest number first |
> | Default behavior | Deny all inbound, allow all outbound | Default ACL: allow all; **custom ACL: deny all** until rules added |
> | Association | Attached to instances | One subnet = one NACL; one NACL = many subnets |

> [!tip] Quiz-confirmed
> - Protect an **EC2 instance** -> **Security group**.
> - **Optional** control at the **subnet** layer -> **Network ACL**.
> - "Security groups are **stateful**" -> True. "NACLs are **stateless**" -> True.

---

## 5. Amazon Route 53

> [!definition] Route 53
> A highly available, scalable **DNS** web service. Translates names (`www.example.com`) into **IP addresses**. IPv4 + IPv6. Routes to AWS **and** non-AWS infrastructure, performs **health checks**, and **registers domains**. ("53" = DNS port 53.)

### Routing policies
| Policy | Use |
|---|---|
| **Simple** | One resource / single-server |
| **Weighted (round robin)** | Split traffic by assigned weights (A/B testing) |
| **Latency** | Route to the Region with **lowest latency** |
| **Geolocation** | Route by the **user's** location |
| **Geoproximity** | Route by the **resource's** location (bias-adjustable) |
| **Failover** | Active-passive; fail over to a backup if primary unhealthy |
| **Multivalue answer** | Return up to **8 healthy** records at random |

---

## 6. Amazon CloudFront

> [!definition] CloudFront
> A fast, global, secure **CDN** (content delivery network). Caches content at **edge locations** + **regional edge caches**. Pay-as-you-go, self-service. Integrates with **Shield** + **WAF** for security.

- **Edge location** - serves **popular** cached content closest to users.
- **Regional edge cache** - caches **less-popular** content between origin and edge.
- Accelerates **static + dynamic** content; supports signed URLs/cookies, field-level encryption.

> [!tip] Cross-module link
> CloudFront low latency = **edge locations** - same fact tested in [[03 - AWS Global Infrastructure Overview#Edge Network Points of Presence]].

### CloudFront pricing (high level)
- Charged for **data transferred out** (edge -> internet/origin) + **number of HTTP(S) requests**.
- First **1,000 invalidation paths/month free**, then $0.005/path.

### Route 53 vs CloudFront vs Global Accelerator
| Service | What it is | When |
|---|---|---|
| **Route 53** | **DNS** - resolves names to IPs | Domain routing, health-based DNS |
| **CloudFront** | **CDN** - caches HTTP(S) content at edge | Websites, video, static/dynamic web content |
| **Global Accelerator** | Routes traffic over the **AWS backbone** via **anycast static IPs** | **Non-HTTP** (gaming/UDP, IoT/MQTT, VoIP), TCP/UDP apps needing static entry IPs |

---

## 7. Elastic Load Balancing (ELB) - quick note

> [!note] ELB
> Automatically **distributes incoming traffic** across multiple targets (EC2, containers, IPs, Lambda) across AZs. Types: **Application LB (HTTP/HTTPS, layer 7)**, **Network LB (TCP/UDP, layer 4)**, **Gateway LB**. Pairs with **Auto Scaling** for HA.

---

## Rapid-Fire Recall

> [!question] Self-test
> - Create a virtual network in AWS? -> **Amazon VPC**
> - Smallest subnet? -> **/28** | Largest VPC? -> **/16**
> - `/24` usable IPs? -> **251** (256 - 5 reserved)
> - 5 reserved IPs = ? -> **first 4 + last 1**
> - Private subnet -> internet? -> **NAT gateway** (outbound only)
> - Private subnets reach internet directly? -> **False**
> - Protect an EC2 instance? -> **Security group**
> - Optional subnet-layer control? -> **Network ACL**
> - SG state? -> **stateful** | NACL state? -> **stateless**
> - SG rules? -> **allow only** | NACL rules? -> **allow + deny**
> - New VPC default? -> **a main route table**
> - DNS? -> **Route 53** | CDN? -> **CloudFront** (edge locations) | non-HTTP backbone? -> **Global Accelerator**
> - Dedicated private line? -> **Direct Connect** | encrypted tunnel over internet? -> **VPN**

**Prev:** [[04 - AWS Cloud Security]] | **Back to start:** [[01 - Cloud Concepts Overview]]
