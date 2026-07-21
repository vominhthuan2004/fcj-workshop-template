---
title: "Week 5 Worklog"
date: 2026-07-20
weight: 5
pre: " <b> 1.5. </b> "
chapter: false
---


**Period:** 15/05/2026 - 21/05/2026

## Week 5 objectives

- Understand VPC components and CIDR planning.
- Trace how public and private resources reach the internet.

## Tasks carried out

| Day | Task | Start date | Completion date | Reference material |
|---|---|---|---|---|
| Fri - 15/05 | Study Module 03 and identify the roles of VPC, subnet, route table, and security group. | 15/05/2026 | 15/05/2026 | Module 03 / [Amazon VPC basics](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-subnet-basics.html) |
| Sat - 16/05 | Review how shared responsibility applies to VPC configuration. | 16/05/2026 | 16/05/2026 | [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/) |
| Sun - 17/05 | Review IPv4 CIDR calculation and plan non-overlapping public, application, and database subnets. | 17/05/2026 | 17/05/2026 | [VPC IP addressing](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html) |
| Mon - 18/05 | Compare Security Groups and network ACLs and identify where each is applied. | 18/05/2026 | 18/05/2026 | [VPC infrastructure security](https://docs.aws.amazon.com/vpc/latest/userguide/infrastructure-security.html) |
| Tue - 19/05 | Analyze Internet Gateway and NAT Gateway traffic flows for public and private subnets. | 19/05/2026 | 19/05/2026 | [Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) / [NAT Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/nat-gateway-basics.html) |
| Wed - 20/05 | Validate the planned CIDR ranges and confirm that subnets do not overlap. | 20/05/2026 | 20/05/2026 | CIDR worksheet / [VPC IP addressing](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html) |
| Thu - 21/05 | Draw a draft VPC flow and list security boundaries for EC2 and RDS. | 21/05/2026 | 21/05/2026 | Architecture draft / [Security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html) |

## Achievements

- Prepared a non-overlapping CIDR plan for the public, private application, and private database subnets.
- Distinguished the roles of security groups, network ACLs, Internet Gateway, and NAT Gateway in public and private traffic flows.
- Completed a draft network flow and identified access boundaries between the ALB, EC2 backend, and RDS.
