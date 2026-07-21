---
title: "Week 7 Worklog"
date: 2026-07-20
weight: 7
pre: " <b> 1.7. </b> "
chapter: false
---


**Period:** 29/05/2026 - 04/06/2026

## Week 7 objectives

- Understand EC2 storage and metadata.
- Prepare secure administration of a private backend instance.

## Tasks carried out

| Day | Task | Start date | Completion date | Reference material |
|---|---|---|---|---|
| Fri - 29/05 | Compare EC2 instance families, purchasing options, and the planned t3.micro demo instance. | 29/05/2026 | 29/05/2026 | [Amazon EC2 instance types](https://docs.aws.amazon.com/ec2/latest/instancetypes/instance-types.html) |
| Sat - 30/05 | Review EC2 security groups, IAM roles, and network access controls. | 30/05/2026 | 30/05/2026 | [IAM roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) |
| Sun - 31/05 | Study instance store, instance metadata, and their security implications. | 31/05/2026 | 31/05/2026 | [EC2 instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) |
| Mon - 01/06 | Attach a temporary EBS volume to a lab instance and inspect its lifecycle. | 01/06/2026 | 01/06/2026 | EBS lab / [Attach an EBS volume](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-attaching-volume.html) |
| Tue - 02/06 | Study EBS volume types, attachment, snapshots, and lifecycle behavior. | 02/06/2026 | 02/06/2026 | [Amazon EBS volumes](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volumes.html) / [EBS snapshots](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-snapshots.html) |
| Wed - 03/06 | Compare Session Manager with SSH for administering a private instance. | 03/06/2026 | 03/06/2026 | [AWS Systems Manager Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html) |
| Thu - 04/06 | Create the EC2 IAM role for Session Manager and review private-instance access. | 04/06/2026 | 04/06/2026 | Screenshots 16-18 / [Instance permissions for Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-instance-permissions.html) |

## Achievements

- Selected EC2 `t3.micro` for the demonstration environment and understood the security implications of instance metadata.
- Practiced attaching an EBS volume, observed its lifecycle, and studied snapshots and major volume types.
- Created the EC2 IAM role and identified Session Manager as a safer private-instance administration method than exposing SSH publicly.
