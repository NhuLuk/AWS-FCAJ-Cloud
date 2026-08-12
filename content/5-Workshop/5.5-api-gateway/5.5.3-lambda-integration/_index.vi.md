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
| :---: | :--- | :--- |
| `POST /order` | Lambda Integration | `createOrder` |
| `GET /orders` | Lambda Integration | `getOrders` |
| `PUT /orders/{orderId}` | Lambda Integration | `updateOrderStatus` |

### Bước 2: Gắn createOrder với POST /order

Chọn Route:

`POST /order`

Sau đó chọn Integration tương ứng với Lambda Function:

`createOrder`

![Integration POST order](/images/5-Workshop/5.5/post-order-integration.png)

Khi Customer gửi request đến `POST /order`, Amazon API Gateway chuyển request đến Lambda Function `createOrder`.

Function nhận dữ liệu đơn hàng từ request, xử lý các thông tin cần thiết và ghi đơn hàng mới vào bảng `CloudMenuOrders` trên Amazon DynamoDB.

Luồng xử lý:

```text
POST /order
     ↓
Amazon API Gateway
     ↓
createOrder
     ↓
DynamoDB put_item()
```

### Bước 3: Gắn getOrders với GET /orders

Route:

`GET /orders`

được gắn với Lambda Function:

`getOrders`

Khi nhận request từ frontend, Amazon API Gateway chuyển request đến Function `getOrders`.

Function sử dụng thao tác `scan()` để đọc các Item hiện có trong bảng `CloudMenuOrders` và trả danh sách đơn hàng về API Gateway.

Luồng xử lý:

```text
GET /orders
     ↓
Amazon API Gateway
     ↓
getOrders
     ↓
DynamoDB scan()
```

Route này được sử dụng bởi các giao diện cần đọc dữ liệu đơn hàng, chẳng hạn như Kitchen Interface và Dashboard.

### Bước 4: Gắn updateOrderStatus với PUT /orders/{orderId}

Route:

`PUT /orders/{orderId}`

được gắn với Lambda Function:

`updateOrderStatus`

Trong Route này, `{orderId}` là Path Parameter được sử dụng để xác định đơn hàng cần cập nhật.

Ví dụ:

```text
PUT /orders/ORD003
```

Amazon API Gateway chuyển request và giá trị `orderId` đến Lambda Function `updateOrderStatus`.

Function lấy `orderId` từ Path Parameter và trạng thái mới từ Request Body. Sau đó, Function sử dụng thao tác `update_item()` để cập nhật Item tương ứng trong bảng `CloudMenuOrders`.

Luồng xử lý:

```text
PUT /orders/{orderId}
          ↓
Amazon API Gateway
          ↓
updateOrderStatus
          ↓
DynamoDB update_item()
```

### Kiểm tra Route và Integration

Sau khi hoàn thành cấu hình, cần kiểm tra lại từng Route để đảm bảo mỗi Route được gắn với đúng Lambda Integration.

Cấu hình cuối cùng của CloudMenu:

```text
POST /order
    └── createOrder
            └── DynamoDB put_item()

GET /orders
    └── getOrders
            └── DynamoDB scan()

PUT /orders/{orderId}
    └── updateOrderStatus
            └── DynamoDB update_item()
```

Sau khi các Integration được cấu hình thành công, Amazon API Gateway có thể tiếp nhận các HTTP request từ frontend và chuyển chúng đến Lambda Function tương ứng để xử lý.

Ở bước tiếp theo, các API sẽ được kiểm thử để xác nhận quá trình tích hợp giữa Amazon API Gateway, AWS Lambda và Amazon DynamoDB hoạt động chính xác.