---
title: "From Multi-AZ to Single-AZ – Optimizing AWS Cost"
date: 2026-07-20
weight: 1
pre: " <b> 3.1. </b> "
chapter: false
---

## Article information

| Field | Details |
| --- | --- |
| Topic | Cost optimization for an application deployed on AWS |
| Related project | Team Task Management |
| Publication platform | AWS Study Group on Facebook |
| Status | Published |
| Publication link | [View the post on Facebook](https://www.facebook.com/share/p/1DFK3ftqfj/) |

## Context

This article presents lessons from designing and deploying the **Team Task Management** project on AWS.

At first, I assumed that an architecture using more services and resembling production would be more complete. The initial design therefore used a Multi-AZ approach with public and private subnets, an Application Load Balancer, API Gateway, RDS Multi-AZ, NAT Gateway, Lambda, EventBridge, and SES.

After using AWS Pricing Calculator and monitoring the Billing Dashboard, I realized that some choices did not match the project's scale. This was a learning and demonstration application with few users, little data, and no production-level availability requirement. I therefore adjusted the architecture to match the actual workload.

## Moving from Multi-AZ to Single-AZ

Multi-AZ is appropriate for production systems that must remain available when an Availability Zone fails. For a learning environment, Single-AZ reduces cost while preserving the required application functions.

This decision taught me that a good architecture is not the one with the most resources. It is the one that meets real requirements within an appropriate budget. Before a production launch, availability, backups, and recovery requirements would need to be reviewed again.

## Moving the frontend from EC2 to S3 and CloudFront

The frontend contains only HTML, CSS, and JavaScript, so it does not require a continuously running EC2 web server. I moved it to Amazon S3 and distributed it through Amazon CloudFront. This approach:

- Removes the need to manage another web server.
- Reduces work on the backend EC2 instance.
- Delivers content over HTTPS.
- Uses CloudFront caching.
- Separates frontend and backend lifecycles.

## Using Lambda for scheduled work

Instead of maintaining a cron process on EC2 for deadline checks, I used EventBridge Scheduler and AWS Lambda:

~~~text
EventBridge Scheduler
        ↓
AWS Lambda
        ↓
Deadline-check API
        ↓
Backend queries Amazon RDS
        ↓
Amazon SES sends email
~~~

Lambda runs only on schedule and terminates after the task completes, which fits a job that executes once or a few times each day.

## Monitoring NAT Gateway cost

NAT Gateway deserves special attention because charges accrue while it exists and for data processing. I developed the following habits:

- Create a NAT Gateway only when required.
- Delete it after deployment work when it is no longer needed.
- Release unused Elastic IP addresses.
- Review the Billing Dashboard regularly.
- Check for running resources after every lab session.

## Lessons learned

- Avoid Multi-AZ in a demo environment without a high-availability requirement.
- Use S3 and CloudFront for a static frontend.
- Use Lambda for periodic tasks instead of maintaining a separate process.
- Monitor resources billed by time.
- Clearly distinguish learning architecture from production architecture.

> A good architecture is not the one with the most services; it is the one that meets the required outcome at a reasonable cost.

## References

- [AWS Pricing Calculator](https://docs.aws.amazon.com/pricing-calculator/latest/userguide/what-is-pricing-calculator.html)
- [Generate an estimate with AWS Pricing Calculator](https://docs.aws.amazon.com/pricing-calculator/latest/userguide/generate-estimate.html)
