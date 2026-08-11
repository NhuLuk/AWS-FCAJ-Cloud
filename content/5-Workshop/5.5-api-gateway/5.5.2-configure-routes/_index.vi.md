---
title: "Cấu hình Routes"
date: 2026-06-22
weight: 2
chapter: false
pre: "<b>5.5.2. </b>"
---

## 5.5.2. Cấu hình Routes

Sau khi tạo `CloudMenuAPI`, bước tiếp theo là định nghĩa các Route để xác định cách API Gateway xử lý các HTTP request từ frontend.

Một Route của HTTP API được xác định bởi hai thành phần chính:

**HTTP Method + Resource Path**

Ví dụ:

`GET /orders`

CloudMenu sử dụng ba Route để xử lý các nghiệp vụ liên quan đến đơn hàng.

| Method | Route | Chức năng |
| :---: | :--- | :--- |
| `POST` | `/order` | Tạo một đơn hàng mới. |
| `GET` | `/orders` | Lấy danh sách các đơn hàng. |
| `PUT` | `/orders/{orderId}` | Cập nhật trạng thái của một đơn hàng. |

### Bước 1: Mở Routes

Trong `CloudMenuAPI`, chọn:

**Develop → Routes**

Danh sách các Route hiện tại của hệ thống được hiển thị.

![Các Route của CloudMenuAPI](/images/5-Workshop/5.5/api-routes.png)

### Bước 2: Cấu hình POST /order

Route:

`POST /order`

được sử dụng khi Customer gửi một đơn hàng mới.

Request body chứa thông tin của đơn hàng như số bàn, danh sách món và tổng giá trị đơn hàng.

Request sau đó được chuyển đến Lambda Function `createOrder`.

Luồng xử lý:

**Customer → POST /order → createOrder**

### Bước 3: Cấu hình GET /orders

Route:

`GET /orders`

được sử dụng để lấy danh sách đơn hàng hiện có.

Route này được sử dụng bởi các giao diện cần đọc dữ liệu đơn hàng, đặc biệt là Kitchen Interface và Manager Dashboard.

Luồng xử lý:

**Kitchen / Manager → GET /orders → getOrders**

### Bước 4: Cấu hình PUT /orders/{orderId}

Route:

`PUT /orders/{orderId}`

được sử dụng để cập nhật trạng thái của một đơn hàng cụ thể.

`{orderId}` là Path Parameter chứa mã của đơn hàng cần cập nhật.

Ví dụ:

`PUT /orders/ORDER001`

Lambda sử dụng giá trị `orderId` từ Path Parameter để xác định Item tương ứng trong DynamoDB.

Luồng xử lý:

**Kitchen → PUT /orders/{orderId} → updateOrderStatus**

Sau khi các Route được tạo, mỗi Route cần được gắn với Lambda Integration tương ứng.