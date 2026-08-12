---
title: "Kiểm thử Lambda Function"
date: 2026-06-22
weight: 4
chapter: false
pre: "<b>5.4.4. </b>"
---

## 5.4.4. Kiểm thử Lambda Function

Sau khi triển khai các Lambda Function và cấu hình quyền truy cập đến Amazon DynamoDB, cần kiểm thử từng Function để xác nhận backend của CloudMenu có thể xử lý dữ liệu đúng trước khi thực hiện kiểm thử hoàn chỉnh thông qua Amazon API Gateway.

AWS Lambda cung cấp chức năng **Test**, cho phép tạo Test Event và thực thi trực tiếp Function trên AWS Management Console.

Trong phần này, ba Lambda Function chính của CloudMenu được kiểm thử:

- `createOrder`
- `getOrders`
- `updateOrderStatus`

---

### Kiểm thử createOrder

Function `createOrder` chịu trách nhiệm tiếp nhận thông tin đơn hàng và tạo một Item mới trong bảng `CloudMenuOrders`.

Để kiểm thử, mở:

**AWS Lambda → Functions → createOrder → Test**

Tạo một Test Event với dữ liệu đơn hàng mẫu. Event được xây dựng tương tự request mà Function nhận từ Amazon API Gateway.

Ví dụ:

```json
{
  "body": "{\"orderId\":\"TEST001\",\"tableNumber\":\"01\",\"items\":[{\"name\":\"Cơm chiên\",\"price\":50000,\"quantity\":1}],\"totalAmount\":50000}"
}