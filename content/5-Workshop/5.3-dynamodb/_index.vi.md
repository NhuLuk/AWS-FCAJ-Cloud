---
title: "Cơ sở dữ liệu với Amazon DynamoDB"
date: 2026-06-22
weight: 3
chapter: false
pre: "<b>5.3. </b>"
---

## 5.3. Cơ sở dữ liệu với Amazon DynamoDB

Trong phần này, Amazon DynamoDB được sử dụng làm cơ sở dữ liệu cho hệ thống CloudMenu. DynamoDB lưu trữ dữ liệu đơn hàng và cung cấp lớp lưu trữ cho các chức năng liên quan đến đặt món, xử lý đơn hàng và thống kê.

CloudMenu không cho phép frontend truy cập trực tiếp vào cơ sở dữ liệu. Thay vào đó, các request từ giao diện được gửi đến Amazon API Gateway, sau đó AWS Lambda xử lý nghiệp vụ và thực hiện các thao tác đọc hoặc ghi dữ liệu trên DynamoDB.

Luồng xử lý dữ liệu chính:

**Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

Trong phần này, chúng ta sẽ thực hiện:

1. Tạo bảng `CloudMenuOrders` trên Amazon DynamoDB.
2. Tìm hiểu cấu trúc dữ liệu đơn hàng được lưu trong bảng.
3. Kiểm tra dữ liệu được tạo và cập nhật trong quá trình CloudMenu hoạt động.

Sau khi hoàn thành phần này, bảng DynamoDB sẽ sẵn sàng để kết nối với các AWS Lambda Function của CloudMenu.