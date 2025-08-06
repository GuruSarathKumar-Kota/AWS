# ⚡ Types of Load Balancers in AWS and When to Use Them?

[![AWS CLB](https://img.shields.io/badge/AWS-Classic%20Load%20Balancer-orange?logo=amazon-aws)](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/introduction.html)
[![AWS ALB](https://img.shields.io/badge/AWS-Application%20Load%20Balancer-blue?logo=amazon-aws)](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)
[![AWS NLB](https://img.shields.io/badge/AWS-Network%20Load%20Balancer-green?logo=amazon-aws)](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html)

---

## 📑 Table of Contents
- [Overview](#overview)
- [Classic Load Balancer (CLB)](#1-classic-load-balancer-clb)
- [Application Load Balancer (ALB)](#2-application-load-balancer-alb)
- [Network Load Balancer (NLB)](#3-network-load-balancer-nlb)
- [When to Use Each](#when-to-use-each-type-of-load-balancer)
- [Architecture Diagram](#-aws-load-balancer-architecture-diagram)

---

## Overview

In AWS, there are three types of load balancers available: **Classic Load Balancer (CLB)**, **Application Load Balancer (ALB)**, and **Network Load Balancer (NLB)**. Each type of load balancer has its own features and use cases:

---

## 1. **Classic Load Balancer (CLB)**

[![Docs](https://img.shields.io/badge/Docs-CLB-orange?logo=read-the-docs)](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/introduction.html)

- CLB is the oldest type of load balancer in AWS, offering basic load balancing across multiple EC2 instances.
- It operates at both the application and transport layers of the OSI model, providing basic routing based on either TCP/UDP protocols or HTTP/HTTPS requests.
- CLB is suitable for simple use cases that require basic load balancing without advanced features like content-based routing or support for WebSocket protocol.

---

## 2. **Application Load Balancer (ALB)**

[![Docs](https://img.shields.io/badge/Docs-ALB-blue?logo=read-the-docs)](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)

- ALB operates at the application layer (Layer 7) of the OSI model and provides advanced features for routing HTTP and HTTPS traffic to different targets (such as EC2 instances, Lambda functions, or containers).
- ALB supports features like content-based routing, host-based routing, path-based routing, and support for WebSocket protocol.
- ALB is suitable for modern web applications that require flexible routing and advanced features for microservices architectures.

---

## 3. **Network Load Balancer (NLB)**

[![Docs](https://img.shields.io/badge/Docs-NLB-green?logo=read-the-docs)](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html)

- NLB operates at the transport layer (Layer 4) of the OSI model and provides ultra-high performance, low-latency load balancing for TCP and UDP traffic.
- NLB is designed for applications that require high throughput, low latency, and static IP addresses.
- NLB is suitable for use cases like TCP/UDP-based services, high-performance computing (HPC), and IoT applications.

---

## **When to Use Each Type of Load Balancer**

- **CLB:** Use CLB for simple load balancing needs, such as distributing HTTP or HTTPS traffic across multiple EC2 instances.
- **ALB:** Use ALB for modern web applications that require advanced routing features, support for WebSocket protocol, and flexible routing based on hostnames, paths, or headers.
- **NLB:** Use NLB for applications that require ultra-high performance, low-latency load balancing, and support for TCP or UDP protocols.

---

## 🎨 **AWS Load Balancer Architecture Diagram**

                       ┌─────────────────────────────┐
                       │        🌐 Clients           │
                       └──────────────┬──────────────┘
                                      │
                    ┌─────────────────┼─────────────────┐
                    │                 │                 │
          ┌─────────▼─────────┐ ┌─────▼──────┐ ┌────────▼────────┐
          │  ☁️ CLB (L4+L7)   │ │ 🎯 ALB (L7) │ │ ⚡ NLB (L4)   │
          │  Basic Balancing  │ │ Smart Routing│ │ High Perf / TCP│
          └─────────┬─────────┘ └─────┬──────┘ └────────┬────────┘
                    │                 │                 │
           ┌────────▼─────┐   ┌───────▼───────┐   ┌─────▼───────┐
           │ EC2 Instances│   │Lambda / ECS   │   │  EC2 / HPC  │
           │(Web Servers) │   │Containers     │   │IoT / Services│
           └──────────────┘   └───────────────┘   └──────────────┘

---

## Summary
In summary, choose the type of load balancer in AWS based on your application's requirements for features, performance, and scalability.
