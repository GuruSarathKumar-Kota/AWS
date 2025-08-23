# 📂 AWS EFS (Elastic File System) 

![EFS](https://img.shields.io/badge/Docs-EFS-blue?logo=amazonaws&link=https://docs.aws.amazon.com/efs/)  
![EC2](https://img.shields.io/badge/Docs-EC2-orange?logo=amazonaws&link=https://docs.aws.amazon.com/ec2/)  
![IAM](https://img.shields.io/badge/Docs-IAM-yellow?logo=amazonaws&link=https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)  
![NFS](https://img.shields.io/badge/Protocol-NFS-green)  

---

## 📑 Table of Contents  
- [📘 What is EFS?](#-what-is-efs)  
- [⚡ Real Use Case](#-real-use-case)  
- [🐞 Issue Encountered](#-issue-encountered)  
- [🔧 Troubleshooting Steps](#-troubleshooting-steps)  
- [✅ Resolution](#-resolution)  
- [🖼️ ASCII Architecture Diagram](#️-ascii-architecture-diagram)  
- [🎯 Interview-Ready Summary](#-interview-ready-summary)  

---

## 📘 What is EFS?  
- **Amazon Elastic File System (EFS)** is a **scalable, serverless, shared file storage** service.  
- Unlike **EBS (block storage, one-to-one attachment)**, EFS supports **concurrent access across multiple EC2 instances**.  
- Built on **NFS protocol**, making it ideal for shared configuration files, application data, or content repositories.  
- Key features:  
  - Auto-scaling storage 📈  
  - Serverless management 🔄  
  - Highly available across multiple AZs 🌍  

---

## ⚡ Real Use Case  
- Used **EFS** to share **data and configuration files** across multiple EC2 instances in different Availability Zones.  
- Requirement: **Concurrent reads/writes** from multiple instances without manual sync.  

---

## 🐞 Issue Encountered  
- Faced an issue: **"EFS Mount Target Unreachable"** 🚨 while trying to mount the EFS volume.  

---

## 🔧 Troubleshooting Steps  
1. **Checked Mount Targets**  
   - Ensured EFS had **mount targets in all Availability Zones** where EC2s were deployed.  

2. **Verified Security Group Rules**  
   - Confirmed that **port 2049 (NFS)** was open for inbound traffic in EFS security group. 🔐  

3. **DNS Resolution & Network**  
   - Verified **VPC DNS resolution** was enabled.  
   - Checked NACLs to confirm no traffic was being blocked.  

4. **IAM Policy Validation**  
   - Looked at **EFS Access Point IAM policy** to confirm correct permissions for mounting.  

---

## ✅ Resolution  
- Found the **IAM policy** attached to the EFS access point was restrictive.  
- Updated the IAM permissions → after that, EC2 instances could mount EFS successfully.  
- Issue fixed ✔️  

---

## 🖼️ ASCII Architecture Diagram  

                                   🌍 AWS Cloud
                                        │
                                        ▼
                              ┌────────────────────┐
                              │   Amazon EFS 📂    │
                              └───┬───────────┬────┘
                                  │           │
                   ┌──────────────┘           └──────────────┐
                   ▼                                       ▼
                 EC2 Instance A 🖥️                     EC2 Instance B 🖥️
                 (AZ1)                                   (AZ2)
                   │                                       │
                   │────── NFS Mount (Port 2049) ──────────│
                   │                                       │
                   ▼                                       ▼
                 Shared Config / Data 🔄 (Concurrent Access)

---

### ✨ Key Notes:

EFS → Mount Targets in each AZ.

EC2 → EFS via NFS Port 2049.

IAM Access Points regulate permissions.

### 🎯 Interview-Ready Summary
“In my experience, Amazon EFS is used for shared storage across multiple EC2 instances. I once faced an issue where EFS mount was failing with ‘mount target unreachable’. I troubleshot step by step: first verifying mount targets across AZs, then security group inbound rules for NFS (2049), checked DNS resolution and NACLs, and finally discovered the root cause was an IAM policy issue with the EFS access point. Once I updated the IAM permissions, the mount worked fine. This taught me the importance of validating networking + IAM together when working with EFS.”
