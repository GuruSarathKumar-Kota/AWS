# AWS Services – ECR, EKS & Lambda 🚀  

[![AWS](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Markdown](https://img.shields.io/badge/Markdown-Notes-blue?logo=markdown)](https://daringfireball.net/projects/markdown/)
[![Status](https://img.shields.io/badge/Status-Complete-brightgreen)](#)
[![License](https://img.shields.io/badge/License-MIT-yellow)](#)

---

## 📖 Table of Contents  
- [What is ECR, EKS, Lambda in AWS?](#what-is-ecr-eks-lambda-in-aws-)
  - [Amazon ECR](#1-amazon-ecr-elastic-container-registry)
  - [Amazon EKS](#2-amazon-eks-elastic-kubernetes-service)
  - [AWS Lambda](#3-aws-lambda)
- [Architecture Diagram](#-detailed-ascii-architecture-diagram)
- [Summary](#summary)

---

## 2. What is ECR, EKS , Lambda in Aws ?  

In AWS, **ECR, EKS, and Lambda** are services that provide different functionalities for building and running applications in the cloud:

---

### 1. **Amazon ECR (Elastic Container Registry)** 🐳  
[![Amazon ECR Docs](https://img.shields.io/badge/AWS-ECR-orange?logo=amazon-aws&logoColor=white)](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)

- Amazon ECR is a fully managed container registry service that makes it easy to store, manage, and deploy Docker container images.  
- It integrates seamlessly with other AWS services such as Amazon ECS (Elastic Container Service) and EKS (Elastic Kubernetes Service), allowing you to deploy containers quickly and securely.

---

### 2. **Amazon EKS (Elastic Kubernetes Service)** ☸️  
[![Amazon EKS Docs](https://img.shields.io/badge/AWS-EKS-blue?logo=amazon-aws&logoColor=white)](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)

- Amazon EKS is a managed Kubernetes service that simplifies the process of running Kubernetes clusters on AWS.  
- It eliminates the need to install, operate, and maintain your own Kubernetes control plane, and provides a highly available and scalable Kubernetes environment.  

---

### 3. **AWS Lambda** ⚡  
[![AWS Lambda Docs](https://img.shields.io/badge/AWS-Lambda-yellow?logo=amazon-aws&logoColor=white)](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)

- AWS Lambda is a serverless computing service that lets you run code without provisioning or managing servers.  
- You can upload your code (in the form of a Lambda function) and AWS Lambda takes care of everything required to run and scale your code with high availability.  

---

## 🖼 Detailed ASCII Architecture Diagram  

                      ┌──────────────────────────┐
                      │       Developer          │
                      │ (Code Commit & Push)     │
                      └────────────┬─────────────┘
                                   │
                                   ▼
                      ┌──────────────────────────┐
                      │    CI/CD Pipeline         │
                      │ (GitHub Actions/Jenkins)  │
                      └────────────┬─────────────┘
                                   │
                ┌──────────────────┼──────────────────┐
                │                  │                  │
                ▼                  ▼                  ▼
       ┌───────────────┐  ┌─────────────────┐  ┌─────────────────┐
       │ Build Docker  │  │ Push to Amazon  │  │ Deploy to EKS   │
       │ Image         │  │ ECR Registry    │  │ Cluster (Pods)  │
       └───────────────┘  └─────────────────┘  └──────────┬──────┘
                                                           │
                                                           ▼
                                                  ┌───────────────────┐
                                                  │  App Running on   │
                                                  │  Kubernetes Pod   │
                                                  └──────────┬────────┘
                                                             │
                                                             ▼
                                                    ┌────────────────┐
                                                    │  Event Triggers│
                                                    │  AWS Lambda    │
                                                    └────────────────┘


---

### **Summary** 📝  

In summary:  

- **ECR** → Container registry service for managing Docker images.  
- **EKS** → Managed Kubernetes service for running Kubernetes clusters.  
- **Lambda** → Serverless computing service for running code without managing servers.  

> Each of these services plays a critical role in modern cloud application development and deployment on AWS.

---

### **Pro Tip** ⚡  

> Combine **ECR + EKS + Lambda** with **CI/CD pipelines** for an **automated, scalable, and event-driven cloud-native architecture**! ✨  

[![Learn More about AWS ECR](https://img.shields.io/badge/AWS-ECR-orange?logo=amazon-aws&logoColor=white)](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)
[![Learn More about AWS EKS](https://img.shields.io/badge/AWS-EKS-blue?logo=amazon-aws&logoColor=white)](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)
[![Learn More about AWS Lambda](https://img.shields.io/badge/AWS-Lambda-yellow?logo=amazon-aws&logoColor=white)](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)

---
