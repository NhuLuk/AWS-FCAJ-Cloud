---
title: "IAM Execution Role"
date: 2026-06-22
weight: 1
chapter: false
pre: "<b>5.4.1. </b>"
---

## 5.4.1. IAM Execution Role

Mỗi AWS Lambda Function sử dụng một IAM Execution Role để xác định các quyền mà Function được phép thực hiện đối với các dịch vụ AWS khác.

Trong CloudMenu, Lambda cần các quyền cần thiết để:

- Ghi log hoạt động vào Amazon CloudWatch Logs.
- Truy cập bảng `CloudMenuOrders` trên Amazon DynamoDB theo các thao tác nghiệp vụ của hệ thống.

Đối với Function `createOrder`, AWS Lambda sử dụng Execution Role:

`createOrder-role-fmflntg9`

Có thể kiểm tra Execution Role bằng cách:

1. Mở AWS Lambda.
2. Chọn Function `createOrder`.
3. Chọn tab **Configuration**.
4. Chọn **Permissions**.
5. Kiểm tra phần **Execution role**.

![Lambda Execution Role](/images/5-Workshop/5.4/lambda-execution-role.png)

Execution Role của `createOrder` được gắn các policy phục vụ cho việc ghi log và truy cập DynamoDB.

Trong đó:

- `AWSLambdaBasicExecutionRole` cung cấp các quyền cần thiết để Lambda ghi log vào Amazon CloudWatch Logs.
- `CloudMenuDynamoPolicy` là custom policy được sử dụng để cấp quyền truy cập bảng `CloudMenuOrders`.

![IAM Role Policies](/images/5-Workshop/5.4/iam-role-policies.png)

Custom policy `CloudMenuDynamoPolicy` giới hạn quyền truy cập DynamoDB vào các thao tác cần thiết:

```text
dynamodb:PutItem
dynamodb:GetItem
dynamodb:UpdateItem
dynamodb:Scan