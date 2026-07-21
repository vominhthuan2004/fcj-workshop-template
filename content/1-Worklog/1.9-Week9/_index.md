---
title: "Week 9 Worklog"
date: 2026-07-20
weight: 9
pre: " <b> 1.9. </b> "
chapter: false
---


**Period:** 12/06/2026 - 18/06/2026

## Week 9 objectives

- Define the Manager/Member business flow.
- Create and refine the AWS architecture with community feedback.

## Tasks carried out

| Day | Task | Start date | Completion date | Reference material |
|---|---|---|---|---|
| Fri - 12/06 | Analyze task-management problems, user roles, task states, and deadline-reminder requirements. | 12/06/2026 | 12/06/2026 | Requirement notes / [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) |
| Sat - 13/06 | Draw the first architecture covering CloudFront, S3, API Gateway, VPC Link, ALB, EC2, RDS, Lambda, and SES. | 13/06/2026 | 13/06/2026 | Architecture diagram / [AWS Architecture Center](https://aws.amazon.com/architecture/) |
| Sun - 14/06 | List failure scenarios including unhealthy targets, IAM errors, and SES Sandbox limits. | 14/06/2026 | 14/06/2026 | [ALB health checks](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html) / [SES Sandbox](https://docs.aws.amazon.com/ses/latest/dg/request-production-access.html) |
| Mon - 15/06 | Request architecture feedback from members of the AWS FCAJ community. | 15/06/2026 | 15/06/2026 | Review notes / [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) |
| Tue - 16/06 | Review service responsibilities and trace user and automated request flows. | 16/06/2026 | 16/06/2026 | Flow review / [AWS Architecture Center](https://aws.amazon.com/architecture/) |
| Wed - 17/06 | Revise subnet placement, security-group relationships, private API access, and automation flow. | 17/06/2026 | 17/06/2026 | [API Gateway private integrations](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-develop-integrations-private.html) / [Security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html) |
| Thu - 18/06 | Finalize the reviewed architecture for implementation planning. | 18/06/2026 | 18/06/2026 | Final diagram / [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) |

## Achievements

- Defined the Team Task Management Manager/Member roles, task statuses, and automated deadline-reminder requirements.
- Completed an architecture diagram covering CloudFront, S3, API Gateway, VPC Link, the Internal ALB, EC2, RDS, Lambda, EventBridge, and SES.
- Incorporated feedback from AWS FCAJ community mentors and revised subnet placement, security groups, private API access, and the two main request flows.
