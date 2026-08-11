---
title: "Worklog Tuần 5"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu Tuần 5

- Tìm hiểu Amazon API Gateway và vai trò của API trong kiến trúc Serverless.
- Nắm được các phương thức HTTP cơ bản và cách xây dựng REST API.
- Thực hành tích hợp Amazon API Gateway với AWS Lambda.
- Tìm hiểu và cấu hình CORS để frontend có thể giao tiếp với backend của hệ thống CloudMenu.

**Thời gian:** 20/07/2026 - 24/07/2026

---

### Tổng quan Nhiệm vụ Tuần

| Ngày | Hoạt động | Ngày bắt đầu | Ngày kết thúc | Tài liệu tham khảo |
| ---- | --------- | ------------ | ------------- | ------------------ |
| 1 | - Tìm hiểu **Amazon API Gateway** <br> + Tìm hiểu vai trò của API Gateway trong kiến trúc Serverless <br> + Tìm hiểu khái niệm REST API <br> + Tìm hiểu luồng giao tiếp giữa Client, API Gateway và Backend | 20/07/2026 | 20/07/2026 | [https://aws.amazon.com/api-gateway/](https://aws.amazon.com/api-gateway/) |
| 2 | - Tìm hiểu các phương thức **HTTP** <br> + Phân biệt GET, POST, PUT và OPTIONS <br> + Tìm hiểu Resource, Method, Request và Response <br> + Thiết kế các API cần thiết cho chức năng xử lý đơn hàng | 21/07/2026 | 21/07/2026 | [https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) |
| 3 | - Thực hành tích hợp **Amazon API Gateway với AWS Lambda** <br> + Tạo API Resource và Method <br> + Kết nối API với Lambda Function <br> + Kiểm thử request và response giữa API Gateway và Lambda | 22/07/2026 | 22/07/2026 | [https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started-with-rest-apis.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started-with-rest-apis.html) |
| 4 | - Xây dựng API xử lý đơn hàng cho **CloudMenu** <br> + Cấu hình `POST /order` để tạo đơn hàng <br> + Cấu hình `GET /orders` để lấy danh sách đơn hàng <br> + Cấu hình `PUT /orders/{orderId}` để cập nhật trạng thái đơn <br> + Kiểm thử luồng API Gateway → Lambda → DynamoDB | 23/07/2026 | 23/07/2026 | [https://docs.aws.amazon.com/apigateway/latest/developerguide/](https://docs.aws.amazon.com/apigateway/latest/developerguide/) |
| 5 | - Tìm hiểu và cấu hình **CORS** <br> + Tìm hiểu Same-Origin Policy và Cross-Origin Request <br> + Cấu hình các HTTP Method và Header cần thiết <br> + Kết nối frontend với API Gateway <br> + Kiểm thử luồng Frontend → API Gateway → Lambda → DynamoDB | 24/07/2026 | 24/07/2026 | [https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-cors.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-cors.html) |

---

### Thành tựu Tuần 5

- Hiểu được vai trò của Amazon API Gateway trong kiến trúc Serverless.
- Nắm được các phương thức HTTP cơ bản và cách xây dựng REST API.
- Thực hành tích hợp Amazon API Gateway với AWS Lambda.
- Xây dựng các API `POST /order`, `GET /orders` và `PUT /orders/{orderId}` phục vụ xử lý đơn hàng của CloudMenu.
- Hiểu và cấu hình CORS để frontend có thể gửi request đến API Gateway.
- Kiểm thử thành công luồng xử lý dữ liệu từ API Gateway đến Lambda và Amazon DynamoDB.
- Hoàn thiện nền tảng backend API để tích hợp với giao diện CloudMenu trong các tuần tiếp theo.