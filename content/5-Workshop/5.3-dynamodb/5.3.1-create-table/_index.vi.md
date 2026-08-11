---
title: "Tạo bảng CloudMenuOrders"
date: 2026-06-22
weight: 1
chapter: false
pre: "<b>5.3.1. </b>"
---

## 5.3.1. Tạo bảng CloudMenuOrders

Trong bước này, chúng ta sẽ tạo bảng Amazon DynamoDB dùng để lưu trữ dữ liệu đơn hàng của hệ thống CloudMenu.

### Bước 1: Truy cập Amazon DynamoDB

Đăng nhập vào **AWS Management Console**.

Tại thanh tìm kiếm của AWS Management Console, nhập:

`DynamoDB`

Chọn **DynamoDB** để truy cập giao diện quản lý Amazon DynamoDB.

Đảm bảo Region đang được sử dụng là:

**Asia Pacific (Singapore) – ap-southeast-1**

### Bước 2: Tạo bảng

Trong giao diện Amazon DynamoDB:

1. Chọn **Tables** ở menu bên trái.
2. Chọn **Create table**.
3. Nhập thông tin cho bảng.

Cấu hình bảng như sau:

| Thuộc tính | Giá trị |
| :--- | :--- |
| **Table name** | `CloudMenuOrders` |
| **Partition key** | `orderId` |
| **Data type** | String |
| **Sort key** | Không sử dụng |
| **Capacity mode** | On-demand |

`orderId` được sử dụng làm Partition Key để định danh từng đơn hàng trong hệ thống CloudMenu.

CloudMenu hiện không sử dụng Sort Key cho bảng `CloudMenuOrders`.

Với **On-demand capacity mode**, DynamoDB tự động quản lý khả năng đọc và ghi theo lưu lượng truy cập mà không yêu cầu cấu hình trước Read Capacity Units hoặc Write Capacity Units. Cấu hình này phù hợp với môi trường Workshop khi lưu lượng truy cập có thể thay đổi và không cần dự đoán trước.

Sau khi hoàn tất cấu hình, chọn **Create table**.

### Bước 3: Kiểm tra bảng

Sau khi DynamoDB hoàn tất quá trình tạo bảng, chọn:

**Tables → CloudMenuOrders**

Kiểm tra phần **General information**.

![Thông tin bảng CloudMenuOrders](/images/5-Workshop/5.3/dynamodb-table-details.png)

Bảng được cấu hình với:

- Partition key: `orderId (String)`
- Sort key: Không sử dụng
- Capacity mode: `On-demand`
- Table status: `Active`

Khi trạng thái của bảng chuyển sang **Active**, bảng `CloudMenuOrders` đã sẵn sàng để được sử dụng bởi backend của CloudMenu.

Ở bước tiếp theo, chúng ta sẽ tìm hiểu cấu trúc dữ liệu của một đơn hàng được lưu trong bảng này.