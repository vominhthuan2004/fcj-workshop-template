---
title: "Week 12 Worklog"
date: 2026-07-20
weight: 12
pre: " <b> 1.12. </b> "
chapter: false
---


**Period:** 03/07/2026 - 09/07/2026

## Week 12 objectives

- Complete the Node.js/PostgreSQL backend and deploy the private AWS architecture.
- Deliver the frontend, automation, security, monitoring, and end-to-end tests.

## Tasks carried out

| Day | Task | Start date | Completion date | Reference material |
|---|---|---|---|---|
| Fri - 03/07 | Complete backend routes, PostgreSQL access, validation, and deadline-check logic. | 03/07/2026 | 03/07/2026 | Backend code / [API Gateway HTTP APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html) |
| Sat - 04/07 | Create private RDS PostgreSQL, initialize the schema, and test the database connection. | 04/07/2026 | 04/07/2026 | Screenshot 19 / [Create an RDS DB instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html) |
| Sun - 05/07 | Launch the private EC2 backend, deploy Node.js, and configure the systemd service. | 05/07/2026 | 05/07/2026 | Screenshot 20 / [Get started with EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) |
| Mon - 06/07 | Create the Target Group and Internal ALB private entry point, then verify target health. | 06/07/2026 | 06/07/2026 | Screenshots 21-23 / [ALB target health checks](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html) |
| Tue - 07/07 | Create the VPC Link and API Gateway HTTP API, deploy the API, and test private integration. | 07/07/2026 | 07/07/2026 | Screenshots 24-28 / [HTTP API private integrations](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-develop-integrations-private.html) |
| Wed - 08/07 | Upload the frontend to S3 and CloudFront; configure SES, Lambda, and EventBridge reminders. | 08/07/2026 | 08/07/2026 | Screenshots 29-45 / [CloudFront with S3 origins](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/DownloadDistS3AndCustomOrigins.html) / [EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/getting-started.html) |
| Thu - 09/07 | Associate WAF, run end-to-end tests, review logs, document cleanup, and summarize results. | 09/07/2026 | 09/07/2026 | Screenshot 46 / [Associate a WAF Web ACL](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-associating-aws-resource.html) / [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) |

## Achievements

- Completed the Node.js backend, RDS PostgreSQL, and systemd service on private EC2; the target group reported the backend as healthy.
- Deployed the private API path through the Internal ALB, VPC Link, and API Gateway; distributed the frontend with S3 and CloudFront and tested Manager and Member flows.
- Completed the EventBridge–Lambda–SES reminder flow, enabled AWS WAF, reviewed CloudWatch logs, and ran end-to-end system testing.
