---
title: "Tạo HTTP API"
date: 2026-06-22
weight: 1
chapter: false
pre: "<b>5.5.1. </b>"
---

## 5.5.1. Tạo HTTP API

Trong bước này, chúng ta sẽ sử dụng Amazon API Gateway để tạo HTTP API làm điểm truy cập cho backend của CloudMenu.

API Gateway tiếp nhận các HTTP request từ frontend và chuyển request đến AWS Lambda Function tương ứng để xử lý.

### Bước 1: Truy cập Amazon API Gateway

Đăng nhập vào **AWS Management Console**.

Tại thanh tìm kiếm, nhập:

`API Gateway`

Chọn **API Gateway** để mở giao diện quản lý API.

### Bước 2: Tạo HTTP API

Trong giao diện Amazon API Gateway:

1. Chọn **APIs**.
2. Chọn **Create API**.
3. Tại loại **HTTP API**, chọn **Build**.
4. Nhập tên API:

`CloudMenuAPI`

CloudMenu sử dụng HTTP API vì backend chủ yếu cần cung cấp các HTTP endpoint để frontend thực hiện các thao tác tạo, truy xuất và cập nhật đơn hàng.

Sau khi hoàn tất cấu hình, tạo API và truy cập giao diện quản lý của `CloudMenuAPI`.

### Bước 3: Kiểm tra API

Sau khi API được tạo, Amazon API Gateway cung cấp một API endpoint để client gửi request đến hệ thống.

Trong CloudMenu, API này đóng vai trò trung gian trong luồng:

**Customer / Kitchen / Manager → API Gateway → Lambda → DynamoDB**

Các route cụ thể sẽ được cấu hình trong bước tiếp theo.
