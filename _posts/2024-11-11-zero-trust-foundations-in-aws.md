---
title: 'Zero Trust Foundations in AWS'
author: bradley
date: 2024-11-11 20:00:00 +/-0400
categories: [AWS, Security]
tags: [aws, security, zero-trust, cloud] 
---

# Zero Trust Foundations in AWS

Zero Trust has become a major buzzword in cybersecurity, but what exactly does it mean? At its core, Zero Trust is a security strategy based on the principle that no user, device, or component is automatically trusted. Every request, whether internal or external, is continuously verified to prevent unauthorized access.

A few years ago, I attended an AWS re:Invent
talk where a company presented an impressive Zero Trust model for AWS. While their implementation was effective, it involved a massive, complex engineering effort. This can intimidate those looking to secure their AWS environments without overwhelming resources or dedicated teams.
Instead, let’s break down Zero Trust into practical, manageable steps. This approach allows you to build robust security into your existing architecture without overhauling your entire infrastructure.

## Start with Identity and Access Management (IAM)
Your users are your primary risk vector, so tackling IAM is crucial. To begin, consider moving away from traditional IAM users and leveraging AWS Identity Center (formerly AWS SSO) for single sign-on (SSO). By using Identity Center, users access resources with temporary credentials instead of long-lived access keys, which reduces the risk of leaks. Plus, managing users centrally means you can enforce consistent policies across multiple accounts without dealing with individual password rotations.
With Identity Center, you can also implement boundaries, such as IP restrictions, and limit access to specific services, tightening control over user permissions.

## Role Management: Apply Least Privilege Principles
Roles should follow least privilege principles and be created via infrastructure-as-code (IaC). This approach lets you enforce policies from the start and ensures roles are scoped to the specific permissions required for each task. When testing, invest time in identifying the minimal permissions each role actually needs to operate.
You may also discover existing roles in your environment that are overly permissive. To address this, use tools like IAM Access Analyzer to identify excessive permissions and gradually lock down access. In larger environments, you may need a custom solution to process and review Access Analyzer findings efficiently.

## Implement Service Control Policies (SCPs)
Service Control Policies (SCPs) allow you to enforce organization-wide restrictions, preventing access to sensitive actions and resources. Begin with high-level policies targeting high-risk actions, then expand to more granular policies for critical assets. SCPs can protect resources like logs, backups, and production databases, which, if compromised, could have severe consequences for your organization. Remember: while applications can be rebuilt, lost or exposed data can be irreplaceable.

## Tighten Security Group Rules
Security groups act as virtual firewalls controlling network traffic between resources. By creating security groups via IaC, you can standardize configurations and ensure consistency. Separate production applications into distinct subnets and configure security groups with the smallest possible ingress access.
AWS security groups allow you to reference other resources’ security groups as inbound rules, a useful strategy as long as security groups are dedicated to specific resources. This technique minimizes risk by restricting access to only the essential resources.
For egress, identify what outbound connections are essential. Since security groups are stateful, inbound traffic can flow back out without an explicit egress rule, so limit outbound rules to only those needed for originating connections. By default, egress is open—lock this down as tightly as possible.

## Emphasize Monitoring and Continuous Compliance
Monitoring is essential to maintaining Zero Trust. Continuously review IAM roles and policies against your established baselines. Monitor CloudTrail logs to track access attempts and resource interactions, and keep an eye on application and database logs for any unusual activity. Network traffic monitoring, with baseline patterns, can help detect compliance issues and anomalies, adding another layer of security.

---

These steps won’t get you to a complete Zero Trust environment, but they are a solid foundation for securing your AWS accounts. Once you have the resources and experience, you can consider more advanced Zero Trust architectures to further enhance your company’s security posture.
Best of luck on your security journey!