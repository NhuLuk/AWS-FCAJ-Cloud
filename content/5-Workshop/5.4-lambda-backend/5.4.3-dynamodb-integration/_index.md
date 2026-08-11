---
title: "Connect Lambda to DynamoDB"
date: 2026-06-22
weight: 3
chapter: false
pre: "<b>5.4.3. </b>"
---

## 5.4.3. Connect Lambda to DynamoDB

The CloudMenu Lambda functions use the AWS SDK for Python (`boto3`) to access Amazon DynamoDB.

For example, Lambda initializes the DynamoDB resource and references the table as follows:

```python
import boto3

dynamodb = boto3.resource(
    "dynamodb",
    region_name="ap-southeast-1"
)

table = dynamodb.Table("CloudMenuOrders")