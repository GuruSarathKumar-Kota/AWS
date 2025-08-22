# 🔐 AWS Network ACLs vs Security Groups

[![AWS VPC](https://img.shields.io/badge/AWS-VPC-orange?logo=amazon-aws)](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
[![AWS NACL](https://img.shields.io/badge/AWS-Network_ACLs-blue?logo=amazon-aws)](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)
[![AWS Security Groups](https://img.shields.io/badge/AWS-Security_Groups-green?logo=amazon-aws)](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)
[![AWS EC2](https://img.shields.io/badge/AWS-EC2-purple?logo=amazon-aws)](https://docs.aws.amazon.com/ec2/index.html)

---

## 📑 Table of Contents
- [The Question](#the-question)
- [Default Behavior in VPC](#default-behavior-in-vpc)
- [What is a Network ACL (NACL)?](#what-is-a-network-acl-nacl)
- [What is a Security Group?](#what-is-a-security-group)
- [Key Difference: Stateless vs Stateful](#key-difference-stateless-vs-stateful)
- [Real-Time Use Cases](#real-time-use-cases)
- [ASCII Architecture Diagram](#ascii-architecture-diagram)
- [Interview-Ready Answer](#interview-ready-answer)

---

## ❓ The Question

👉 *“Explain the difference between a Network ACL and a Security Group. Which one do you use in your organization?”*  

This is a **basic AWS interview question** that almost always comes up.

---

## 🌐 Default Behavior in VPC

- By default, when you create a **VPC**, AWS allows communication between subnets.  
- Instances in different subnets can talk to each other because of the **main route table’s local route**.  
- To restrict or control access:  
  - Use **Network ACLs (NACLs)** → subnet-level  
  - Use **Security Groups (SGs)** → instance-level  

---

## 🔒 What is a Network ACL (NACL)?

- Operates at the **subnet level**.  
- Default behavior: **DENY all traffic** until rules are added.  
- Rules can be configured to:  
  - Allow traffic on specific ports.  
  - Deny traffic on specific ports.  
- You can even add a **Deny-All rule** to completely block a subnet.  

📌 Example from real organization use case:  
- A **database subnet** where access was restricted from other subnets using a NACL.  

---

## 🛡️ What is a Security Group?

- Operates at the **instance level**.  
- Used when **different instances in the same subnet need different rules**.  
- Example:  
  - App A → Port 80 exposed  
  - App B → Port 80 not exposed  
- SGs are flexible because they attach directly to **EC2 instances**.  

---

## ⚖️ Key Difference: Stateless vs Stateful

- **NACLs → Stateless**
  - Must explicitly allow **both inbound & outbound** traffic.  
  - Example: If you allow inbound HTTP (port 80), you must also allow outbound HTTP.  

- **Security Groups → Stateful**
  - If you allow inbound on port 80, the response traffic is **automatically allowed outbound**.  
  - You don’t need to define both directions.  

---

## 🏢 Real-Time Use Cases

- **Only NACL:**  
  - To restrict all access at the subnet level (e.g., DB subnet).  
- **Only SG:**  
  - To apply specific rules per instance (e.g., some apps expose HTTP, others don’t).  
- **NACL + SG Combination:**  
  - Example:  
    - NACL allows HTTP at subnet level.  
    - SGs used to selectively allow/deny HTTP on individual instances.  

---

## 🗺️ Architecture Diagram

                                           🌐 VPC
                        ┌────────────────────────────────────────────┐
                        │            Main Route Table (local)         │
                        └────────────────────────────────────────────┘
                                          ▲             ▲
                                          │             │
                                ┌─────────┘             └─────────┐
                                │                                 │
                         🏠 Subnet A                        🏠 Subnet B
                       (with NACL rules)                  (with NACL rules)
                                │                                 │
                        ┌──────────────┐                  ┌──────────────┐
                        │ EC2 Instance │                  │ EC2 Instance │
                        │  App Server  │                  │  DB Server   │
                        └──────────────┘                  └──────────────┘
                                │                                 │
                        🛡️ Security Group                  🛡️ Security Group
                           (Port 80 ✅)                       (Port 80 ❌)

---

---
### 💡 Interview-Ready Answer
In AWS, NACLs operate at the subnet level and are stateless, meaning you must explicitly allow inbound and outbound rules for traffic to flow. They’re useful when you want to apply rules across an entire subnet, for example restricting access to a database subnet. Security Groups, on the other hand, operate at the instance level and are stateful, meaning if you allow inbound on a port, the response traffic is automatically allowed outbound. These are best used when different instances within the same subnet require different access rules.

👉 In my organization, we use a combination of NACLs and Security Groups. For example, we allow HTTP traffic at the subnet level with NACL, but then use Security Groups to selectively allow or deny HTTP on specific instances. This gives us both broad subnet-level control and fine-grained instance-level security.
