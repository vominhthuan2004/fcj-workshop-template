---
title: "Multi-account Patch Dashboard with Kiro Specs"
date: 2026-07-20
weight: 3
pre: " <b> 3.3. </b> "
chapter: false
---

## Article information

| Field | Details |
| --- | --- |
| Topic | AWS Systems Manager, Kiro, Serverless, DevOps, and compliance management |
| Type | Research based on the AWS Cloud Operations Blog |
| Publication platform | AWS Study Group on Facebook |
| Status | Published |
| Publication link | [View the post on Facebook](https://www.facebook.com/share/p/1E51kMkYdK/) |
| Research source | [Build a Multi Account Patch Compliance Dashboard with Kiro Specs](https://aws.amazon.com/blogs/mt/build-a-multi-account-patch-compliance-dashboard-with-kiro-specs/) |

## Multi-account patch management problem

Organizations with many AWS accounts need a reliable view of server patch status. AWS Systems Manager Patch Manager can scan and patch managed nodes, but manually inspecting each account becomes slow and difficult at scale.

The key problems are:

- Compliance data is distributed across accounts.
- Unpatched servers are difficult to identify quickly.
- Manual reporting takes time.
- Raw data stored in S3 can become large.
- Reading all raw files whenever the dashboard opens increases latency.
- An administrative dashboard should not be publicly exposed.

## Dashboard architecture

AWS Systems Manager Resource Data Sync centralizes compliance data in Amazon S3:

~~~text
AWS Accounts
      ↓
Systems Manager Patch Manager
      ↓
Resource Data Sync
      ↓
Amazon S3 – Raw Compliance Data
      ↓
Scheduled Cache Lambda
      ↓
Amazon S3 – Aggregated JSON Cache
      ↓
API Lambda
      ↓
Internal Application Load Balancer
      ↓
React Dashboard
~~~

## Private access with Session Manager

The dashboard has no public endpoint. Administrators connect through Systems Manager Session Manager port forwarding:

~~~text
Developer Laptop
       ↓
Systems Manager Port Forwarding
       ↓
Private EC2 Instance
       ↓
Internal Application Load Balancer
       ↓
Dashboard Application
~~~

This removes the need for a public IP or Internet-facing SSH, keeps the ALB internal, controls access through IAM, and reduces the dashboard's attack surface.

## Caching strategy

EventBridge invokes a Cache Lambda on a schedule. The function reads raw compliance data, aggregates it, and writes smaller JSON files to S3:

~~~text
EventBridge Schedule
        ↓
Cache Lambda
        ↓
Read Raw S3 Data
        ↓
Aggregate Compliance Results
        ↓
Write JSON Cache to S3
~~~

The API reads processed cache data instead of rescanning all Resource Data Sync objects whenever the dashboard opens. This reduces page-load latency and repeated processing and makes summary and detail views easier to build.

## Spec-driven development with Kiro

The article follows a structured workflow:

~~~text
Requirements
      ↓
Design
      ↓
Implementation Tasks
~~~

- **Requirements:** define compliance summaries, account lists, instance details, missing patches, and compliant-state rules.
- **Design:** describe AWS architecture, request flow, data model, IAM roles, caching, security groups, and security requirements.
- **Tasks:** divide the design into Lambda, S3, internal ALB, API, frontend, CloudFormation, testing, and security-validation work.

This process enables engineers to review requirements and architecture before AI generates implementation code.

## Steering files

Kiro uses files under **.kiro/steering/** to retain project rules and context:

~~~text
.kiro/steering/
├── architecture.md
├── data-schemas.md
├── compliance-logic.md
├── frontend-specs.md
└── security.md
~~~

They define network and service architecture, JSON schemas, compliance rules, interface behavior, and security constraints.

## Security

Because the dashboard contains security posture data, the design should:

- Enable S3 Block Public Access and encryption.
- Use an internal ALB.
- Avoid public IPs for application resources.
- Use Session Manager for access.
- Apply least-privilege IAM.
- Restrict security groups.
- Scan Infrastructure as Code and review IAM policies.
- Avoid writing sensitive data to logs.

## Connection to Team Task Management

The same specification-first mindset can support my personal project:

~~~text
project-specs/
├── architecture.md
├── database-schema.md
├── task-business-rules.md
├── api-contracts.md
├── security-requirements.md
└── testing-criteria.md
~~~

Examples include:

- **Business rules:** only Managers create and assign tasks; Members update only assigned tasks; completed tasks stop generating reminders; duplicate email is prevented for 24 hours.
- **Security requirements:** do not commit environment files or hard-code access keys; RDS accepts only the backend security group; internal endpoints require authorization; WAF protects CloudFront.
- **Testing criteria:** login succeeds; tasks persist in RDS; the target is healthy; deadline check returns HTTP 200; Lambda writes CloudWatch logs; SES delivers email.

## Lessons learned

- Do not start coding before requirements are clear.
- AI performs better with schemas, rules, and explicit acceptance criteria.
- Security must be defined during design.
- Internal dashboards do not need public Internet exposure.
- Caching improves performance and avoids repeated processing.
- Infrastructure as Code requires review and security checks.
- AI accelerates development but does not replace engineering responsibility.

> AI can help build faster, but clear specifications keep the system moving in the right direction.

{{% notice info %}}
This is a research and analysis article based on the AWS Cloud Operations Blog. I did not deploy the complete multi-account dashboard in an enterprise environment, so the scope is limited to architecture, workflow, and applicable lessons.
{{% /notice %}}
