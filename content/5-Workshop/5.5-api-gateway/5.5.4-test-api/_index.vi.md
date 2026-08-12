---
title: "Kiểm thử API"
date: 2026-06-22
weight: 4
chapter: false
pre: "<b>5.5.4. </b>"
---

## 5.5.4. Kiểm thử API

Sau khi hoàn thành cấu hình Routes và Lambda Integrations, tiến hành kiểm thử API để xác nhận Amazon API Gateway có thể chuyển request đến đúng Lambda Function và các chức năng backend của CloudMenu hoạt động chính xác.

Trong CloudMenu, ba API chính được kiểm thử bằng Postman:

| Method | Endpoint | Chức năng |
| :---: | :--- | :--- |
| `POST` | `/order` | Tạo đơn hàng mới |
| `GET` | `/orders` | Lấy danh sách đơn hàng |
| `PUT` | `/orders/{orderId}` | Cập nhật trạng thái đơn hàng |

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

Amazon API Gateway chuyển request đến Lambda Function `createOrder`. Lambda xử lý dữ liệu và lưu đơn hàng mới vào bảng Amazon DynamoDB `CloudMenuOrders`.

Kết quả kiểm thử trả về HTTP status:

```text
200 OK
```

Điều này xác nhận API tạo đơn hàng đã hoạt động thành công.

![Test POST API](/images/5-Workshop/5.5/test-post-order.png)

*Hình 1. Kiểm thử API POST /order bằng Postman.*

---

### 2. Kiểm thử GET /orders

API `GET /orders` được sử dụng để lấy danh sách các đơn hàng đã được lưu trong hệ thống.

Trong Postman, chọn method **GET** và nhập endpoint:

```text
https://<api-id>.execute-api.us-east-1.amazonaws.com/orders
```

Request `GET` không cần truyền Request Body.

Nhấn **Send** để gửi request.

Amazon API Gateway chuyển request đến Lambda Function `getOrders`. Lambda đọc dữ liệu từ bảng `CloudMenuOrders` và trả danh sách đơn hàng về client.

Kết quả kiểm thử trả về:

```text
200 OK
```

Response chứa thông tin các đơn hàng hiện có trong hệ thống.

![Test GET API](/images/5-Workshop/5.5/test-get-orders.png)

*Hình 2. Kiểm thử API GET /orders bằng Postman.*

---

### 3. Kiểm thử PUT /orders/{orderId}

API `PUT /orders/{orderId}` được sử dụng để cập nhật trạng thái của một đơn hàng.

Trong bài kiểm thử này, đơn hàng có `orderId` là `ORD003` được sử dụng.

Trong Postman, chọn method **PUT** và nhập endpoint:

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

Amazon API Gateway lấy giá trị `ORD003` từ Path Parameter `{orderId}` và chuyển request đến Lambda Function `updateOrderStatus`.

Lambda xác định đơn hàng tương ứng trong bảng `CloudMenuOrders` và cập nhật trạng thái của đơn hàng.

Kết quả kiểm thử trả về:

```text
200 OK
```

Điều này xác nhận trạng thái của đơn hàng đã được cập nhật thành công.

![Test PUT API](/images/5-Workshop/5.5/test-put-order.png)

*Hình 3. Kiểm thử API PUT /orders/{orderId} bằng Postman.*

---

### Kết quả kiểm thử

Cả ba API đều trả về HTTP status `200 OK`, cho thấy quá trình tích hợp giữa Amazon API Gateway, AWS Lambda và Amazon DynamoDB hoạt động thành công.

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

Kết quả kiểm thử:

| API | Lambda Function | Kết quả |
| :--- | :--- | :---: |
| `POST /order` | `createOrder` | `200 OK` |
| `GET /orders` | `getOrders` | `200 OK` |
| `PUT /orders/{orderId}` | `updateOrderStatus` | `200 OK` |

Các API hiện hỗ trợ các chức năng backend cơ bản của CloudMenu:

- Tạo đơn hàng mới.
- Lấy danh sách đơn hàng.
- Cập nhật trạng thái đơn hàng.

Sau khi xác nhận các API hoạt động chính xác, backend của CloudMenu đã sẵn sàng để được tích hợp với giao diện người dùng.