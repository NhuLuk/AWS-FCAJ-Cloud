---
title: "Worklog Tuần 3"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu Tuần 3

- Tìm hiểu mô hình cơ sở dữ liệu NoSQL và dịch vụ Amazon DynamoDB trên AWS.
- Nắm được các thành phần chính của DynamoDB như Table, Item, Attribute và Partition Key.
- Thực hành tạo bảng, thêm, đọc và cập nhật dữ liệu trên Amazon DynamoDB.
- Tìm hiểu cách thiết kế cấu trúc dữ liệu phù hợp để lưu trữ thông tin đơn hàng của hệ thống CloudMenu.

**Thời gian:** 06/07/2026 - 10/07/2026

---

### Tổng quan Nhiệm vụ Tuần

| Ngày | Hoạt động | Ngày bắt đầu | Ngày kết thúc | Tài liệu tham khảo |
| ---- | --------- | ------------ | ------------- | ------------------ |
| 1 | - Tìm hiểu mô hình cơ sở dữ liệu **NoSQL** <br> + Phân biệt cơ bản giữa cơ sở dữ liệu quan hệ và NoSQL <br> + Tìm hiểu đặc điểm và các trường hợp sử dụng NoSQL <br> + Tìm hiểu tổng quan về **Amazon DynamoDB** | 06/07/2026 | 06/07/2026 | [https://aws.amazon.com/dynamodb/](https://aws.amazon.com/dynamodb/) |
| 2 | - Tìm hiểu các thành phần chính của **Amazon DynamoDB** <br> + Table <br> + Item <br> + Attribute <br> + Partition Key <br> + Tìm hiểu cách DynamoDB tổ chức và truy xuất dữ liệu | 07/07/2026 | 07/07/2026 | [https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) |
| 3 | - Thực hành với **Amazon DynamoDB** <br> + Tạo DynamoDB Table <br> + Thiết lập Partition Key <br> + Thêm dữ liệu mẫu vào Table <br> + Thực hiện đọc và cập nhật Item trên AWS Management Console | 08/07/2026 | 08/07/2026 | [https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html) |
| 4 | - Tìm hiểu cách thiết kế dữ liệu đơn hàng trên **DynamoDB** <br> + Xác định các thông tin cần lưu trữ của một đơn hàng <br> + Thiết kế cấu trúc dữ liệu gồm Order ID, Table Number, Items, Total Amount và Status <br> + Tìm hiểu cách lưu danh sách món ăn trong một Item | 09/07/2026 | 09/07/2026 | [https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-general-nosql-design.html](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-general-nosql-design.html) |
| 5 | - Thiết kế dữ liệu cho hệ thống **CloudMenu** <br> + Tạo bảng `CloudMenuOrders` <br> + Sử dụng `orderId` làm Partition Key <br> + Xác định các thuộc tính `tableNumber`, `items`, `totalAmount`, `status`, `createdAt` và `updatedAt` <br> + Kiểm tra việc lưu trữ và truy xuất dữ liệu đơn hàng | 10/07/2026 | 10/07/2026 | [https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/) |

---

### Thành tựu Tuần 3

- Hiểu được mô hình cơ sở dữ liệu NoSQL và vai trò của Amazon DynamoDB trong kiến trúc Serverless.
- Nắm được các thành phần cơ bản của DynamoDB gồm Table, Item, Attribute và Partition Key.
- Thực hành tạo bảng, thêm, đọc và cập nhật dữ liệu trên Amazon DynamoDB.
- Thiết kế được cấu trúc dữ liệu đơn hàng phù hợp với nghiệp vụ của hệ thống CloudMenu.
- Tạo bảng `CloudMenuOrders` với `orderId` làm Partition Key và chuẩn bị nền tảng dữ liệu để tích hợp với AWS Lambda trong các tuần tiếp theo.