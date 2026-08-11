---
title: "Tích hợp API Gateway với AWS Lambda"
date: 2026-06-22
weight: 3
chapter: false
pre: "<b>5.5.3. </b>"
---

## 5.5.3. Tích hợp API Gateway với AWS Lambda

Sau khi tạo các Route, mỗi Route cần được kết nối với Lambda Function thực hiện nghiệp vụ tương ứng.

Trong Amazon API Gateway, kết nối giữa một Route và Backend được gọi là **Integration**.

CloudMenu sử dụng AWS Lambda làm Backend Integration.

### Bước 1: Mở Integrations

Trong `CloudMenuAPI`, chọn:

**Develop → Integrations**

CloudMenu có ba Lambda Integration tương ứng với ba chức năng backend.

![Các Integration của CloudMenuAPI](/images/5-Workshop/5.5/api-integrations.png)

Cấu hình được sử dụng:

| Route | Integration | Lambda Function |
| :--- | :--- | :--- |
| `POST /order` | Lambda Integration | `createOrder` |
| `GET /orders` | Lambda Integration | `getOrders` |
| `PUT /orders/{orderId}` | Lambda Integration | `updateOrderStatus` |

### Bước 2: Gắn createOrder với POST /order

Chọn Route:

`POST /order`

Sau đó chọn Integration tương ứng với Lambda:

`createOrder`

![Integration POST order](/images/5-Workshop/5.5/post-order-integration.png)

Khi Customer gửi request đến `POST /order`, API Gateway chuyển request đến `createOrder`.

Function xử lý request và ghi đơn hàng mới vào bảng `CloudMenuOrders`.

Luồng xử lý:

**POST /order → createOrder → DynamoDB put_item()**

### Bước 3: Gắn getOrders với GET /orders

Route:

`GET /orders`

được gắn với:

`getOrders`

Khi nhận request, Function đọc dữ liệu từ bảng `CloudMenuOrders` và trả danh sách đơn hàng về API Gateway.

Luồng xử lý:

**GET /orders → getOrders → DynamoDB scan()**

### Bước 4: Gắn updateOrderStatus với PUT /orders/{orderId}

Route:

`PUT /orders/{orderId}`

được gắn với:

`updateOrderStatus`

Function lấy `orderId` từ Path Parameter và trạng thái mới từ Request Body.

Sau đó Function cập nhật Item tương ứng trong DynamoDB.

Luồng xử lý:

**PUT /orders/{orderId} → updateOrderStatus → DynamoDB update_item()**

### Kiểm tra Route và Integration

Sau khi cấu hình, cần kiểm tra lại từng Route để đảm bảo Route được gắn đúng Integration.

Cấu hình cuối cùng của CloudMenu:

```text
POST /order
    └── createOrder

GET /orders
    └── getOrders

PUT /orders/{orderId}
    └── updateOrderStatus