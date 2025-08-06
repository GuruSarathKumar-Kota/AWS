# 🔒 AWS Security Services Overview  

[![AWS Security](https://img.shields.io/badge/AWS-Security%20Services-232F3E?logo=amazon-aws&logoColor=white)](https://aws.amazon.com/security/)
[![Compliance](https://img.shields.io/badge/Compliance-Ready-green?logo=trustpilot)](https://aws.amazon.com/compliance/)
[![Threat Detection](https://img.shields.io/badge/Threat%20Detection-GuardDuty-blue?logo=amazon-aws)](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html)

---

## 📑 Table of Contents  
- [Introduction](#introduction)  
- [AWS Identity and Access Management (IAM)](#1-aws-identity-and-access-management-iam)  
- [Amazon GuardDuty](#2-amazon-guardduty)  
- [AWS WAF](#3-aws-waf-web-application-firewall)  
- [Amazon Inspector](#4-amazon-inspector)  
- [AWS CloudTrail](#5-aws-cloudtrail)  
- [AWS Key Management Service (KMS)](#6-aws-key-management-service-kms)  
- [Amazon Macie](#7-amazon-macie)  
- [AWS Security Hub](#8-aws-security-hub)  
- [Architecture Diagram](#-aws-security-architecture-diagram)  
- [Summary](#summary)  

---

## Introduction  

AWS provides a variety of security services to help you secure your cloud resources and meet compliance requirements. Below are some of the **key security services**:  

---

## 1. **AWS Identity and Access Management (IAM)**  
[![Docs](https://img.shields.io/badge/IAM-Docs-blue?logo=read-the-docs)](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)  

IAM enables you to manage access to AWS services and resources securely.  
You can create and manage **users, groups, and roles** and define permissions to allow or deny access.  

---

## 2. **Amazon GuardDuty**  
[![Docs](https://img.shields.io/badge/GuardDuty-Docs-blue?logo=read-the-docs)](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html)  

GuardDuty is a **threat detection service** that continuously monitors for malicious activity and unauthorized behavior in your AWS account by analyzing logs and network traffic.  

---

## 3. **AWS WAF (Web Application Firewall)**  
[![Docs](https://img.shields.io/badge/AWS%20WAF-Docs-blue?logo=read-the-docs)](https://docs.aws.amazon.com/waf/latest/developerguide/what-is-aws-waf.html)  

AWS WAF helps protect your **web applications from exploits**.  
You can create rules to allow, block, or monitor web requests based on custom conditions.  

---

## 4. **Amazon Inspector**  
[![Docs](https://img.shields.io/badge/Amazon%20Inspector-Docs-blue?logo=read-the-docs)](https://docs.aws.amazon.com/inspector/latest/user/what-is-inspector.html)  

Inspector automatically **assesses applications for vulnerabilities** and provides remediation recommendations to improve compliance and security posture.  

---

## 5. **AWS CloudTrail**  
[![Docs](https://img.shields.io/badge/AWS%20CloudTrail-Docs-blue?logo=read-the-docs)](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)  

CloudTrail provides a **record of actions** taken by users, services, and resources in your AWS account.  
Helps in **auditing, troubleshooting, and security analysis**.  

---

## 6. **AWS Key Management Service (KMS)**  
[![Docs](https://img.shields.io/badge/KMS-Docs-blue?logo=read-the-docs)](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)  

KMS is a managed service that makes it easy to **create and control encryption keys** used to secure data at rest and in transit.  

---

## 7. **Amazon Macie**  
[![Docs](https://img.shields.io/badge/Amazon%20Macie-Docs-blue?logo=read-the-docs)](https://docs.aws.amazon.com/macie/latest/user/what-is-macie.html)  

Macie uses **machine learning** to automatically discover, classify, and protect sensitive data stored in AWS.  

---

## 8. **AWS Security Hub**  
[![Docs](https://img.shields.io/badge/Security%20Hub-Docs-blue?logo=read-the-docs)](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html)  

Security Hub provides a **centralized dashboard** to aggregate, organize, and prioritize findings from multiple AWS services and third‑party tools.  

---

## 🎨 **AWS Security Architecture Diagram**

                              ┌─────────────────────────┐
                              │       👤 Users          │
                              └───────────┬────────────┘
                                          │
                          ┌───────────────┼────────────────┐
                          │                                   │
                 ┌────────▼────────┐                 ┌───────▼──────────┐
                 │🔑 IAM (Access)  │                 │🛡️ WAF (Web Sec) │
                 └────────┬────────┘                 └────────┬─────────┘
                          │                                     │
                 ┌────────▼──────────┐                ┌────────▼─────────┐
                 │🔍 GuardDuty       │                │🔎 Inspector     │
                 └────────┬──────────┘                └────────┬─────────┘
                          │                                     │
             ┌────────────▼──────────────┐           ┌──────────▼─────────┐
             │🧾 CloudTrail (Auditing)   │           │🔐 KMS (Encryption) │
             └────────────┬──────────────┘           └──────────┬─────────┘
                          │                                     │
              ┌───────────▼─────────────┐            ┌──────────▼─────────┐
              │🤖 Macie (Data Discovery)│            │📊 Security Hub    │
              └─────────────────────────┘            └─────────────────────┘

---

### Summary
These are just a few examples of the security services offered by AWS.
AWS provides a comprehensive ecosystem to secure your cloud environment, protect sensitive data, and ensure compliance with industry standards.
