---
title: "Các dịch vụ AWS sử dụng"
date: 2026-06-22
weight: 3
chapter: false
pre: "<b>5.2.3. </b>"
---

## 5.2.3. Các dịch vụ AWS sử dụng

CloudMenu sử dụng nhiều dịch vụ AWS để triển khai các thành phần frontend, backend và lưu trữ dữ liệu.

Các dịch vụ chính được sử dụng trong Workshop gồm:

| Dịch vụ AWS | Vai trò trong CloudMenu |
| :--- | :--- |
| **Amazon S3** | Lưu trữ các tệp frontend và tài nguyên tĩnh của ứng dụng. |
| **Amazon CloudFront** | Phân phối frontend từ S3 đến người dùng thông qua CDN. |
| **Amazon API Gateway** | Cung cấp API endpoint và tiếp nhận HTTP request từ frontend. |
| **AWS Lambda** | Xử lý logic nghiệp vụ của backend mà không cần duy trì máy chủ ứng dụng. |
| **Amazon DynamoDB** | Lưu trữ dữ liệu đơn hàng và các dữ liệu cần thiết của hệ thống. |
| **AWS IAM** | Kiểm soát quyền truy cập của Lambda và các thành phần AWS liên quan. |

Các dịch vụ được kết hợp theo hai luồng chính.

### Luồng phân phối frontend

**User → Amazon CloudFront → Amazon S3**

Amazon S3 lưu trữ các tài nguyên frontend, trong khi Amazon CloudFront đóng vai trò phân phối nội dung đến trình duyệt hoặc thiết bị của người dùng.

### Luồng xử lý dữ liệu

**Frontend → Amazon API Gateway → AWS Lambda → Amazon DynamoDB**

Frontend gửi HTTP request đến API Gateway. Request sau đó được chuyển đến Lambda để xử lý nghiệp vụ. Lambda đọc hoặc ghi dữ liệu trên DynamoDB và trả kết quả về cho frontend.

IAM được sử dụng để cấp các quyền cần thiết cho Lambda khi truy cập DynamoDB và các tài nguyên AWS liên quan.

![Kiến trúc AWS CloudMenu](/images/AWS_CloudMenu.png)

Sau khi hoàn thành các bước chuẩn bị trên, có thể bắt đầu tạo các tài nguyên AWS và triển khai từng thành phần của CloudMenu.