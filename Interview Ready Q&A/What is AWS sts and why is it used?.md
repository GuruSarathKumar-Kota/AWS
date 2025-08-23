# 🔐 AWS STS (Security Token Service) Explained  

![AWS STS Docs](https://img.shields.io/badge/Docs-STS-orange?logo=amazonaws&link=https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html)  
![AWS IAM Docs](https://img.shields.io/badge/Docs-IAM-green?logo=amazonaws&link=https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)  
![AWS Lambda Docs](https://img.shields.io/badge/Docs-Lambda-yellow?logo=awslambda&link=https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)  
![AWS DynamoDB Docs](https://img.shields.io/badge/Docs-DynamoDB-blue?logo=amazondynamodb&link=https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)  

---

## 📑 Table of Contents
- [📝 What is AWS STS?](#-what-is-aws-sts)  
- [⚡ Why is STS Used?](#-why-is-sts-used)  
- [💡 Real-World Example: Lambda + DynamoDB](#-real-world-example-lambda--dynamodb)  
- [🛠️ Code Snippet with Boto3](#️-code-snippet-with-boto3)  
- [🖼️ ASCII Architecture Diagram](#️-ascii-architecture-diagram)  
- [🎯 Interview-Ready Answer](#-interview-ready-answer)  

---

## 📝 What is AWS STS?  
**AWS STS (Security Token Service)** is a service that provides **temporary security credentials** (Access Key ID, Secret Access Key, Session Token) to **users, AWS services, or IAM principals**.  

👉 These credentials are short-lived and allow assuming **IAM roles** with specific permissions.  
👉 It’s commonly used for **cross-account access**, **federated authentication**, and **temporary privilege elevation**.  

---

## ⚡ Why is STS Used?  
- 🔑 **Temporary Access**: Provides credentials valid for a specific duration.  
- 🔄 **Role Assumption**: Lets AWS services (like Lambda, EC2) or users assume an IAM role.  
- 🛡️ **Security**: Reduces risks by avoiding long-term credentials.  
- 🌍 **Cross-Account Access**: Enables secure access to resources across AWS accounts (requires trust policy).  

---

## 💡 Real-World Example: Lambda + DynamoDB  
Imagine you have a **Lambda function** that needs to **read data from DynamoDB**, but the Lambda itself doesn’t have direct permissions.  

1. Create an **IAM Role** with DynamoDB read permissions.  
2. Inside the Lambda function, use **AWS STS** to **assume this IAM Role**.  
3. Lambda gets **temporary credentials** → reads from DynamoDB.  
4. Credentials expire after defined time → ensuring **least privilege** and **time-limited security**.  

---

## 🛠️ Code Snippet with Boto3  

import boto3

def lambda_handler(event, context):
    # Create STS client
    sts_client = boto3.client('sts')

    # Assume IAM Role
    response = sts_client.assume_role(
        RoleArn="arn:aws:iam::<ACCOUNT-ID>:role/DynamoDBAccessRole",
        RoleSessionName="LambdaSession"
    )

    # Extract temporary credentials
    credentials = response['Credentials']

    # Use temporary credentials to access DynamoDB
    dynamodb = boto3.client(
        'dynamodb',
        aws_access_key_id=credentials['AccessKeyId'],
        aws_secret_access_key=credentials['SecretAccessKey'],
        aws_session_token=credentials['SessionToken']
    )

    # Example: Get item from DynamoDB
    result = dynamodb.get_item(
        TableName='MyTable',
        Key={'id': {'S': '123'}}
    )

    return result

### 🖼️ Architecture Diagram


                                     🌩️ AWS Cloud
                     ┌──────────────────────────┐
                     │         Lambda 🟡         │
                     │ (No direct DynamoDB access)│
                     └───────────┬───────────────┘
                                 │ Uses STS AssumeRole
                                 ▼
                     ┌──────────────────────────┐
                     │  AWS STS 🔐              │
                     │  Issues Temporary Keys   │
                     │  (AccessKey, Secret,     │
                     │   SessionToken)          │
                     └───────────┬──────────────┘
                                 │
                                 ▼
                     ┌──────────────────────────┐
                     │  IAM Role 📜              │
                     │  Policy → DynamoDB:Read   │
                     └───────────┬──────────────┘
                                 │
                                 ▼
                     ┌──────────────────────────┐
                     │  DynamoDB 🗄️              │
                     │  Provides Data            │
                     └──────────────────────────┘

---
### 🎯 Interview-Ready Answer
“AWS STS (Security Token Service) is used to generate temporary security credentials that allow IAM users, services, or principals to assume roles securely. For example, if a Lambda function needs access to DynamoDB but doesn’t have direct permissions, it can use STS to assume an IAM role with the right policy. STS provides temporary keys (Access Key, Secret Key, Session Token), which the Lambda uses during execution to fetch data from DynamoDB. This ensures least privilege access, short-lived credentials, and secure cross-account communication. If the role exists in another AWS account, we also update the trust policy to allow access. So, STS is all about secure, temporary, role-based access management.”
