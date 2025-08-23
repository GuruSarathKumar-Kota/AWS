# 🔄 Cross-Account Access: Lambda in Account A → DynamoDB in Account B  

![AWS Lambda Docs](https://img.shields.io/badge/Docs-Lambda-yellow?logo=awslambda&link=https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)  
![AWS DynamoDB Docs](https://img.shields.io/badge/Docs-DynamoDB-blue?logo=amazondynamodb&link=https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)  
![AWS STS Docs](https://img.shields.io/badge/Docs-STS-orange?logo=amazonaws&link=https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html)  
![AWS IAM Docs](https://img.shields.io/badge/Docs-IAM-green?logo=amazonaws&link=https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)  

---

## 📑 Table of Contents
- [📝 Interview Context](#-interview-context)  
- [⚡ Step-by-Step Solution](#-step-by-step-solution)  
- [🖼️ Cross-Account Architecture Diagram](#️-cross-account-architecture-diagram)  
- [🛠️ Sample IAM Role Policy (Account B)](#️-sample-iam-role-policy-account-b)  
- [🛡️ Trust Policy Example (Account B)](#️-trust-policy-example-account-b)  
- [💻 Lambda Code Snippet (Account A)](#-lambda-code-snippet-account-a)  
- [🎯 Interview-Ready Answer](#-interview-ready-answer)  

---

## 📝 Interview Context  
The question is about **cross-account communication**:  

- ✅ **Account A** → Contains **Lambda function**  
- ✅ **Account B** → Contains **DynamoDB table**  
- ❌ By default → cross-account access is **blocked**  

We need to enable **Lambda in Account A** to query **DynamoDB in Account B** securely.  

---

## ⚡ Step-by-Step Solution  

1. **In Account B (DynamoDB Account)**  
   - Create an **IAM Role** (e.g., `CrossAccountDynamoDBRole`).  
   - Attach a **policy** granting DynamoDB access (`dynamodb:GetItem`).  

2. **Trust Policy Setup (Account B)**  
   - In the role’s **Trust Policy**, explicitly allow the **Lambda function ARN from Account A** to assume this role.  

3. **In Account A (Lambda Account)**  
   - Create the **Lambda function**.  
   - Inside Lambda code, use **AWS STS AssumeRole** API to assume the role created in Account B.  
   - Use the **temporary credentials** from STS to query DynamoDB.  

4. **Execution Flow**  
   - Lambda → STS AssumeRole (Account B Role) → Temporary Credentials → DynamoDB Access.  

---

## 🖼️ Cross-Account Architecture Diagram  

                                           🌩️ AWS Cloud
                       ┌───────────────────────────────┐
                       │        AWS Account A           │
                       │                               │
                       │  Lambda Function 🟡           │
                       │   - Uses STS:AssumeRole       │
                       │   - Needs DynamoDB Data       │
                       └───────────────┬───────────────┘
                                       │
                                       │ AssumeRole via STS
                                       ▼
                       ┌───────────────────────────────┐
                       │        AWS Account B           │
                       │                               │
                       │  IAM Role 📜                   │
                       │   - Policy: DynamoDB:GetItem   │
                       │   - Trust: Lambda ARN (A)      │
                       │                               │
                       │  DynamoDB Table 🗄️             │
                       │   - Data Accessed Securely     │
                       └───────────────────────────────┘                

---
### 🛠️ Sample IAM Role Policy (Account B)

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetItem"
      ],
      "Resource": "arn:aws:dynamodb:us-east-1:ACCOUNT_B_ID:table/YourTableName"
    }
  ]
}
🛡️ Trust Policy Example (Account B)
json
Copy
Edit
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::ACCOUNT_A_ID:role/service-role/YourLambdaExecutionRole"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}

### 💻 Lambda Code Snippet (Account A)

import boto3

def lambda_handler(event, context):
    # Create STS client
    sts_client = boto3.client('sts')

    # Assume Role in Account B
    assumed_role = sts_client.assume_role(
        RoleArn="arn:aws:iam::ACCOUNT_B_ID:role/CrossAccountDynamoDBRole",
        RoleSessionName="LambdaDynamoDBSession"
    )

    # Extract temporary credentials
    credentials = assumed_role['Credentials']

    # Use temporary creds for DynamoDB
    dynamodb = boto3.client(
        'dynamodb',
        aws_access_key_id=credentials['AccessKeyId'],
        aws_secret_access_key=credentials['SecretAccessKey'],
        aws_session_token=credentials['SessionToken']
    )

    # Fetch item from DynamoDB table
    response = dynamodb.get_item(
        TableName='YourTableName',
        Key={'id': {'S': '123'}}
    )

    return response['Item']

### 🎯 Interview-Ready Answer
“To allow a Lambda function in Account A to interact with DynamoDB in Account B, we use STS AssumeRole with trust policy. In Account B, we create an IAM Role with DynamoDB permissions and a trust policy allowing the Lambda execution role from Account A to assume it. Then in Account A, the Lambda uses STS AssumeRole API to fetch temporary credentials from Account B and uses them to query DynamoDB. The difference from same-account access is simply the trust relationship across accounts.”
