---
title: "Lambda and DynamoDB Integration"
date: 2026-06-22
weight: 3
chapter: false
pre: "<b>5.4.3. </b>"
---

## 5.4.3. Lambda and DynamoDB Integration

After creating the Lambda Functions and configuring the IAM Execution Role, the next step is to connect the Functions to the Amazon DynamoDB table `CloudMenuOrders`.

In CloudMenu, AWS Lambda handles the backend business logic, while Amazon DynamoDB is used to store order data.

The interaction flow between the components is:

**Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

### Initialize the DynamoDB Connection

The CloudMenu Lambda Functions use the AWS SDK for Python (`boto3`) to interact with Amazon DynamoDB.

The connection to DynamoDB is initialized as follows:

```python
import boto3

dynamodb = boto3.resource(
    "dynamodb",
    region_name="ap-southeast-1"
)

table = dynamodb.Table("CloudMenuOrders")