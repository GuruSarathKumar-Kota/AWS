# 🔑 AWS Trust Policy Explained  

![AWS IAM Docs](https://img.shields.io/badge/Docs-IAM-green?logo=amazonaws&link=https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)  
![AWS STS Docs](https://img.shields.io/badge/Docs-STS-orange?logo=amazonaws&link=https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html)  
![AWS Lambda Docs](https://img.shields.io/badge/Docs-Lambda-yellow?logo=awslambda&link=https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)  
![AWS DynamoDB Docs](https://img.shields.io/badge/Docs-DynamoDB-blue?logo=amazondynamodb&link=https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)  

---

## 📑 Table of Contents  
- [📝 What is Trust Policy?](#-what-is-trust-policy)  
- [⚡ Why is it Used?](#-why-is-it-used)  
- [💡 Real-World Example: Lambda + DynamoDB](#-real-world-example-lambda--dynamodb)  
- [🖼️ ASCII Architecture Diagram](#️-ascii-architecture-diagram)  
- [🛠️ Trust Policy JSON Example](#️-trust-policy-json-example)  
- [🎯 Interview-Ready Answer](#-interview-ready-answer)  

---

## 📝 What is Trust Policy?  
A **Trust Policy** in AWS is a special type of policy attached to an **IAM Role** that defines **who (which principals)** can assume that role.  

- 🟢 Principals can be AWS **services** (e.g., Lambda, EC2), **users**, or even **resources in another AWS account**.  
- 🔐 Without a trust policy, AWS STS (Security Token Service) cannot grant temporary credentials.  
- 📜 It ensures that **only authorized entities** are allowed to assume the role.  

---

## ⚡ Why is it Used?  
- ✅ Restricts which **services, users, or accounts** can assume an IAM role.  
- ✅ Ensures **least privilege** by preventing unintended services from using sensitive roles.  
- ✅ Works **across accounts**, enabling cross-account access securely.  
- ✅ Often paired with **STS AssumeRole** for temporary access.  

---

## 💡 Real-World Example: Lambda + DynamoDB  
Imagine you have a **Lambda function** that needs to **read data from DynamoDB**, but the Lambda doesn’t have direct permissions.  

- Instead of granting full DynamoDB access to Lambda directly,  
- You create an **IAM Role with DynamoDB permissions**.  
- Then, you add a **Trust Policy** that specifies 👉 only **Lambda service** can assume this role.  
- During execution, Lambda uses **STS** to assume the role → fetch DynamoDB data.  

---

## 🖼️ Architecture Diagram  

                                             🌩️ AWS Cloud
                             ┌──────────────────────────┐
                             │         Lambda 🟡         │
                             │ Wants DynamoDB Access     │
                             └───────────┬───────────────┘
                                         │ STS AssumeRole
                                         ▼
                             ┌──────────────────────────┐
                             │  IAM Role 📜              │
                             │  Trust Policy:            │
                             │    Allow "lambda.amazonaws.com"  
                             │  Permissions: DynamoDB:GetItem  
                             └───────────┬──────────────┘
                                         │
                                         ▼
                             ┌──────────────────────────┐
                             │  DynamoDB 🗄️              │
                             │  Provides Data            │
                             └──────────────────────────┘


### 🛠️ Trust Policy JSON Example
Here’s a sample trust policy that allows Lambda to assume a role:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}

## 👉 If you want cross-account access, replace the "Service" principal with an AWS Account ID / Role ARN.

### 🎯 Interview-Ready Answer
“A trust policy in AWS is a JSON policy attached to an IAM role that defines who can assume the role. For example, if a Lambda function needs to access DynamoDB via an IAM role, the trust policy specifies that only lambda.amazonaws.com can assume that role. This ensures that no other services or users can assume it. Trust policies are critical in cross-account access too, where you explicitly grant another AWS account or role the ability to assume your role. In short, a trust policy acts as a gatekeeper that controls who is allowed to assume the role while STS issues the temporary credentials.”
