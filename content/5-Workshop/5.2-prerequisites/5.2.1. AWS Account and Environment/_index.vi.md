---
title: "Tài khoản AWS và môi trường"
date: 2026-06-22
weight: 1
chapter: false
pre: "<b>5.2.1. </b>"
---

## 5.2.1. Tài khoản AWS và môi trường

### Tài khoản AWS

Để thực hiện Workshop, cần có tài khoản AWS và quyền truy cập vào AWS Management Console.

Các dịch vụ được sử dụng trong quá trình triển khai CloudMenu gồm:

- Amazon DynamoDB
- AWS Lambda
- Amazon API Gateway
- Amazon S3
- Amazon CloudFront
- AWS Identity and Access Management (IAM)

Sau khi đăng nhập vào AWS Management Console, có thể sử dụng thanh tìm kiếm ở phía trên giao diện để truy cập từng dịch vụ cần thiết.

![AWS Management Console](/images/5-Workshop/5.2/aws-console.png)

### AWS Region

Các tài nguyên có phạm vi Region trong Workshop cần được triển khai nhất quán trong cùng Region để thuận tiện cho quá trình cấu hình và kết nối giữa các dịch vụ.

Trước khi tạo tài nguyên, kiểm tra Region đang được lựa chọn trên AWS Management Console và sử dụng Region phù hợp trong suốt quá trình triển khai.

### Trình duyệt và thiết bị kiểm thử

Cần chuẩn bị một trình duyệt Web để:

- Truy cập AWS Management Console.
- Kiểm tra frontend sau khi triển khai.
- Gửi các request đến backend thông qua giao diện CloudMenu.
- Kiểm tra giao diện Customer, Kitchen và Manager Dashboard.

Đối với chức năng truy cập theo mã QR, có thể sử dụng điện thoại để quét QR và kiểm thử giao diện Customer trên thiết bị di động.