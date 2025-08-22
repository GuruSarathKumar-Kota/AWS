# 🔗 Applications in Different Subnets of a VPC – AWS Interview Notes

[![AWS VPC](https://img.shields.io/badge/AWS-VPC-orange?logo=amazon-aws)](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
[![AWS Subnets](https://img.shields.io/badge/AWS-Subnets-blue?logo=amazon-aws)](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)
[![AWS Route Tables](https://img.shields.io/badge/AWS-Route_Tables-green?logo=amazon-aws)](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
[![AWS NACL](https://img.shields.io/badge/AWS-Network_ACLs-yellow?logo=amazon-aws)](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)
[![AWS Security Groups](https://img.shields.io/badge/AWS-Security_Groups-purple?logo=amazon-aws)](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)

---

## 📑 Table of Contents
- [The Question](#the-question)
- [Scenario Setup](#scenario-setup)
- [Answer](#answer)
- [Reason](#reason)
- [ASCII Architecture Diagram](#ascii-architecture-diagram)
- [Key Points](#key-points)

---

## ❓ The Question

Applications in different subnets of the VPC interact by default.  
If no, why? If yes, why?

---

## 🏗️ Scenario Setup

- You have created a **VPC** with CIDR range: `10.0.0.0/16`.  
- Within this VPC, you created two subnets:  
  - **Subnet A**: `10.0.1.0/24`  
  - **Subnet B**: `10.0.2.0/24`  

Now,  
- An **application is deployed in Subnet A**.  
- Another **application is deployed in Subnet B**.  

👉 Question: Can the applications interact (e.g., ping from one to the other) by default in this plain vanilla setup?

---

## ✅ Answer

Yes — by default, **applications within different subnets of the same VPC can interact with each other**.  

---

## 🔎 Reason

- When you create a VPC, AWS automatically creates a **main route table**.  
- This route table contains a **default local route**:  

Destination: 10.0.0.0/16
Target: local


- This means:  
- All subnets inside the VPC (A & B) can communicate with each other.  
- No additional configuration is required.  
- This is the **default behavior of AWS and most cloud providers**:  
- Communication is enabled **within the VPC**.  
- External communication is disabled by default.  

Of course, you can restrict communication using:  
- **Network ACLs (NACLs)** at the subnet level.  
- **Security Groups** at the instance level.  

But by default, AWS allows subnet-to-subnet communication inside the same VPC.  

---

## 🗺️ ASCII Architecture Diagram

                                🌐 VPC: 10.0.0.0/16
                       ┌─────────────────────────────────┐
                       │        Main Route Table          │
                       │ Destination: 10.0.0.0/16         │
                       │ Target: local ✅                 │
                       └─────────────────────────────────┘
                                  ▲               ▲
                                  │               │
                        ┌─────────┘               └─────────┐
                        │                                   │
                🏠 Subnet A (10.0.1.0/24)          🏠 Subnet B (10.0.2.0/24)
                        │                                   │
                   🖥️ App Instance A                  🖥️ App Instance B
                        │                                   │
                        └────────────── 🔁 Communicate 🔁 ─┘

---

---
📌 Key Points
✔️ By default, instances in different subnets of the same VPC can communicate.

✔️ This is because of the local route in the main route table.

🔒 Restrictions can be applied via Security Groups or NACLs.

❌ No external communication is possible unless an Internet Gateway or NAT Gateway is configured.

---
