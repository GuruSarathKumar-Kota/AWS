# ⚡ Troubleshooting Unexpected EC2 Instance Termination

[![AWS EC2](https://img.shields.io/badge/AWS-EC2-orange?logo=amazon-aws)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)  
[![AWS CloudTrail](https://img.shields.io/badge/AWS-CloudTrail-blue?logo=amazon-aws)](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)  
[![AWS Auto Scaling](https://img.shields.io/badge/AWS-Auto_Scaling-green?logo=amazon-aws)](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)  
[![AWS Spot Instances](https://img.shields.io/badge/AWS-Spot_Instances-purple?logo=amazon-aws)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html)  

---

## 📑 Table of Contents
- [The Interview Question](#the-interview-question)
- [Step 1: Check CloudTrail Logs](#step-1-check-cloudtrail-logs)
- [Step 2: Verify Auto Scaling Group Activity](#step-2-verify-auto-scaling-group-activity)
- [Step 3: Check if it was a Spot Instance](#step-3-check-if-it-was-a-spot-instance)
- [Step 4: Investigate Lifecycle Policies / Automation](#step-4-investigate-lifecycle-policies--automation)
- [ASCII Troubleshooting Flow](#ascii-troubleshooting-flow)
- [Interview-Ready Answer](#interview-ready-answer)

---

## ❓ The Interview Question

👉 *“One of your important EC2 instances terminated unexpectedly. How will you troubleshoot the issue?”*  

---

## 🔍 Step 1: Check CloudTrail Logs
- Navigate to **CloudTrail**.  
- Search for the **`TerminateInstances` API call**.  
- This call shows **who/what terminated the instance**.  
- Possible sources:  
  - Auto Scaling policy  
  - Lifecycle hook  
  - Manual user termination  
  - Automation script  

---

## 📈 Step 2: Verify Auto Scaling Group Activity
- If the instance is part of an **Auto Scaling Group**, check:  
  - Scaling policies  
  - Health checks (EC2 vs ELB-based)  
  - Scheduled scaling actions  

---

## 💰 Step 3: Check if it was a Spot Instance
- Spot instances can be **terminated by AWS** when:  
  - Spot price > bid price  
  - AWS needs the capacity  
- Critical workloads should avoid Spot Instances.  

---

## ⚙️ Step 4: Investigate Lifecycle Policies / Automation
- Check if the instance was terminated by:  
  - **Lifecycle policies** (e.g., AMI cleanup)  
  - **Automation scripts** (e.g., Ansible/Terraform destroy)  
  - **CloudWatch alarms** triggering termination  

---

## 🗺️ ASCII Troubleshooting Flow

                                             ⚡ EC2 Instance Terminated
                                                          │
                           ┌──────────────────────────────┼─────────────────────────────┐
                           │                              │                             │
                           ▼                              ▼                             ▼
                     🔍 CloudTrail Logs          📈 Auto Scaling Group        💰 Spot Instance Check
                     (TerminateInstances)         Activity History             Pricing / Capacity
                           │                              │                             │
                           ▼                              ▼                             ▼
                     ⚙️ Automation / Scripts        ✅ Scaling Policy             ❌ Critical Apps
                     (Lifecycle Hooks, IaC)           Misconfigured?              should not use Spot

---

### 💡 Interview-Ready Answer
If an EC2 instance terminates unexpectedly, the first step is to check CloudTrail logs for the TerminateInstances API call, which reveals whether the termination was triggered by an Auto Scaling policy, lifecycle hook, automation script, or a manual action. Next, I would verify if the instance was part of an Auto Scaling Group, since scaling policies or health checks can cause automatic termination. Another important check is whether the instance was a Spot Instance, as these can be evicted anytime based on pricing or capacity, making them unsuitable for critical workloads. Finally, I would investigate any lifecycle policies or automation tools like Ansible, Terraform, or cleanup scripts that might have triggered termination. This structured approach ensures we identify the root cause quickly and prevent recurrence.
