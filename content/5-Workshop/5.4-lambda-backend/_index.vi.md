---
title: "Xây dựng Backend với AWS Lambda"
date: 2026-06-22
weight: 4
chapter: false
pre: "<b>5.4. </b>"
---

## 5.4. Xây dựng Backend với AWS Lambda

Trong phần này, AWS Lambda được sử dụng để xây dựng lớp xử lý nghiệp vụ phía backend cho hệ thống CloudMenu.

CloudMenu hiện sử dụng ba Lambda Function chính:

- `createOrder`: Tạo đơn hàng mới.
- `getOrders`: Lấy danh sách đơn hàng.
- `updateOrderStatus`: Cập nhật trạng thái đơn hàng.

Các Lambda Function nhận request từ Amazon API Gateway, thực hiện logic nghiệp vụ và tương tác với bảng `CloudMenuOrders` trên Amazon DynamoDB.

Luồng xử lý backend chính:

**Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

Trong phần này, chúng ta sẽ:

1. Kiểm tra IAM Execution Role của Lambda.
2. Tạo và cấu hình các Lambda Function của CloudMenu.
3. Kết nối Lambda với Amazon DynamoDB.
4. Kiểm thử Lambda Function.

Sau khi hoàn thành phần này, backend của CloudMenu sẽ sẵn sàng để được kết nối với Amazon API Gateway.