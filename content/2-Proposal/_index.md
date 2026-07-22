---
title: "Project Proposal"
date: 2026-07-20
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

This proposal defines the planned direction, architecture, delivery phases, risks, cost items, and expected outcomes for the Team Task Management system. Detailed deployment commands remain in the Workshop so this section stays focused on decisions and planning.

- **Project:** Team Task Management and Automated Deadline Reminder System on AWS
- **Source code:** [GitHub – vominhthuan2004/AWSPROJECT](https://github.com/vominhthuan2004/AWSPROJECT)
- **Target users:** small teams with Manager and Member roles
- **Region:** ap-southeast-1 (Singapore)
- **Proposal structure:** executive summary, problem, architecture, implementation, timeline, budget, risks, and outcomes

## Content map

| Section | Focus |
|---|---|
| [1. Executive summary](#1-executive-summary) | Project value and high-level solution |
| [2. Problem statement](#2-problem-statement-and-solution) | Current limitations and proposed response |
| [3. Solution architecture](#3-solution-architecture) | User requests, private backend, data, and automation flows |
| [4. Technical implementation](#4-technical-implementation-plan) | Delivery phases and engineering approach |
| [5. Timeline and milestones](#5-timeline-and-milestones) | Alignment with the 12 Worklog entries |
| [6. Budget estimation](#6-budget-estimation) | Estimated AWS Credit usage for one month |
| [7. Risk assessment](#7-risk-assessment) | Technical, security, operational, and cost risks |
| [8. Expected outcomes](#8-expected-outcomes) | Verifiable project success criteria |

## 1. Executive summary

The application helps small teams create, assign, and track tasks in one place. A static frontend is delivered by Amazon S3 and CloudFront. A Node.js/Express backend runs on EC2 in a private subnet and stores data in Amazon RDS for PostgreSQL. EventBridge Scheduler invokes Lambda, which calls the deadline-check API; Amazon SES sends reminder emails. The design emphasizes baseline security, scalability, operability, and reasonable cost for a learning environment.

## 2. Problem statement and solution

Assigning work through messages or spreadsheets scatters information, makes progress difficult to follow, and increases the risk of missed deadlines. The proposed platform centralizes Manager and Member roles, task status, deadlines, email reminders, and operational logs.

## 3. Solution architecture

![Team Task Management architecture on AWS](/images/2-Proposal/team-task-management-architecture.png)

{{% notice note %}}
In the diagram, step 5 labeled “Load balanced Backend Request” means a **private backend request routed through the Internal ALB listener**. The current Target Group contains one EC2 instance, so the ALB is not being used to distribute traffic across multiple backends.
{{% /notice %}}

The architecture contains two main flows:

1. **User request flow:** Manager or Member sends an HTTPS request to CloudFront. CloudFront returns static frontend files from S3 and forwards API requests to API Gateway. VPC Link uses the internal ALB listener as the private entry point into the VPC. The ALB routes each request to the EC2 target group, which currently contains one backend instance. The backend creates, reads, updates, and deletes task data in RDS for PostgreSQL.
2. **Automated reminder flow:** EventBridge triggers Lambda on schedule. Lambda invokes the protected deadline-check API through API Gateway. The backend queries RDS for approaching deadlines and calls Amazon SES through outbound connectivity provided by the NAT Gateway. SES delivers the reminder to the user.

The AWS WAF Web ACL is associated with the CloudFront distribution to inspect and filter incoming requests before they reach the application origins.

| Layer | AWS service | Responsibility |
|---|---|---|
| Edge | AWS WAF, CloudFront | HTTPS delivery, caching, and request filtering |
| Frontend | Amazon S3 | Static web assets |
| API | API Gateway HTTP API, VPC Link | Public API entry and private integration |
| Application | Internal ALB, EC2 | Private VPC entry point, request routing, and Node.js/Express backend |
| Data | RDS for PostgreSQL | Users, tasks, assignments, status, and deadlines |
| Automation | EventBridge Scheduler, Lambda, SES | Scheduled checks and email reminders |
| Operations | IAM, Security Groups, CloudWatch | Access control, network isolation, logs, metrics, and alarms |

## 4. Technical implementation plan

1. Analyze requirements and review the architecture.
2. Develop and test the frontend, backend, and database locally.
3. Deploy networking, security, data, compute, API, frontend, and automation layers.
4. Run end-to-end tests, troubleshoot, document, and optimize cost.

## 5. Timeline and milestones

The plan follows 12 consecutive seven-day Worklog periods from 17 April to 9 July 2026: AWS foundations and labs (Weeks 1-4), networking and compute (Weeks 5-7), cost and architecture review (Weeks 8-9), team planning (Week 10), frontend development (Week 11), and backend plus AWS deployment, automation, security, and testing (Week 12). The period from 10 to 31 July is reserved for finalizing the report, obtaining confirmations, and completing submission procedures.

## 6. Budget estimation

The following estimate uses on-demand prices for `ap-southeast-1` (Singapore), checked on 20 July 2026. For planning purposes, one AWS Credit is treated as approximately USD 1 of eligible AWS service charges.

**Monthly workload assumptions:** 730 operating hours; one Linux `t3.micro` EC2 instance with 8 GB gp3 EBS; one Single-AZ PostgreSQL `db.t3.micro` instance with 20 GB gp3 storage; one NAT Gateway with one public IPv4 address and 1 GB processed data; one Internal ALB with light LCU usage; 1 GB in S3; 5 GB delivered through CloudFront; 10,000 HTTP API requests; 1,000 reminder emails; and one WAF Web ACL containing three AWS managed rule groups and one rate-based rule.

| AWS service | Monthly calculation | Estimated credits/month |
|---|---:|---:|
| EC2 | `t3.micro`: 730 h × $0.0132 | 9.64 |
| EBS | 8 GB gp3 × $0.096/GB-month | 0.77 |
| RDS for PostgreSQL | `db.t3.micro`: 730 h × $0.028 + 20 GB gp3 × $0.138 | 23.20 |
| NAT Gateway | 730 h × $0.059 + 1 GB × $0.059 | 43.13 |
| Public IPv4 | One NAT Gateway Elastic IP: 730 h × $0.005 | 3.65 |
| Internal ALB | 730 h × $0.0252 + light LCU usage | 18.40 |
| S3 | 1 GB storage and low request volume | 0.03 |
| CloudFront | About 5 GB transfer and 10,000 requests | 0.61 |
| API Gateway HTTP API | 10,000 requests × $1.00/million | 0.01 |
| Lambda and EventBridge Scheduler | About 30 scheduled invocations, within free monthly quotas | 0.00 |
| Amazon SES | 1,000 outbound emails × $0.10/1,000 | 0.10 |
| AWS WAF | One Web ACL + four rules/rule groups + 10,000 requests | 9.01 |
| CloudWatch | Under the 5 GB monthly Logs free allowance | 0.00 |
| **Estimated total** |  | **108.55 credits/month** |
| **Recommended budget with about 10% contingency** |  | **120 credits/month** |

Therefore, the project should reserve **approximately 120 AWS Credits for one month of continuous operation**. The expected usage is about **109 credits**, while the remaining 11 credits cover small traffic increases, log growth, and rounding. NAT Gateway, RDS, and Internal ALB account for roughly 78% of the estimate. Although the ALB currently serves as the private VPC entry point for API Gateway and has only one EC2 target, it is still billed for every hour that it exists.

The estimate excludes taxes, a domain name, Route 53 hosted zones, paid support, RDS snapshots beyond the included backup allowance, and unexpected cross-AZ or Internet data transfer. Promotional credits and account-specific Free Tier benefits are not subtracted; AWS applies eligible credits to the actual bill. To reduce workshop cost, delete the NAT Gateway and Internal ALB when they are not needed, stop EC2, and stop or delete the RDS database after exporting required evidence.

Pricing references: [Amazon EC2](https://aws.amazon.com/ec2/pricing/on-demand/), [Amazon RDS](https://aws.amazon.com/rds/postgresql/pricing/), [NAT Gateway and public IPv4](https://aws.amazon.com/vpc/pricing/), [Elastic Load Balancing](https://aws.amazon.com/elasticloadbalancing/pricing/), [API Gateway](https://aws.amazon.com/api-gateway/pricing/), [AWS WAF](https://aws.amazon.com/waf/pricing/), and [Amazon SES](https://aws.amazon.com/ses/pricing/).

## 7. Risk assessment

| Risk | Mitigation |
|---|---|
| Incorrect routes or security groups | Test each network hop and apply least privilege |
| API Gateway 503 or unhealthy targets | Verify routes, listener, target health, VPC Link, and logs |
| Missing IAM permissions | Use scoped IAM roles and validate CloudTrail/CloudWatch errors |
| SES Sandbox or spam filtering | Verify identities, test recipients, and check suppression/spam folders |
| Secret exposure | Keep `.env` out of Git and use roles; plan migration to Secrets Manager |
| Unexpected cost | Create AWS Budgets and delete hourly resources after the workshop |

## 8. Expected outcomes

- The HTTPS frontend and API are reachable through the intended entry points.
- EC2 and RDS remain private, while all task-management flows work correctly.
- Lambda, Scheduler, and SES deliver deadline reminders with CloudWatch logs.
- The workshop enables another learner to reproduce, test, monitor, and clean up the solution.
