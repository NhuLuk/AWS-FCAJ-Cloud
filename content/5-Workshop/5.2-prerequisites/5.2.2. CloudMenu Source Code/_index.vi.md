---
title: "Mã nguồn CloudMenu"
date: 2026-06-22
weight: 2
chapter: false
pre: "<b>5.2.2. </b>"
---

## 5.2.2. Mã nguồn CloudMenu

Trước khi triển khai lên AWS, cần chuẩn bị mã nguồn của hệ thống CloudMenu.

CloudMenu bao gồm phần frontend phục vụ các nhóm người dùng khác nhau và phần backend thực hiện xử lý nghiệp vụ, giao tiếp với cơ sở dữ liệu.

### Frontend

Frontend của CloudMenu cung cấp ba giao diện chính:

- **Customer Interface:** Cho phép khách hàng truy cập bằng mã QR của bàn, xem menu, lựa chọn món, gửi đơn hàng và theo dõi trạng thái đơn.
- **Kitchen Interface:** Cho phép nhân viên bếp xem các đơn hàng và cập nhật trạng thái xử lý.
- **Manager Dashboard:** Cho phép quản lý theo dõi đơn hàng, doanh thu và các thông tin thống kê của hệ thống.

Các tài nguyên frontend sau khi được chuẩn bị để triển khai sẽ được upload lên Amazon S3 và phân phối đến người dùng thông qua Amazon CloudFront.

![CloudMenu Customer Interface](/images/5-Workshop/5.2/customer-interface.png)

### Backend

Backend của CloudMenu được xây dựng theo mô hình Serverless. Các request từ frontend được gửi đến Amazon API Gateway và chuyển đến AWS Lambda để xử lý.

Lambda thực hiện các nghiệp vụ chính như:

- Tạo đơn hàng.
- Lấy danh sách đơn hàng.
- Đọc thông tin đơn hàng.
- Cập nhật trạng thái đơn hàng.
- Truy xuất dữ liệu cần thiết cho các giao diện của hệ thống.

Dữ liệu được Lambda đọc và ghi trên Amazon DynamoDB.

Luồng xử lý backend có thể được mô tả như sau:

**Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

Việc chuẩn bị đầy đủ mã nguồn frontend và backend trước khi tạo tài nguyên AWS giúp quá trình triển khai ở các bước tiếp theo được thực hiện thuận lợi hơn.