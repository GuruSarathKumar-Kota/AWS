# 💰 AWS Cost Optimization Activities (Interview Ready)

[![Amazon EC2](https://img.shields.io/badge/Amazon-EC2-orange?logo=amazon-aws)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)  
[![Amazon EBS](https://img.shields.io/badge/Amazon-EBS-blue?logo=amazon-aws)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html)  
[![AWS Lambda](https://img.shields.io/badge/AWS-Lambda-yellow?logo=amazon-aws)](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)  
[![AWS Boto3](https://img.shields.io/badge/Boto3-Python-green?logo=python)](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)  

---

## 📑 Table of Contents
- [⚡ Example 1: Deleting Unused EBS Volumes](#-example-1-deleting-unused-ebs-volumes)
- [⚡ Example 2: Migrating GP2 → GP3](#-example-2-migrating-gp2--gp3)
- [⚡ Example 3: EC2 Rightsizing & Scheduling](#-example-3-ec2-rightsizing--scheduling)
- [⚡ Example 4: S3 Lifecycle Policies & Glacier](#-example-4-s3-lifecycle-policies--glacier)
- [💡 Interview-Ready Answer](#-interview-ready-answer)

---

## ⚡ Example 1: Deleting Unused EBS Volumes
- Problem: Developers created EBS volumes but many remained unused → **monthly storage cost kept growing**.  
- Action:
  - First used `aws ec2 describe-volumes` via CLI.  
  - Later automated using **Lambda + Boto3**.  
  - Scheduled the Lambda to run **every 4th Friday** → detects and deletes unused volumes.  
- Result: **Significant cost reduction** and **no manual intervention needed**.  

---

## ⚡ Example 2: Migrating GP2 → GP3
- Problem: Old **GP2 volumes** were still running, costing more 💸.  
- Action:
  - Wrote a **Lambda (Python, Boto3)** to identify GP2 volumes.  
  - Used API calls to seamlessly upgrade GP2 → GP3.  
- Benefit:
  - **Lower cost** per GB.  
  - **Higher IOPS** at no additional cost.  
- Result: Improved **performance** + **cost savings**.  

---

## ⚡ Example 3: EC2 Rightsizing & Scheduling
- Problem: Many **EC2 instances were oversized** (t3.large/t3.xlarge) running at low CPU utilization.  
- Action:
  - Used **CloudWatch metrics** to identify low-utilization instances (<10% CPU).  
  - Recommended **rightsizing to t3.medium/t3.small**.  
  - Implemented **Instance Scheduler** → stopped non-prod EC2s during off-hours.  
- Result: Saved **30-40% on EC2 costs** across dev/test environments.  

---

## ⚡ Example 4: S3 Lifecycle Policies & Glacier
- Problem: Large S3 buckets with **stale log files** consuming high-cost S3 Standard storage.  
- Action:
  - Applied **S3 Lifecycle Policies** to move old logs → **Glacier / S3 IA**.  
  - Deleted data older than 1 year not needed for compliance.  
- Result: Cut **storage cost by >50%** while retaining compliance.  

---

## 💡 Interview-Ready Answer

Cost optimization is part of my regular responsibilities as a DevOps engineer. Recently, I worked on **automating unused EBS volume cleanup**. Over time, developers created many EBS volumes that remained unattached or unused. Initially, I identified them using the AWS CLI (`aws ec2 describe-volumes`), but to make it sustainable, I created a **Lambda function using Python (Boto3)** that scans for unused volumes and deletes them automatically. The Lambda runs periodically (every 4th Friday), which reduced significant costs without manual effort.  

In another activity, I migrated older **GP2 volumes to GP3**, which reduced cost per GB while improving performance. Beyond EBS, I have also optimized **EC2 instances via rightsizing and scheduling**, and **applied S3 lifecycle rules to move old logs to Glacier**.  

This shows that I not only solve cost issues immediately but also build **automated, long-term solutions** that continuously optimize costs.  
