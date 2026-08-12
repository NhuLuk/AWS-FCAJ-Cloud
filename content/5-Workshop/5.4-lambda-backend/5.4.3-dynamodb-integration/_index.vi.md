---
title: "Kết nối Lambda với DynamoDB"
date: 2026-06-22
weight: 3
chapter: false
pre: "<b>5.4.3. </b>"
---

## 5.4.3. Kết nối Lambda với DynamoDB

Sau khi tạo các Lambda Function và cấu hình IAM Execution Role, bước tiếp theo là kết nối các Function với bảng Amazon DynamoDB `CloudMenuOrders`.

Trong CloudMenu, AWS Lambda đảm nhiệm việc xử lý logic nghiệp vụ, trong khi Amazon DynamoDB được sử dụng để lưu trữ dữ liệu đơn hàng.

Luồng tương tác giữa các thành phần:

**Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

### Khởi tạo kết nối DynamoDB

Các Lambda Function của CloudMenu sử dụng AWS SDK for Python (`boto3`) để tương tác với Amazon DynamoDB.

Trong Lambda, kết nối đến DynamoDB được khởi tạo như sau:

```python
import boto3

dynamodb = boto3.resource(
    "dynamodb",
    region_name="ap-southeast-1"
)

table = dynamodb.Table("CloudMenuOrders")