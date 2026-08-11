---
title: "Các Lambda Function của CloudMenu"
date: 2026-06-22
weight: 2
chapter: false
pre: "<b>5.4.2. </b>"
---

## 5.4.2. Các Lambda Function của CloudMenu

CloudMenu sử dụng ba AWS Lambda Function để xử lý các nghiệp vụ chính của hệ thống.

Các Function đều được triển khai với runtime **Python 3.13** và Package type **Zip**.

![Danh sách Lambda Function](/images/5-Workshop/5.4/lambda-functions.png)

### createOrder

Function `createOrder` được sử dụng để tạo một đơn hàng mới.

Function nhận dữ liệu đơn hàng từ request, bao gồm:

- `orderId`
- `tableNumber`
- `items`
- `totalAmount`
- `createdAt`

Khi đơn hàng được tạo, backend tự động thiết lập:

- `status` = `PENDING`
- `updatedAt` = thời điểm tạo đơn
- `completedAt` = `None`

Sau đó dữ liệu được ghi vào bảng `CloudMenuOrders` bằng thao tác:

`put_item()`

Luồng xử lý:

**Customer → API Gateway → createOrder → DynamoDB**

Nếu thiếu trường dữ liệu bắt buộc, Function trả về HTTP status `400`. Nếu xảy ra lỗi trong quá trình xử lý, Function trả về status `500`.

### getOrders

Function `getOrders` được sử dụng để lấy danh sách các đơn hàng hiện có trong bảng `CloudMenuOrders`.

Function sử dụng:

`table.scan()`

để đọc các Item trong bảng DynamoDB.

Do DynamoDB có thể trả về kiểu dữ liệu `Decimal`, Function sử dụng `DecimalEncoder` để chuyển đổi giá trị `Decimal` sang dạng số có thể serialize sang JSON.

Luồng xử lý:

**Kitchen / Manager → API Gateway → getOrders → DynamoDB**

Kết quả được trả về dưới dạng JSON để các giao diện Kitchen và Manager sử dụng.

### updateOrderStatus

Function `updateOrderStatus` được sử dụng để cập nhật trạng thái của một đơn hàng.

Function lấy `orderId` từ `pathParameters` và nhận trạng thái mới từ request body.

Các trạng thái hợp lệ gồm:

- `PENDING`
- `PREPARING`
- `COMPLETED`

Function cập nhật:

- `status`
- `updatedAt`

Nếu trạng thái mới là `COMPLETED`, Function đồng thời cập nhật:

- `completedAt`

Dữ liệu được cập nhật bằng:

`update_item()`

và Function sử dụng điều kiện:

`attribute_exists(orderId)`

để đảm bảo đơn hàng cần cập nhật tồn tại trong DynamoDB.

Luồng xử lý:

**Kitchen → API Gateway → updateOrderStatus → DynamoDB**

Nếu không tìm thấy đơn hàng, Function trả về status `404`. Các request không hợp lệ trả về `400`, trong khi lỗi xử lý nội bộ trả về `500`.