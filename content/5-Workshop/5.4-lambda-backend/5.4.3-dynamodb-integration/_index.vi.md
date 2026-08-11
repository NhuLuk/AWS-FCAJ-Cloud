---
title: "Kết nối Lambda với DynamoDB"
date: 2026-06-22
weight: 3
chapter: false
pre: "<b>5.4.3. </b>"
---

## 5.4.3. Kết nối Lambda với DynamoDB

Các Lambda Function của CloudMenu sử dụng AWS SDK for Python (`boto3`) để truy cập Amazon DynamoDB.

Ví dụ, Lambda khởi tạo kết nối đến DynamoDB và tham chiếu đến bảng:

```python
import boto3

dynamodb = boto3.resource(
    "dynamodb",
    region_name="ap-southeast-1"
)

table = dynamodb.Table("CloudMenuOrders")