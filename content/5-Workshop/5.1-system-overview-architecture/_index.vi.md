---
title: "Tổng quan và kiến trúc hệ thống"
date: 2026-06-22
weight: 1
chapter: false
pre: "<b>5.1. </b>"
---

## 5.1. Tổng quan và kiến trúc hệ thống

### Tổng quan

CloudMenu là hệ thống gọi món trực tuyến tại bàn sử dụng mã QR, cho phép khách hàng truy cập giao diện gọi món và thực hiện đặt món trực tiếp bằng điện thoại mà không cần sử dụng menu giấy hoặc thiết bị gọi món chuyên dụng.

Mỗi bàn trong nhà hàng được gán một mã QR riêng. Khi khách hàng quét mã QR, hệ thống sử dụng thông tin từ mã để xác định số bàn. Khách hàng sau đó có thể xem menu, tìm kiếm và lựa chọn món, thêm món vào giỏ hàng, gửi đơn gọi món và theo dõi trạng thái xử lý của đơn hàng.

Hệ thống cung cấp ba giao diện chính:

- **Customer Interface:** Giao diện dành cho khách hàng để truy cập bằng mã QR, xem menu, lựa chọn món, gửi đơn hàng và theo dõi trạng thái đơn.
- **Kitchen Interface:** Giao diện dành cho nhân viên bếp để xem các đơn hàng, tiếp nhận đơn và cập nhật trạng thái chế biến.
- **Manager Dashboard:** Giao diện dành cho quản lý để theo dõi số lượng đơn hàng, doanh thu, trạng thái đơn và các thông tin thống kê của hệ thống.

### Kiến trúc

![Sơ đồ kiến trúc AWS CloudMenu](/images/AWS_CloudMenu.png)

CloudMenu được triển khai theo kiến trúc Serverless trên AWS. Các tệp frontend được lưu trữ trên Amazon S3 và phân phối đến người dùng thông qua Amazon CloudFront. Khi người dùng thực hiện các thao tác nghiệp vụ, frontend gửi HTTP request đến Amazon API Gateway. API Gateway chuyển request đến AWS Lambda để xử lý logic nghiệp vụ. Lambda thực hiện đọc hoặc ghi dữ liệu trên Amazon DynamoDB và trả kết quả về frontend thông qua API Gateway.

AWS Identity and Access Management (IAM) được sử dụng để kiểm soát quyền truy cập của Lambda đến DynamoDB và các tài nguyên AWS cần thiết.

Kiến trúc này giúp CloudMenu không cần duy trì một máy chủ backend hoạt động liên tục, đồng thời phân tách rõ các thành phần frontend, API, xử lý nghiệp vụ và lưu trữ dữ liệu.

Trong tương lai, hệ thống có thể được mở rộng thêm các thành phần như Amazon Cognito để xác thực và phân quyền người dùng, AWS WAF để tăng cường bảo vệ ứng dụng, quy trình CI/CD để tự động hóa quá trình triển khai và các cơ chế monitoring phù hợp khi hệ thống được vận hành ở quy mô lớn hơn.