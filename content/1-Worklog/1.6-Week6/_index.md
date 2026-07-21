---
title: "Week 6 Worklog"
date: 2026-07-20
weight: 6
pre: " <b> 1.6. </b> "
chapter: false
---


**Period:** 22/05/2026 - 28/05/2026

## Week 6 objectives

- Build the project VPC and subnet layout.
- Configure public and private routing for later deployment.

## Tasks carried out

| Day | Task | Start date | Completion date | Reference material |
|---|---|---|---|---|
| Fri - 22/05 | Create the project VPC and enable the required DNS settings. | 22/05/2026 | 22/05/2026 | Deployment document / [VPC DNS attributes](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html) |
| Sat - 23/05 | Create the public subnets and verify their Availability Zones and CIDR ranges. | 23/05/2026 | 23/05/2026 | Screenshots 01-02 / [Subnets for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html) |
| Sun - 24/05 | Create private application and database subnets for EC2 and RDS. | 24/05/2026 | 24/05/2026 | Screenshots 03-05 / [Subnets for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html) |
| Mon - 25/05 | Create initial ALB, backend, and database security groups with source-based rules. | 25/05/2026 | 25/05/2026 | Screenshots 14-15 / [Security group rules](https://docs.aws.amazon.com/vpc/latest/userguide/security-group-rules.html) |
| Tue - 26/05 | Create and attach the Internet Gateway; configure the public route table. | 26/05/2026 | 26/05/2026 | Screenshots 06-10 / [Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) |
| Wed - 27/05 | Apply consistent tags and review VPC resources before configuring private routes. | 27/05/2026 | 27/05/2026 | VPC inventory / [Tag AWS resources](https://docs.aws.amazon.com/tag-editor/latest/userguide/tagging.html) |
| Thu - 28/05 | Create the NAT Gateway and private route table, then verify the outbound route. | 28/05/2026 | 28/05/2026 | Screenshots 11-13 / [NAT Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/nat-gateway-basics.html) |

## Achievements

- Built the project VPC with DNS settings and public, private application, and private database subnets based on the CIDR plan.
- Configured the Internet Gateway and public route table and the NAT Gateway and private route table for the intended traffic paths.
- Created initial security groups for the Internal ALB, EC2, and RDS and applied consistent tags to VPC resources.
