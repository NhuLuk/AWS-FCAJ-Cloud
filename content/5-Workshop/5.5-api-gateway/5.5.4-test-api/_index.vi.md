---
title: "Kiểm thử API"
date: 2026-06-22
weight: 4
chapter: false
pre: "<b>5.5.4. </b>"
---

## 5.5.4. Kiểm thử API

Sau khi hoàn thành cấu hình Routes và Lambda Integrations, tiến hành kiểm thử API để xác nhận API Gateway có thể chuyển request đến đúng Lambda Function và các chức năng của hệ thống hoạt động chính xác.

Trong CloudMenu, ba API chính được kiểm thử bằng Postman:

| Method | Endpoint | Chức năng |
|---|---|---|
| POST | `/order` | Tạo đơn hàng mới |
| GET | `/orders` | Lấy danh sách đơn hàng |
| PUT | `/orders/{orderId}` | Cập nhật trạng thái đơn hàng |

---

### 1. Kiểm thử POST /order

API `POST /order` được sử dụng để tạo một đơn hàng mới.

Trong Postman, chọn method **POST** và nhập endpoint:

```text
https://<api-id>.execute-api.us-east-1.amazonaws.com/order
```

Trong phần **Body**, chọn **raw → JSON** và nhập dữ liệu đơn hàng:

```json
{
  "orderId": "ORD003",
  "tableNumber": "05",
  "items": [
    {
      "name": "Cơm chiên",
      "price": 50000,
      "quantity": 2
    }
  ],
  "totalAmount": 100000
}
```

Nhấn **Send** để gửi request.

API Gateway chuyển request đến Lambda Function `createOrder`. Lambda xử lý dữ liệu và lưu đơn hàng vào bảng DynamoDB `CloudMenuOrders`.

Kết quả kiểm thử trả về HTTP status:

```text
200 OK
```

Điều này xác nhận API tạo đơn hàng đã hoạt động thành công.

![Test POST API](images/test-post-order.png)

---

### 2. Kiểm thử GET /orders

API `GET /orders` được sử dụng để lấy danh sách các đơn hàng đã được lưu trong hệ thống.

Trong Postman, chọn method **GET** và nhập endpoint:

```text
https://<api-id>.execute-api.us-east-1.amazonaws.com/orders
```

API Gateway chuyển request đến Lambda Function `getOrders`. Lambda truy vấn dữ liệu từ bảng DynamoDB `CloudMenuOrders` và trả danh sách đơn hàng về client.

Không cần truyền Request Body cho GET request.

Nhấn **Send** để gửi request.

Kết quả kiểm thử trả về:

```text
200 OK
```

Response chứa thông tin các đơn hàng hiện có trong hệ thống.

![Test GET API](images/test-get-orders.png)

---

### 3. Kiểm thử PUT /orders/{orderId}

API `PUT /orders/{orderId}` được sử dụng để cập nhật trạng thái của một đơn hàng.

Trong bài kiểm thử này, đơn hàng có `orderId` là `ORD003` được cập nhật.

Chọn method **PUT** và sử dụng endpoint:

```text
https://<api-id>.execute-api.us-east-1.amazonaws.com/orders/ORD003
```

Trong phần **Body**, chọn **raw → JSON** và nhập:

```json
{
  "status": "PREPARING"
}
```

Nhấn **Send** để gửi request.

API Gateway lấy giá trị `ORD003` từ path parameter `{orderId}` và chuyển request đến Lambda Function `updateOrderStatus`.

Lambda cập nhật trạng thái của đơn hàng tương ứng trong bảng DynamoDB `CloudMenuOrders`.

Kết quả kiểm thử trả về:

```text
200 OK
```

Điều này xác nhận trạng thái của đơn hàng đã được cập nhật thành công.

![Test PUT API](images/test-put-order.png)

---

### Kết quả kiểm thử

Cả ba API đều trả về HTTP status `200 OK`, cho thấy quá trình tích hợp giữa API Gateway, Lambda và DynamoDB hoạt động thành công.

Luồng xử lý có thể được tóm tắt như sau:

```text
Postman
   ↓
Amazon API Gateway
   ↓
AWS Lambda
   ↓
Amazon DynamoDB
   ↓
Response
```

Các API hiện hỗ trợ các chức năng cơ bản của hệ thống CloudMenu:

- Tạo đơn hàng mới.
- Lấy danh sách đơn hàng.
- Cập nhật trạng thái đơn hàng.

Sau khi xác nhận các API hoạt động chính xác, backend đã sẵn sàng để được tích hợp với giao diện người dùng của CloudMenu.