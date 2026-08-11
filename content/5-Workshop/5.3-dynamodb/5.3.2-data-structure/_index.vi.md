---
title: "Cấu trúc dữ liệu đơn hàng"
date: 2026-06-22
weight: 2
chapter: false
pre: "<b>5.3.2. </b>"
---

## 5.3.2. Cấu trúc dữ liệu đơn hàng

Sau khi tạo bảng `CloudMenuOrders`, dữ liệu đơn hàng của CloudMenu được lưu dưới dạng các Item trong Amazon DynamoDB.

Mỗi đơn hàng được xác định duy nhất bằng thuộc tính `orderId`.

![Mô hình dữ liệu CloudMenuOrders](/images/5-Workshop/5.3/dynamodb-data-model.png)

Dựa trên dữ liệu thực tế của hệ thống CloudMenu, một đơn hàng trong bảng `CloudMenuOrders` gồm các thuộc tính chính:

| Thuộc tính | Mô tả |
| :--- | :--- |
| `orderId` | Mã định danh duy nhất của đơn hàng và là Partition Key của bảng. |
| `tableNumber` | Số bàn thực hiện gọi món. |
| `items` | Danh sách các món ăn thuộc đơn hàng. |
| `totalAmount` | Tổng giá trị của đơn hàng. |
| `status` | Trạng thái xử lý hiện tại của đơn hàng. |
| `createdAt` | Thời điểm đơn hàng được tạo. |
| `updatedAt` | Thời điểm đơn hàng được cập nhật gần nhất. |
| `completedAt` | Thời điểm đơn hàng được hoàn thành. |

### Trạng thái đơn hàng

Trong quá trình xử lý, trạng thái của đơn hàng được thay đổi theo luồng:

**PENDING → PREPARING → COMPLETED**

Trong đó:

- `PENDING`: Đơn hàng đã được khách hàng gửi và đang chờ bếp tiếp nhận.
- `PREPARING`: Đơn hàng đã được bếp tiếp nhận và đang được chế biến.
- `COMPLETED`: Quá trình chế biến đơn hàng đã hoàn thành.

Khi khách hàng gửi đơn từ Customer Interface, frontend gửi request đến Amazon API Gateway. AWS Lambda xử lý request và tạo dữ liệu đơn hàng trong bảng `CloudMenuOrders`.

Luồng ghi dữ liệu:

**Customer Interface → API Gateway → Lambda → CloudMenuOrders**

Khi nhân viên bếp tiếp nhận hoặc hoàn thành đơn hàng, Kitchen Interface gửi request cập nhật trạng thái. Lambda xác định đơn hàng thông qua `orderId` và cập nhật dữ liệu tương ứng trong DynamoDB.

Luồng cập nhật dữ liệu:

**Kitchen Interface → API Gateway → Lambda → CloudMenuOrders**

Nhờ sử dụng `orderId` làm Partition Key, backend có thể xác định đơn hàng cần xử lý khi thực hiện các thao tác cập nhật.

Ở bước tiếp theo, chúng ta sẽ kiểm tra các Item thực tế được lưu trong bảng `CloudMenuOrders`.