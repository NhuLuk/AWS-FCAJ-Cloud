---
title: "Xây dựng API với Amazon API Gateway"
date: 2026-06-22
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---

## 5.5. Xây dựng API với Amazon API Gateway

Trong phần này, Amazon API Gateway được sử dụng làm lớp giao tiếp giữa frontend CloudMenu và các AWS Lambda Function của backend.

CloudMenu sử dụng một HTTP API có tên:

`CloudMenuAPI`

API tiếp nhận HTTP request từ các giao diện Customer, Kitchen và Manager, sau đó chuyển request đến Lambda Function tương ứng để xử lý nghiệp vụ.

Luồng xử lý chính:

**Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

CloudMenu hiện sử dụng ba route chính:

| Method | Route | Lambda Function | Chức năng |
| :--- | :--- | :--- | :--- |
| `POST` | `/order` | `createOrder` | Tạo đơn hàng mới. |
| `GET` | `/orders` | `getOrders` | Lấy danh sách đơn hàng. |
| `PUT` | `/orders/{orderId}` | `updateOrderStatus` | Cập nhật trạng thái đơn hàng. |

Trong phần này, chúng ta sẽ:

1. Tạo HTTP API `CloudMenuAPI`.
2. Cấu hình các route cho hệ thống.
3. Kết nối từng route với Lambda Function tương ứng.
4. Kiểm thử hoạt động của API.

Sau khi hoàn thành phần này, frontend CloudMenu có thể gửi request đến backend thông qua Amazon API Gateway.