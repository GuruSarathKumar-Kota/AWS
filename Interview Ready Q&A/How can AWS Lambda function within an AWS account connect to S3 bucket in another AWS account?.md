# 🔄 Cross-Account AWS Lambda → S3 Access  

![AWS Lambda Docs](https://img.shields.io/badge/Docs-Lambda-yellow?logo=awslambda&link=https://docs.aws.amazon.com/lambda/)  
![AWS S3 Docs](https://img.shields.io/badge/Docs-S3-blue?logo=amazonaws&link=https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)  
![AWS IAM Docs](https://img.shields.io/badge/Docs-IAM-green?logo=amazonaws&link=https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)  

---

## 📑 Table of Contents  
- [📝 Problem Statement](#-problem-statement)  
- [⚙️ Step-by-Step Solution](#️-step-by-step-solution)  
- [🛡️ IAM Policy for Lambda Execution Role](#️-iam-policy-for-lambda-execution-role)  
- [🛡️ S3 Bucket Policy in Account B](#️-s3-bucket-policy-in-account-b)  
- [🖼️ ASCII Architecture Diagram](#️-ascii-architecture-diagram)  
- [🎯 Interview-Ready Answer](#-interview-ready-answer)  

---

## 📝 Problem Statement  
👉 You have an **AWS Lambda function in Account A** that needs to connect to an **S3 bucket in Account B**.  
👉 Interviewers often use this question to test **cross-account resource knowledge**.  
👉 Reality: **It’s almost the same process as same-account access** — the only difference is the **Account ID in the IAM Role ARN**.  

---

## ⚙️ Step-by-Step Solution  

1. **Create (or use existing) Lambda Function** in **Account A**.  
2. Attach an **Execution Role** to the Lambda function.  
3. Modify the **IAM Policy** for the Lambda execution role to allow `s3:GetObject` / `s3:PutObject`.  
4. In **Account B**, update the **S3 bucket policy** to trust the Lambda execution role from **Account A**.  
5. Test by invoking the Lambda to read/write from the cross-account S3 bucket.  

---

## 🛡️ IAM Policy for Lambda Execution Role  
👉 Applied in **Account A**.  

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-shared-bucket/*"
    }
  ]
}

🛡️ S3 Bucket Policy in Account B
👉 Update the bucket policy to allow Lambda’s Execution Role ARN from Account A.


{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<ACCOUNT-A-ID>:role/<LambdaExecutionRole>"
      },
      "Action": [
      "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-shared-bucket/*"
    }
  ]
}

### 🖼️ Architecture Diagram

                                                    🌩️ AWS Cloud
                                  
                                       ┌──────────────────────┐
                                       │  Account A           │
                                       │  ───────────         │
                                       │  AWS Lambda 🟡        │
                                       │  Role: LambdaExec    │
                                       └─────────┬────────────┘
                                                 │ AssumeRole / IAM Trust
                                                 ▼
                                       ┌──────────────────────┐
                                       │  Account B           │
                                       │  ───────────         │
                                       │  Amazon S3 🪣         │
                                       │  Bucket: my-shared   │
                                       │  Policy trusts ARN   │
                                       └──────────────────────┘

---
### 🔑 Key Flow:

Lambda in Account A executes.

Uses its Execution Role IAM Policy.

S3 Bucket in Account B trusts that Role’s ARN.

Lambda reads/writes objects securely.

### 🎯 Interview-Ready Answer
“In AWS, making a Lambda in Account A access an S3 bucket in Account B is almost the same as doing it in the same account. First, I attach an execution role to the Lambda in Account A with permissions to access the S3 bucket. Then, on the bucket policy in Account B, I explicitly allow the ARN of the Lambda execution role from Account A. The only real difference is that the IAM ARN contains a different account ID. Once that trust is established, Lambda can securely perform actions like GetObject and PutObject across accounts. So it’s just a matter of IAM policy on the role and a bucket policy on the target bucket.”
