---
title: "Worklog Tuần 7"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu Tuần 7

- Hoàn thiện việc tích hợp các thành phần của hệ thống CloudMenu trên nền tảng AWS.
- Triển khai frontend lên Amazon S3 và phân phối nội dung thông qua Amazon CloudFront.
- Kết nối frontend với Amazon API Gateway, AWS Lambda và Amazon DynamoDB.
- Kiểm thử toàn bộ quy trình gọi món từ khách hàng đến bếp và xử lý các lỗi phát sinh.

**Thời gian:** 03/08/2026 - 07/08/2026

---

### Tổng quan Nhiệm vụ Tuần

| Ngày | Hoạt động | Ngày bắt đầu | Ngày kết thúc | Tài liệu tham khảo |
| ---- | --------- | ------------ | ------------- | ------------------ |
| 1 | - Hoàn thiện các thành phần của hệ thống **CloudMenu** <br> + Kiểm tra giao diện khách hàng và giao diện Kitchen <br> + Kiểm tra các Lambda Function và API đã xây dựng <br> + Kiểm tra cấu trúc dữ liệu trong bảng `CloudMenuOrders` | 03/08/2026 | 03/08/2026 | - |
| 2 | - Triển khai frontend CloudMenu lên **Amazon S3** <br> + Upload các file HTML, CSS, JavaScript và hình ảnh <br> + Kiểm tra cấu trúc và đường dẫn của các file frontend <br> + Cấu hình Amazon S3 làm Origin cho CloudFront | 04/08/2026 | 04/08/2026 | [https://aws.amazon.com/s3/](https://aws.amazon.com/s3/) |
| 3 | - Phân phối frontend thông qua **Amazon CloudFront** <br> + Cấu hình CloudFront Distribution <br> + Truy cập CloudMenu thông qua CloudFront Domain <br> + Thực hiện CloudFront Invalidation sau khi cập nhật frontend <br> + Kiểm tra giao diện trên máy tính và điện thoại | 05/08/2026 | 05/08/2026 | [https://aws.amazon.com/cloudfront/](https://aws.amazon.com/cloudfront/) |
| 4 | - Tích hợp các thành phần của hệ thống **CloudMenu** <br> + Kết nối frontend với Amazon API Gateway <br> + Kiểm tra luồng API Gateway → AWS Lambda → Amazon DynamoDB <br> + Kiểm tra CORS và xử lý các lỗi khi frontend gọi API <br> + Kiểm tra dữ liệu đơn hàng được lưu trong DynamoDB | 06/08/2026 | 06/08/2026 | [https://aws.amazon.com/api-gateway/](https://aws.amazon.com/api-gateway/) |
| 5 | - Kiểm thử toàn bộ hệ thống **CloudMenu** <br> + Quét QR để truy cập đúng bàn <br> + Chọn món và gửi đơn từ giao diện khách hàng <br> + Kiểm tra đơn hàng trên giao diện Kitchen <br> + Cập nhật trạng thái `PENDING` → `PREPARING` → `COMPLETED` <br> + Kiểm tra trạng thái đơn hiển thị lại cho khách hàng | 07/08/2026 | 07/08/2026 | - |

---

### Thành tựu Tuần 7

- Hoàn thiện việc tích hợp các thành phần chính của hệ thống CloudMenu.
- Triển khai frontend lên Amazon S3 và phân phối website thông qua Amazon CloudFront.
- Kết nối thành công frontend với Amazon API Gateway, AWS Lambda và Amazon DynamoDB.
- Kiểm tra và xử lý các vấn đề liên quan đến CORS và kết nối giữa frontend với backend.
- Kiểm thử thành công chức năng nhận diện bàn thông qua QR Code và tham số URL.
- Hoàn thiện luồng xử lý đơn hàng từ khách hàng gửi đơn đến bếp tiếp nhận, chế biến và hoàn thành.