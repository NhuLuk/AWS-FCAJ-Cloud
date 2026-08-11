---
title: "IAM Execution Role"
date: 2026-06-22
weight: 1
chapter: false
pre: "<b>5.4.1. </b>"
---

## 5.4.1. IAM Execution Role

Mỗi AWS Lambda Function sử dụng một IAM Execution Role để xác định các quyền mà Function được phép thực hiện đối với các dịch vụ AWS khác.

Trong CloudMenu, Lambda cần quyền để:

- Ghi log hoạt động vào Amazon CloudWatch Logs.
- Truy cập bảng `CloudMenuOrders` trên Amazon DynamoDB.
- Được Amazon API Gateway gọi thông qua quyền `lambda:InvokeFunction`.

Đối với Function `createOrder`, AWS Lambda sử dụng Execution Role:

`createOrder-role-fmflntg9`

Có thể kiểm tra Execution Role bằng cách:

1. Mở AWS Lambda.
2. Chọn Function `createOrder`.
3. Chọn tab **Configuration**.
4. Chọn **Permissions**.
5. Kiểm tra phần **Execution role**.

![Lambda Execution Role](/images/5-Workshop/5.4/lambda-execution-role.png)

Execution Role cần được cấp các quyền phù hợp để Lambda thực hiện đúng chức năng nhưng không được cấp các quyền không cần thiết.

Nguyên tắc **Least Privilege** nên được áp dụng để giới hạn quyền truy cập của từng Function đến các tài nguyên cần thiết.

Sau khi Execution Role được cấu hình, Lambda có thể ghi log và tương tác với DynamoDB theo các quyền được cấp.