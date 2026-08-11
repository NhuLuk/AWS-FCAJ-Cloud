---
title: "Kiểm tra dữ liệu"
date: 2026-06-22
weight: 3
chapter: false
pre: "<b>5.3.3. </b>"
---

## 5.3.3. Kiểm tra dữ liệu

Sau khi CloudMenu đã tạo và xử lý các đơn hàng, chúng ta có thể kiểm tra dữ liệu được lưu trong Amazon DynamoDB để xác nhận quá trình ghi và cập nhật dữ liệu hoạt động đúng.

### Bước 1: Mở bảng CloudMenuOrders

Trong AWS Management Console, truy cập:

**Amazon DynamoDB → Explore items**

Chọn bảng:

`CloudMenuOrders`

Sau đó thực hiện truy vấn để hiển thị các Item hiện có trong bảng.

### Bước 2: Kiểm tra các Item

DynamoDB hiển thị danh sách các đơn hàng đã được lưu trong `CloudMenuOrders`.

![Dữ liệu trong bảng CloudMenuOrders](/images/5-Workshop/5.3/order-items.png)

Trong kết quả có thể kiểm tra các thuộc tính như:

- `orderId`
- `completedAt`
- `createdAt`
- `items`
- `status`
- `tableNumber`
- `totalAmount`
- `updatedAt`

Mỗi dòng tương ứng với một đơn hàng trong CloudMenu.

Ví dụ, `orderId` được sử dụng để phân biệt các đơn hàng, `tableNumber` xác định bàn gọi món, `totalAmount` lưu tổng giá trị đơn hàng và `status` cho biết trạng thái xử lý hiện tại.

### Bước 3: Đối chiếu với CloudMenu

Có thể đối chiếu dữ liệu trong DynamoDB với các giao diện của CloudMenu.

Khi khách hàng gửi một đơn hàng:

**Customer → API Gateway → Lambda → DynamoDB**

Một Item tương ứng được tạo trong bảng `CloudMenuOrders`.

Khi nhân viên bếp cập nhật trạng thái:

**Kitchen → API Gateway → Lambda → DynamoDB**

Thuộc tính `status` của đơn hàng được cập nhật.

Dashboard quản lý sử dụng dữ liệu từ backend để tổng hợp các thông tin như số lượng đơn hàng, doanh thu và trạng thái xử lý.

Việc các Item xuất hiện trong `CloudMenuOrders` với đầy đủ thông tin về đơn hàng cho thấy lớp lưu trữ dữ liệu đã hoạt động và sẵn sàng được sử dụng bởi các chức năng backend của CloudMenu.

Trong phần tiếp theo, chúng ta sẽ triển khai AWS Lambda để xử lý các nghiệp vụ của hệ thống và tương tác với bảng `CloudMenuOrders`.