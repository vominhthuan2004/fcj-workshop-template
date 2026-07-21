---
title: "FCAJ Community Day – May 2026"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

## Event information

| Item | Details |
| --- | --- |
| Event | FCAJ Community Day |
| Date | 23 May 2026 |
| Format | Knowledge-sharing event about Cloud, AI, and career development |
| Role | Attendee |
| Main topics | Cloud, AI, Prompt Engineering, Amazon CloudFront, hackathons, and engineering responsibility in the GenAI era |
| Evidence | Event video and attendance photo |

## Attendance photo

<figure style="margin: 1rem auto; max-width: 820px; text-align: center;">
  <img src="/images/4-EventParticipated/fcaj-community-day-may-2026.jpg"
       alt="Group photo at FCAJ Community Day on 23 May 2026"
       style="display: block; width: 100%; height: auto; border-radius: 6px;">
  <figcaption style="margin-top: 0.5rem; font-style: italic;">
    Figure 1: Group photo at FCAJ Community Day on 23 May 2026.
  </figcaption>
</figure>

## Overview

FCAJ Community Day on 23 May 2026 brought together the technology community around Cloud, AI, system architecture, and career development.


## The technology paradox and essential engineering skills

In the opening session, **Nguyen Gia Hung**, a Solutions Architect at AWS Vietnam and Founder of First Cloud Journey, discussed a paradox that often appears as technology advances.

When the cost of using a technology falls, demand may increase rather than decrease. LED lighting, for example, reduces energy consumption per device but encourages lighting to be used more widely. Similarly, AI reduces software development time and cost, creating greater demand for digital products.

The message I recorded was that engineers still need strong foundations. Academic knowledge matters, but learners must also understand real business problems, identify the right use cases, and build working demonstrations.

## AI and Prompt Engineering

Another session focused on interacting effectively with AI by providing clear context. Instead of asking a general question, users should define:

- The objective.
- The role the AI should perform.
- Relevant data and context.
- Constraints.
- The desired output format.

Providing sufficient information can make the result more accurate and relevant. The session showed me that Prompt Engineering is not simply writing a longer question; it is the structured organization of requirements, input data, and output criteria.

## Amazon CloudFront, security, and cost control

In the Amazon CloudFront session, **Nguyen Han Thinh**, a DevOps Engineer, introduced **flat-rate pricing** as a way for businesses to forecast cost more easily and reduce the risk of unexpected bill increases. He also discussed security features including **mTLS** and **VPC Origin**.

## Lessons from a 36-hour hackathon

A group of speakers shared their experience in a 36-hour hackathon, including idea formation, AI token limits, and prioritizing core product functions as a **Minimum Viable Product (MVP)**.

They also discussed comparing the performance of models available through Amazon Bedrock with models running locally.

## Engineering responsibility in the GenAI era

The final discussion considered a business credit-assessment system and the responsibility of engineers when AI makes an incorrect decision.

Two questions were emphasized:

> Who uses the system?

> Why do they use it?

These questions help ensure that a solution is built for the actual need.

## My personal takeaways

Based on the event, I personally concluded that:

- Strong technical foundations remain important in the AI era.
- I should begin projects with real problems and user needs.
- I need to provide clear context when using AI.
- When time is limited, I should prioritize core functions and an MVP.
- In my view, AI-assisted output should be checked before it is applied.
- I believe engineers should consider how their systems affect users.

These are my reflections after the event, not direct quotations or claims attributed to the speakers.

## My connection to the Team Task Management project

I can relate the CloudFront session to my project architecture: the frontend is stored on Amazon S3 and distributed through CloudFront, while API requests are routed to Amazon API Gateway. This describes my implementation and is not presented as a technical claim from the event summary.

The hackathon experience helped me prioritize the project's core functions:

- Login.
- Task creation.
- Task assignment.
- Status updates.
- Deadline reminder emails.

Advanced features should be added only after the main workflow is stable.

Prompt Engineering also helps me structure requests when using AI for debugging, architecture review, and documentation. As a personal practice, I verify technical guidance against official AWS documentation and practical test results.
