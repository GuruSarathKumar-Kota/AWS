# 🚀 Auto Scaling Group (ASG) Not Launching EC2 Instances  

![ASG](https://img.shields.io/badge/Docs-Auto_Scaling_Group-orange?logo=amazonaws&link=https://docs.aws.amazon.com/autoscaling/index.html)  
![EC2](https://img.shields.io/badge/Docs-EC2-blue?logo=amazonaws&link=https://docs.aws.amazon.com/ec2/index.html)  
![IAM](https://img.shields.io/badge/Docs-IAM-yellow?logo=amazonaws&link=https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)  
![CloudTrail](https://img.shields.io/badge/Docs-CloudTrail-green?logo=amazonaws&link=https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)  

---

## 📑 Table of Contents
- [🎯 Interview-Ready Answer](#-interview-ready-answer)
- [🖼️ ASCII Architecture Diagram](#️-ascii-architecture-diagram)
- [✅ Closing Line](#-closing-line)

---

## 🎯 Interview-Ready Answer  

If my Auto Scaling Group (ASG) is not launching EC2 instances, I’ll troubleshoot step by step:  

1. **Check Launch Template / Configuration**  
   - Ensure the AMI ID is valid and available in the region.  
   - Verify the instance type exists in that AZ and is not retired.  
   - Check if key pair, security group, and user data are valid.  

2. **Check Subnets & Availability Zones**  
   - Confirm the ASG has healthy subnets mapped to it.  
   - Sometimes a subnet has no capacity (for example, spot capacity exhausted or AZ-specific limits).  

3. **Check Resource Quotas**  
   - I’ll check EC2 quotas for the instance type. If the quota is reached, new instances can’t launch.  

4. **Check Spot vs On-Demand**  
   - If the ASG is configured with **spot instances**, they may not be available → leading to launch failures.  

5. **Check IAM Permissions**  
   - The ASG or Launch Template needs an IAM role to launch instances.  
   - If the role’s policy was modified or restricted, the ASG won’t be able to create new EC2 instances.  

6. **Check Scaling Policies / Health Checks**  
   - Sometimes instances launch and immediately terminate if health checks (ELB/ALB) fail.  
   - I’ll confirm scaling policies are not restricting desired/min/max size incorrectly.  

7. **Check CloudTrail / ASG Activity History**  
   - ASG logs activity (e.g., “failed to launch due to …”).  
   - CloudTrail events for `RunInstances` will confirm if the launch attempt failed due to quota, AZ capacity, or permissions.  

---

## 🖼️ ASCII Architecture Diagram  

                                     🌍 AWS Cloud
                                          │
                                          ▼
                              ┌───────────────────────────┐
                              │   Auto Scaling Group 🔄   │
                              └───────────┬──────────────┘
                                          │
                          ┌───────────────┼────────────────┐
                          ▼               ▼                ▼
                     📦 Launch       🗄️ Subnets &      🔑 IAM Role
                     Template        AZ Mappings         (Permissions)
                     (AMI/Type)      (Capacity)         
                          │               │                │
                          └───────────────┼────────────────┘
                                          ▼
                                 ⚡ EC2 Instances 🚀
                                          │
                                          ▼
                                🩺 Health Checks (ALB/ELB)  
                                          │
                                          ▼
                                 📊 CloudTrail / Logs  

---

### ✨ Notes:

Launch Template → Defines AMI, Instance Type, User Data.

Subnets/AZ → Must have capacity.

IAM Role → Needs ec2:RunInstances.

CloudTrail → Verify RunInstances calls.

Health Checks → Prevent infinite launch/terminate loops.

### ✅ Closing Line
So in short, I’d start with launch template validity, then verify subnets/AZ capacity and quotas, check if spot capacity issues exist, and finally ensure IAM and health checks are correct.
This systematic approach usually helps identify why the ASG is failing to launch EC2 instances.
