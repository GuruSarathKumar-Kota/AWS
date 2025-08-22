# 🌐 AWS NAT Gateway

[![AWS VPC](https://img.shields.io/badge/AWS-VPC-orange?logo=amazon-aws)](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
[![AWS NAT Gateway](https://img.shields.io/badge/AWS-NAT_Gateway-blue?logo=amazon-aws)](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)
[![AWS Route Tables](https://img.shields.io/badge/AWS-Route_Tables-green?logo=amazon-aws)](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
[![AWS Subnets](https://img.shields.io/badge/AWS-Subnets-yellow?logo=amazon-aws)](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)
[![OSI Model](https://img.shields.io/badge/Networking-OSI_Layers-purple)](https://en.wikipedia.org/wiki/OSI_model)

---

## 📑 Table of Contents
- [What is AWS NAT and when is it used?](#what-is-aws-nat-and-when-is-it-used)
- [Detailed Explanation](#detailed-explanation)
- [How NAT Works](#how-nat-works)
- [ASCII Architecture Diagram](#ascii-architecture-diagram)
- [In a Nutshell](#in-a-nutshell)

---

## ❓ What is AWS NAT and when is it used?

So typically interviewer is asking you **what exactly is AWS Nat?**  
- How does it work?  
- And when you need to use NAT in real time?  

When asked this question, you can take an example and explain so that interviewer clearly understands.

---

## 📖 Detailed Explanation

👉 You have used NAT in the past, so you can explain:  

In our company we have **virtual machines deployed in private subnet**.  
- Within these VMs, backend applications are deployed.  
- These applications need to download dependencies (e.g., from **GitHub**).  
- Since they are in a **private subnet**, they don’t have direct internet access.  

💡 Reason: Private subnets do not have a route pointing to the **Internet Gateway** (only public subnets do).  

➡️ **Solution:** Create a **NAT Gateway**.  

- Add a route in the route table with destination `0.0.0.0/0` → pointing to **NAT Gateway** (instead of Internet Gateway).  
- NAT then allows instances in the private subnet to **initiate outbound internet requests** securely.  

---

## ⚙️ How NAT Works

1. Application in private subnet tries to download from the internet (e.g., GitHub).  
2. Request goes to **NAT Gateway**.  
3. NAT performs **Network Address Translation**:
   - Replaces **Source IP (Private App IP)** with **NAT Gateway Public IP**.  
   - Destination IP = GitHub server.  
4. GitHub responds → Response goes to NAT → NAT forwards it to the application.  

🔒 Benefits:  
- **Outbound internet access** without exposing private IPs.  
- **Security** → Real application IP is hidden.  
- Prevents external users from initiating inbound connections.  

---

## 🗺️ Architecture Diagram

                                        🌍 Internet (GitHub, External Services)
                                                      ▲
                                                      │
                                            🔒 NAT Gateway (Public IP)
                                                      ▲
                                                      │
                                ┌───────────────────────────────────────────┐
                                │               Route Table                 │
                                │    Destination: 0.0.0.0/0 → NAT Gateway   │
                                └───────────────────────────────────────────┘
                                                      ▲
                                                      │
                                    🖥️ Private Subnet (No direct Internet GW)
                                                      │
                          ┌───────────────────────┬───────────────────────┐
                          │                       │                       │
                    🖥️ App Server 1          🖥️ App Server 2          🖥️ App Server N
                     (Backend)                (Backend)                (Backend)
                                                      │
                                               📦 Dependencies
                                               (Download via NAT)


---

---

## 📌 In a Nutshell
NAT in AWS allows private subnet resources to initiate internet connections.

It hides the real IP and only exposes the NAT Gateway’s public IP.

Ensures security by blocking incoming connections from the internet.

In interviews, summarize it like:

    “NAT Gateway is used when instances in private subnets need outbound internet access for updates or dependencies, while still being protected from inbound traffic.”

---
